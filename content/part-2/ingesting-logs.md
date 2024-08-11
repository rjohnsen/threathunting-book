---
title: "API Bulk Ingesting Logs"
date: 2024-08-04T09:48:00+02:00
draft: fasle
weight: 2
---

{{% notice info %}}
This public preview offers a glimpse of upcoming content. Please note that the content may be subject to change without prior notice as I continue to develop this site.
{{% /notice %}}

## Introduction

In this section, we will import logs into OpenSearch, a process known as log ingestion. There are various methods to ingest logs into OpenSearch, but we will focus on using the API with a Python script. The script will read an ndjson log file of your choice and ingest the data into OpenSearch. In this procedure, we will work within the Alma Linux instance, executing the Python script locally. Alternatively, this procedure can be executed from another machine as well by changing endpoint address in the Python script.

## Setting up a virtual environment

The first step is to create a sandboxed environment for Python. This is done to avoid polluting the native Python environment in our Alma installation. In Alma, start by creating a working folder in your home directory and navigate into it. Then, create a virtual environment named "env":

```bash
python3 -m venv env
```

When working on a Python project, it’s good practice to create a virtual environment for each project. This will save you a lot of trouble in the long run by preventing conflicts between packages and other issues. Once created, you can activate the environment as follows:

```bash
source env/bin/activate
```

You can see the environment has been loaded by the "(env)" prefix on command line, like so:

```bash
(env) [hunter@localhost Ingester]$ 
```

## Installing necessary Python libraries

To communicate with OpenSearch, we need a Python package that simplifies our work—__opensearch-py__ is the library for that. We also need a package to handle date and time operations more easily, for which we’ll use __pendulum__. Instead of installing each package manually using `pip`, we’ll create a file named `requirements.txt` and add the following lines:

```bash
opensearch-py
pendulum
```

This allows us to install all the required packages at once with the following command:

```bash
pip install -r requirements.txt
```

## Python bulk indexer

Now, with the virtual environment loaded and requirements installed, we can finally start ingesting data. In this chapter we will focus on ingesting a ndjson log using the provided Python script.

### A note on ndjson format

Ndjson (Newline Delimited JSON) is a format for storing or streaming structured data that consists of individual JSON objects separated by newline characters. Each line in an NDJSON file represents a single JSON object, making it easy to process large datasets line by line. This format is particularly useful for log files, data streams, or any situation where you need to handle large amounts of JSON data incrementally.

A log in ndjson format typically looks like this and typically has a ```.ndjson``` file extension: 

```json
[
    { "datetime": "datetime", "key_1": "value", "key_2": "value"},
    { "datetime": "datetime", "key_1": "value", "key_2": "value"},
]
```

Take note of the “datetime” key. Typically, there is a field that indicates when the event occurred. Before ingesting the log, you should identify this field. Later, you will use this field to set up a time mapping in OpenSearch.

### The ingestion script

The following is a Python script that ingests the log file. Pay attention to these variables: 

* hostname
* username
* password

Adjust the hostname to suit your environment, depending on where you’re running the script. Additionally, ensure that the username and password are correctly configured.

