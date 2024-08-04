---
title: "Ingesting Logs"
date: 2024-08-04T09:48:00+02:00
draft: fasle
weight: 2
---

{{% notice info %}}
This public preview offers a glimpse of upcoming content. Please note that the content may be subject to change without prior notice as I continue to develop this site.
{{% /notice %}}

## Introduction

In this section, we will import logs into OpenSearch, a process known as log ingestion. There are various methods to ingest logs into OpenSearch, but we will focus on using the API through a Python script. The script will read a Ndjson log file of your choice and ingest the data into OpenSearch. In this procedure we will work inside the Alma Linux instance, executing the Python script directly.

## Setting up Virtualenv

In Alma, create a work folder in your home directoy and change into it. Then create a virtual environment named "env":

```bash
python3 -m venv env
```

We create a virtual environment in order to not pollute the Python system installed directly on the host. Then, load the virtual environment:

```bash
source env/bin/activate
```

You can see the environment has been loaded by the "(env)" prefix on command line, like so:

```bash
(env) [hunter@localhost Ingester]$ 
```

## Installing necessary Python libraries

Create a file named "requirements.txt", then add the following into it:

```bash
opensearch-py
pendulum
```

Install the requiements by:

```bash
pip install -r requirements.txt
```

## Python bulk indexer

This is the Python script that ingests the log file:

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

This script will parse the Ndjson file and actively locate index names from each row - whereby the script recreated indices found as it tags along parsing. Example:

![Manage dashboard 1](/images/manage-dashboard-1.png)

Keep in mind that "Discover" knows nothing about these indices, thus you can't search into them directly. You first need to create index aliases for "Discover" to see them. You can do so by reaching the "Dashboard Management" utiligy by this path: "Hamburgermenu" -> Management -> "Dashboard Management": 

![Manage dashboard 2](/images/manage-dashboard-2.png)

Then select "Index patterns", then click on the button "Create index pattern":

![Manage dashboard 3](/images/manage-dashboard-3.png)

In "Index pattern name" enter a name of choice. Since we now got several logs starting with "logs-", I find it nice to use the name "logs-*. This will easily match the logs I have an interested in. Keep in mind the name we enter here will show in "Discovery".

![Manage dashboard 4](/images/manage-dashboard-4.png)

Then point top a time field to use and click save.

![Manage dashboard 5](/images/manage-dashboard-5.png)

If we go back to "Discover", we can now see that our "Index alias" is available to us to search in. 

![Manage dashboard 6](/images/manage-dashboard-6.png)
