---
title: "Ingesting Evtx"
date: 2024-08-05T20:01:02+02:00
draft: true
---


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

## Download Winlogbeat

* https://opensearch.org/docs/latest/tools/
* https://www.elastic.co/downloads/past-releases/winlogbeat-oss-7-12-1

## Install Winlogbeat

We are goint to download the "Windows ZIP 64-bit" version and run it from a Windows machine. Winlogbeat is intended to be installed as a service, but we'll use it to ship Windows logs adhoc from the command line. 

Download said file to ```C:\WinlogBeat``` or any suitable place, then unzip.

## Configure Winlogbeat

Create a Winlogbeat configuration file (winlogbeat.yml) with the following content: 

```yaml
winlogbeat.event_logs:
  - name: ${EVTX_FILE} 
    no_more_events: stop

winlogbeat.shutdown_timeout: 10s 
winlogbeat.registry_file: evtx-registry.yml 
output.elasticsearch.hosts: ['http://127.0.0.1:9200']
```

## Shipping logs to OpenSearch

Run the following command in Windows CMD.exe:

```
C:\Winlogbeat>winlogbeat-7.12.1-windows-x86_64\winlogbeat.exe -e -c "c:\Winlogbeat\winlogbeat.yml" -E EVTX_FILE="c:\Winlogbeat\sikkerhet.evtx" > test.txt
```

If all goes good, you should have several "winlogbeat-*" indices under "Index Management". 

## Create Index Alias 

For us to make use of these in "Discovery", we must set up Index Alias for these.