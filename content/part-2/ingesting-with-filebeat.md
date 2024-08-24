---
title: "Ingesting with Filebeat"
date: 2024-08-05T20:01:02+02:00
draft: false
---

**Filebeat** is a lightweight shipper for forwarding and centralizing various types of logs. It is part of the Elastic Stack (ELK Stack) and is specifically designed to collect logs and forward them to Elasticsearch, Logstash, or a third-party service for further analysis and visualization.

When we say shipper, we mean "a binary that take your logs and sends it to the SIEM" - to put it bluntly.

## Obtaining and installing Filebeat

1. Download the **_WINDOWS ZIP 64-BIT_** version of Filebeat OSS 7.12.1 from [this link](https://www.elastic.co/downloads/past-releases/filebeat-oss-7-12-1).
2. Create a folder at `C:\Filebeat`.
3. Move the downloaded ZIP archive to the `C:\Filebeat` folder and extract it.

After extraction, the full path to the `filebeat.exe` binary should be:
```plaintext
C:\Filebeat\filebeat-oss-7.12.1-windows-x86_64\filebeat-7.12.1-windows-x86_64\filebeat.exe
```

However, using this long path to run Filebeat can be cumbersome. To simplify this, let's do some initial housekeeping.

In PowerShell, move the inner folder to `C:\Filebeat\filebeat` with the following command:
```powershell
mv .\filebeat-oss-7.12.1-windows-x86_64\filebeat-7.12.1-windows-x86_64 .\filebeat
```

The rationale behind this is that we plan to place the logs we want to ingest in the `C:\Filebeat` folder. This way, we can easily run the Filebeat binary from a nearby directory via the command line.

## Helpful things to know working with Filebeat

### Directory structure

| Path | Type | Description |
| ---- | ---- | ----------- |
| C:\Filebeat\filebeat\modules.d | Folder | This directory holds Yaml configuration files for each module. Every Yaml file has initially a file ending of ".disabled". You must enable each module you want to use. |
| C:\Filebeat\filebeat\filebeat.yml | File | Filebeat configuration file. This is where you set credentials and hostname for Filebeat to connect to OpenSearch |
| C:\Filebeat\filebeat\data| Folder | Filebeat store data to keep track on what has been ingested. If you suddenly find that logs doesn't end up in OpenSearch, then delete this folder and try to re-ingest.|

### Logs supported by Filebeat out of the box 

Filebeat OSS 7.12.1 supports the following logsources out of the box (log module):

| Module | Description |
| ------ | ----------- |
| apache | The apache module parses access and error logs created by the Apache HTTP server. |
| auditd | The auditd module collects and parses logs from the audit daemon (auditd). |
| haproxy | The haproxy module collects and parses logs from a (haproxy) process. |
| icinga | The icinga module parses the main, debug, and startup logs of Icinga. |
| iis | The iis module parses access and error logs created by the Internet Information Services (IIS) HTTP server. |
| kafka | The kafka module collects and parses the logs created by Kafka. |
| logstash | The logstash modules parse logstash regular logs and the slow log, it will support the plain text format and the JSON format. |
| mongodb | The mongodb module collects and parses logs created by MongoDB. |
| mysql | The mysql module collects and parses the slow logs and error logs created by MySQL. |
| nginx | The nginx module parses access and error logs created by the Nginx HTTP server. | 
| osquery | The osquery module collects and decodes the result logs written by osqueryd in the JSON format. |
| pensando | The pensando module parses distributed firewall logs created by the Pensando distributed services card (DSC). |
| postgresql | The postgresql module collects and parses logs created by PostgreSQL. |
| redis | The redis module parses logs and slowlogs created by Redis. |
| santa | The santa module collects and parses logs from Google Santa, a security tool for macOS that monitors process executions and can blacklist/whitelist binaries. | 
| system | The system module collects and parses logs created by the system logging service of common Unix/Linux based distributions. |
| traefik | The traefik module parses access logs created by Træfik. |

### Enabling a log module

With basis in the table "_Logs supported by Filebeat out of the box_", we can enable the desired log module like so (by using Apache as an example): 

```powershell
cd C:\Filebeat\filebeat
.\filebeat.exe modules enable apache
```

{{% notice warning %}}
You have to stand in C:\Filebeat\filebeat folder for this to work, or else you will get problems with paths and settings.
{{% /notice %}}

{{% notice info %}}
You only need to enable the log modules once.
{{% /notice %}}

### Configuring a log module

Continuing with our Apache example, we need to set up the Apache module. But first, I like to create a folder in ```C:\Filebeat``` for each log module like so:

```powereshell
mkdir C:\Filebeat\apache
```

This folder will hold all apache logs I will ingest for my hunting purposes. Now I will have to instruct Filebeat where it can pick up my logs. I do so by setting the ```var.paths``` directive in ```C:\Filebeat\fb\modules.d\apache.yml```:

```
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

### Instruct Filebeat how to connect to OpenSearch

In the main configuration file for Filebeat, ```C:\Filebeat\filebeat\filebeat.yml```, we locate the "Elasticsearch Output" and changes the associated variables to:

```
output.elasticsearch:
  hosts: ["http://127.0.0.1:9200"]
  username: "hunter"
  password: "hunter" 
```

### Removing noise from Filebeat parsing

By default Filebeat adds host, client and agent data to the log entry. These are not necessary when looking at Apache logs, they would make the analysis confusing if present. This step is considered optional, if you prefer to have these values present.

In the main configuration file for Filebeat, ```C:\Filebeat\filebeat\filebeat.yml```, we locate the "processors" section and changes the associated variables to:

```
processors:
  - add_host_metadata:
      when.not.contains.tags: forwarded
  - add_cloud_metadata: ~
  - add_docker_metadata: ~
  - add_kubernetes_metadata: ~
  - drop_fields:
      fields: ["host", "client", "agent"]
```
### Enabling OpenSearch compability setting

{{% notice info %}}
If you have already done this step from the other chapters, you can safely skip this step.
{{% /notice %}}


Historically, many multiple popular agents and ingestion tools have worked with Elasticsearch OSS, such as Beats, Logstash, Fluentd, FluentBit, and OpenTelemetry. OpenSearch aims to continue to support a broad set of agents and ingestion tools, but not all have been tested or have explicitly added OpenSearch compatibility. As an intermediate compatibility solution, OpenSearch has a setting that instructs the cluster to return version 7.10.2 rather than its actual version. If you use clients that include a version check, such as versions of Logstash OSS or Filebeat OSS between 7.x - 7.12.x, enable the setting:

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

Enable this setting by going into the __"Dev Tools"__ tool (__"Hamburger menu" -> "Management" -> "Dev Tools"__). Paste the above code into the left side panel, then hit the "Click to send request"/"Play" button. The status from this query is shown in the right panel, like so: 

![OpenSearch](/images/opensearch-devtools.png)