```python
from opensearchpy import OpenSearch, helpers

import os
import json
import pendulum
import re
import sys
import copy

# User settings
hostname = "http://localhost:9200" # CHANGE THIS!
username = "hunter"  # CHANGE THIS!
password = "hunter"  # CHANGE THIS!

# Initialize the OpenSearch client
os_client = OpenSearch(
    http_auth=(
        username,
        password
    ),
    retry_on_timeout=True,
    timeout=60
)

NOW = pendulum.now()

# Get the file path from command-line arguments
file_path = sys.argv[1]
print(f"Using logfile: {file_path}")
if os.path.exists(file_path):
    open_search_logs = file_path
else:
    print("Input Correct File Path")
    raise

def set_date_time(json_item_copy):
    split_index = json_item_copy['_index'].split('.ds-')[1]
    parsed_index = re.findall(r'(.*)-\d\d\d\d.\d\d.\d\d-(.*)', split_index)
    index_date = f"{parsed_index[0][0]}"

    # Set Log Time to today or yesterday
    current_timestamp = pendulum.parse(json_item_copy['_source']['@timestamp'])
    try:
        new_timestamp = current_timestamp.set(day=NOW.day - 1, year=NOW.year, month=NOW.month)
    except ValueError:
        new_timestamp = current_timestamp.set(day=NOW.day, year=NOW.year, month=NOW.month)
    return index_date, str(new_timestamp)

def batch_trace_logs(logs):
    i = 0
    count = 0
    for item in logs:
        count += 1
        if i == 10000:
            print(f"Bulk Indexed {count} logs")
            i = 0
        json_item = json.loads(copy.deepcopy(item))
        if 'sysmon' in json_item['_index']:
            new_index, new_timestamp = set_date_time(json_item)
            json_item['_source']['@timestamp'] = new_timestamp
            yield {"_index": new_index, "_source": json_item['_source'], '_op_type': "create"}
            i += 1
        else:
            new_index, new_timestamp = set_date_time(json_item)
            json_item['_source']['@timestamp'] = new_timestamp
            yield {"_index": new_index, "_source": json_item['_source'], '_op_type': "create"}
            i += 1

# Delete Data Streams
print("Deleting Existing Indices")
print(os_client.indices.delete_data_stream(name='*'))

# Bulk indexing
helpers.bulk(os_client, batch_trace_logs(open(open_search_logs)), request_timeout=60)
```

Running it: 

```bash
python3 ingest.py logfile.ndjson
```

This Python script is designed to efficiently ingest logs into OpenSearch. It starts by importing necessary libraries and setting up user-specific configurations such as the OpenSearch server hostname, username, and password. After initializing the OpenSearch client, the script retrieves the log file path from the command-line arguments and verifies its existence.

A key function within the script adjusts the timestamps of log entries to ensure they are recent, setting them to either today or yesterday. The script processes the log entries in batches, adjusting their timestamps and preparing them for bulk indexing. It periodically reports progress to keep the user informed.

Before ingesting new data, the script deletes any existing data streams in OpenSearch, ensuring a clean environment. Finally, the script uses OpenSearch's bulk helper to efficiently index the processed log entries into OpenSearch. This process ensures that the log data is up-to-date and ready for analysis and querying in OpenSearch.

### Important note

This script is not a Swiss army knife solution that parses all log formats on the planet. It is meant as an example on how to use the OpenSearch API - customizations to fit your log formats are expected.

## Making the index aliases

This script will parse the ndjson file and actively locate index names from each row - whereby the script recreated indices found as it tags along parsing. Example:

![Manage dashboard 1](/images/manage-dashboard-1.png)

Keep in mind that "Discover" (the main query interface) knows nothing about these indices, thus you can't search into them directly. You first need to create index aliases for "Discover" to see them. You can do so by reaching the "Dashboard Management" utility by going to this path: __"Hamburger menu" -> Management -> "Dashboard Management"__: 

![Manage dashboard 2](/images/manage-dashboard-2.png)

Then select __"Index patterns"__, then click on the button __"Create index pattern"__:

![Manage dashboard 3](/images/manage-dashboard-3.png)

In __"Index pattern name"__ enter a name of choice. Since we now got several logs starting with "logs-", I find it nice to use the name "logs-*. This will easily match the logs I have an interested in. Keep in mind the name we enter here will show in "Discovery".

![Manage dashboard 4](/images/manage-dashboard-4.png)

Then point tp a time field to use and click save (as mentioned in the "A note on ndjson format").

![Manage dashboard 5](/images/manage-dashboard-5.png)

If we go back to __"Discover"__, we can now see that our __"Index alias"__ is available to us to search in. 

![Manage dashboard 6](/images/manage-dashboard-6.png)
