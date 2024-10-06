---
title: Ingesting with Filebeat  
date: 2024-08-05T20:01:02+02:00  
draft: False
weight: 5
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Improved formatting and wording | 

## Introduction

**Filebeat is a lightweight log shipper designed to forward and centralize various types of logs. As part of the Elastic Stack (ELK Stack), Filebeat is specifically tailored to collect logs and send them to Elasticsearch, Logstash, or third-party services for analysis and visualization. When we refer to Filebeat as a "shipper," we mean it's a tool that takes your logs and sends them to a SIEM, to put it simply.**

## Obtaining and Installing Filebeat

1. Download the **_WINDOWS ZIP 64-BIT_** version of Filebeat OSS 7.12.1 from [this link](https://www.elastic.co/downloads/past-releases/filebeat-oss-7-12-1).
2. Create a folder at `C:\Filebeat`.
3. Move the downloaded ZIP archive to the `C:\Filebeat` folder and extract it.

After extraction, the full path to the `filebeat.exe` binary will be:
```plaintext
C:\Filebeat\filebeat-oss-7.12.1-windows-x86_64\filebeat-7.12.1-windows-x86_64\filebeat.exe
```

However, using this long path to run Filebeat can be cumbersome. To simplify this, let's do some initial housekeeping. In PowerShell, move the inner folder to `C:\Filebeat\filebeat` with the following command:

```powershell
mv .\filebeat-oss-7.12.1-windows-x86_64\filebeat-7.12.1-windows-x86_64 .\filebeat
```

This approach allows us to place the logs we want to ingest in the `C:\Filebeat` folder (hrough subfolders), making it easier to run the Filebeat binary from a nearby directory via the command line.

## Helpful Information for Working with Filebeat

### Directory Structure

| Path | Type | Description |
| ---- | ---- | ----------- |
| `C:\Filebeat\filebeat\modules.d` | Folder | Contains YAML configuration files for each module. By default, these files have a `.disabled` extension. You must enable each module you want to use. |
| `C:\Filebeat\filebeat\filebeat.yml` | File | The main Filebeat configuration file, where you set credentials and hostname for connecting to OpenSearch, and other settings like processors |
| `C:\Filebeat\filebeat\data` | Folder | Stores data that Filebeat uses to track what has been ingested. If logs stop appearing in OpenSearch, try deleting this folder and re-ingesting. |

### Logs Supported by Filebeat Out of the Box

Filebeat OSS 7.12.1 supports the following log sources out of the box:

| Module | Description |
| ------ | ----------- |
| apache | Parses access and error logs created by the Apache HTTP server. |
| auditd | Collects and parses logs from the audit daemon (auditd). |
| haproxy | Collects and parses logs from an HAProxy process. |
| icinga | Parses the main, debug, and startup logs of Icinga. |
| iis | Parses access and error logs created by the Internet Information Services (IIS) HTTP server. |
| kafka | Collects and parses logs created by Kafka. |
| logstash | Parses regular logs and the slow log from Logstash, supporting both plain text and JSON formats. |
| mongodb | Collects and parses logs created by MongoDB. |
| mysql | Collects and parses slow logs and error logs created by MySQL. |
| nginx | Parses access and error logs created by the Nginx HTTP server. | 
| osquery | Collects and decodes result logs written by `osqueryd` in JSON format. |
| pensando | Parses distributed firewall logs created by the Pensando Distributed Services Card (DSC). |
| postgresql | Collects and parses logs created by PostgreSQL. |
| redis | Parses logs and slow logs created by Redis. |
| santa | Collects and parses logs from Google Santa, a macOS security tool that monitors process executions and can blacklist/whitelist binaries. | 
| system | Collects and parses logs created by the system logging service of common Unix/Linux-based distributions. |
| traefik | Parses access logs created by Træfik. |

### Enabling a Log Module

To enable a log module, refer to the table "_Logs Supported by Filebeat Out of the Box_." For example, to enable the Apache module, run the following commands in PowerShell:

```powershell
cd C:\Filebeat\filebeat
.\filebeat.exe modules enable apache
```

{{% notice warning %}}
You must run this command from the `C:\Filebeat\filebeat` folder, or you may encounter path and settings issues.
{{% /notice %}}

{{% notice info %}}
You only need to enable each log module once.
{{% /notice %}}

### Configuring a Log Module

Continuing with the Apache example, you'll need to set up the Apache module. First, create a folder in `C:\Filebeat` for the Apache logs:

```powershell
mkdir C:\Filebeat\apache
```

This folder will hold all Apache logs you wish to ingest. Next, instruct Filebeat where to find your logs by setting the `var.paths` directive in `C:\Filebeat\filebeat\modules.d\apache.yml`:

```yaml
# Module: apache
# Docs: https://www.elastic.co/guide/en/beats/filebeat/7.x/filebeat-module-apache.html

- module: apache
  # Access logs
  access:
    enabled: true

    # Set custom paths for the log files. If left empty,
    # Filebeat will choose the paths depending on your OS.
    var.paths: [ 
        "C:\\Filebeat\\apache\\*",
      ] 

  # Error logs
  error:
    enabled: true

    # Set custom paths for the log files. If left empty,
    # Filebeat will choose the paths depending on your OS.
    #var.paths:
```

### Instructing Filebeat to Connect to OpenSearch

In the main Filebeat configuration file (`C:\Filebeat\filebeat\filebeat.yml`), locate the "Elasticsearch Output" section and modify it as follows:

```yaml
output.elasticsearch:
  hosts: ["http://127.0.0.1:9200"]
  username: "hunter"
  password: "hunter" 
```

If using HTTPS, please consider the following:

```yaml
output.elasticsearch:
  hosts: ["https://127.0.0.1:9200"]
  username: "" # Change this
  password: "" # Change this
output.elasticsearch.ssl.verification_mode: none
```

### Removing Unnecessary Metadata from Filebeat Parsing

By default, Filebeat adds host, client, and agent metadata to log entries. This information is often unnecessary when analyzing Apache logs and can clutter the data. To remove this metadata (optional), update the "processors" section in the `C:\Filebeat\filebeat\filebeat.yml` file:

```yaml
processors:
  - add_host_metadata:
      when.not.contains.tags: forwarded
  - add_cloud_metadata: ~
  - add_docker_metadata: ~
  - add_kubernetes_metadata: ~
  - drop_fields:
      fields: ["host", "client", "agent"] # This line is central. Add sectiosn as sees fit for your use.
```

### Enabling OpenSearch Compatibility

{{% notice info %}}
If you have already completed this step in other chapters, you can skip it.
{{% /notice %}}

Historically, many popular agents and ingestion tools, such as Beats, Logstash, Fluentd, FluentBit, and OpenTelemetry, have worked with Elasticsearch OSS. OpenSearch aims to support a broad range of these tools, but not all have been tested or explicitly marked as compatible. As an intermediate solution, OpenSearch provides a setting that instructs the cluster to return version 7.10.2 instead of its actual version. If you are using clients that perform a version check (such as Logstash OSS or Filebeat OSS versions 7.x - 7.12.x), you can enable this setting:

```json
PUT _cluster/settings
{
  "persistent": {
    "compatibility": {
      "override_main_response_version": true
    }
  }
}
```

To enable this setting, go to the **"Dev Tools"** section (**"Hamburger menu" -> "Management" -> "Dev Tools"**). Paste the above code into the left-side panel, then click the "Click to send request"/"Play" button. The status of the request will be displayed in the right panel, like this:

![OpenSearch](/images/opensearch-devtools.png)

## Ingesting data log

Finally, we've arrived at the ingestion step. To ingest the logs in ```C:\Filebeat\apache````, execute the following:

```
cd C:\Filebeat\filebeat
filebeat.exe -e
```

This will create a ```filebeat-7.12.1-2024.08.24``` index in OpenSearch. Note the index is named after which shipper sent data to it - and the current date.

## Create Index Alias 

For us to make use of this new index in "Discovery", we must set up Index Alias for it.

Keep in mind that "Discover" (the main query interface) knows nothing about new indices, thus you can't search into them directly. You first need to create index aliases for "Discover" to see them. You can do so by reaching the "Dashboard Management" utility by going to this path: __"Hamburger menu" -> Management -> "Dashboard Management"__: 

![Manage dashboard 2](/images/manage-dashboard-2.png)

Then select __"Index patterns"__, then click on the button __"Create index pattern"__:

![Manage dashboard 3](/images/manage-dashboard-3.png)

In __"Index pattern name"__ enter a name of choice. Since we now got several logs starting with "logs-", I find it nice to use the name "winlogbeat-*". This will easily match the logs I have an interested in. Keep in mind the name we enter here will show in "Discovery".

![Manage dashboard 4](/images/filebeat-index-pattern.png)

Then point to a time field to use and click save (as mentioned in the "A note on ndjson format").

![Manage dashboard 5](/images/filebeat-index-pattern-time.png)

If we go back to __"Discover"__, we can now see that our __"Index alias"__ is available to us to search in. 

![Manage dashboard 6](/images/filebeat-discovery.png)
