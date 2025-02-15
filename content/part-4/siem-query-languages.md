---
title: "Siem Query Languages"
date: 2025-02-15T15:29:12+01:00
draft: false
weight: 3
---

| Revised Date | Comment |
| ------------ | ------- |
| 15.02.2025   | Added page | 

## Introduction

Early in my SOC and threat hunting career, I was thrown into the deep end with IBM QRadar. As I recall, the handover was abrupt: "This is our SIEM, QRadar. It's yours now – good luck!" Faced with an unfamiliar system, I quickly realized the immense value of cheat sheets, a concept I'd come to rely on throughout my journey.

Cheat sheets are indispensable tools for navigating complex systems like SIEMs, particularly when you're working with a platform you're unfamiliar with. While they won't magically transform you into an expert, they offer a practical approach to rapidly acquire operational knowledge and build confidence. Think of them as valuable aids that help you dissect intricate SIEMs and accelerate your learning curve.

## What is a Cheat Sheet?

A cheat sheet is a concise and readily accessible reference guide that distills essential information about a specific subject, skill, or tool into an easily digestible format. It provides quick access to key facts, formulas, commands, or best practices, enabling users to efficiently recall and apply knowledge without wading through extensive documentation or relying solely on memory.

In essence, a cheat sheet serves as both a memory aid and a productivity booster. It's an invaluable asset for anyone seeking to quickly grasp the essentials of a topic, overcome learning hurdles, or streamline their workflow. Whether it's a condensed summary of programming syntax, a quick reference for keyboard shortcuts, or a guide to troubleshooting common issues, a cheat sheet empowers users to perform tasks more effectively and with greater confidence.

## Can I Survive on Cheat Sheets Alone?

Absolutely not! And that's the key intention behind this article. Cheat sheets are a fantastic way to gain initial exposure to various technologies, offering a glimpse into their core functionalities. Beyond simply getting you up and running, I believe cheat sheets provide essential reference points when you need to delve into and truly understand the official documentation – which, let's face it, can often be uninspiring to pick up and read.

## Overview of SIEM Query Languages

Cheat sheets are undeniably important. What I wish I had access to when I was starting out was an overview of multiple SIEM query languages, coupled with syntax examples and readily available, useful references. So, here's my attempt to provide just that:

### 1. Kusto Query Language (KQL) - Microsoft Sentinel & Defender

KQL is optimized for efficiently querying structured log data and bears a resemblance to PowerShell in its syntax and logic flow.

**Features**

- Read-only query language with powerful aggregation functions
- Supports time-series analysis and anomaly detection
- Integrated with Microsoft Defender, Sentinel, and other security tools

**Example Query**

```sql
SecurityEvent
| where TimeGenerated > ago(1d)
| where EventID == 4625  // Failed logon attempts
| summarize count() by Account, bin(TimeGenerated, 1h)
| order by count_ desc
```

**Resources**

- [Official Documentation](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)
- [Kusto Detective Agency](https://detective.kusto.io/)
- [KC7 Training](https://kc7cyber.com/)
- [KQL Cheat Sheet](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/get-started-queries)

It's worth noting that Microsoft Sentinel is built on top of Azure Monitor and uses Log Analytics workspaces to store data. Furthermore, Microsoft Defender XDR data can be queried within Microsoft Sentinel when configured correctly, providing a unified view of your security landscape.

### 2. Splunk Processing Language (SPL) - Splunk

SPL is a pipeline-based language specifically designed for flexible log searching and powerful data visualization.

**Features**

- Uses pipelines (`|`) to chain search commands together.
- Supports advanced statistical functions and visualization tools for in-depth analysis.
- Handles both structured and unstructured data with ease.

**Example Query**

```sql
index=security sourcetype=windows_security
| search EventCode=4625
| stats count by user, _time
| sort -count
```

**Resources**

- [Official Documentation](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference)
- [Splunk Beginner Cheatsheet](https://christiant.io/spl-beginner)
- [Splunk SPL cheatsheet](https://gist.github.com/albertzsigovits/49a81b33ffd5ed6d9f588eff0a2902e0)
- [Splunk Cheat Sheet: Search and Query Commands](https://www.stationx.net/splunk-cheat-sheet/)
- [Splunk Cheat Sheet](https://7958885.fs1.hubspotusercontent-na1.net/hubfs/7958885/Downloadable%20Assets/Brochures/QAL/Campaign/Cyber%20Pulse/Cheat%20Sheets/Splunk_Cheat_Sheet.pdf)


### 3. Lucene Query Language / OpenSearch Query DSL - OpenSearch DSL

OpenSearch offers two primary methods for querying data: the Lucene Query Language and its own JSON-based Query DSL. This section focuses on the Query DSL, highlighting its structure and capabilities for OpenSearch. While Lucene is a powerful underlying technology, the Query DSL is the more commonly used and flexible approach for most OpenSearch users.

#### Features:

- Supports full-text search, aggregations, and filtering for comprehensive analysis.
- Designed for structured, programmatic searches using JSON format.
- Enables complex queries with boolean logic, range filters, and more.

#### Example Query (Query DSL):

```json
{
  "query": {
    "bool": {
      "must": [
        { "match": { "event_id": 4625 } }
      ],
      "filter": {
        "range": { "@timestamp": { "gte": "now-1d/d" } }
      }
    }
  }
}
```

#### Resources:

- [OpenSearch Query DSL Documentation](https://opensearch.org/docs/latest/query-dsl/index/)
- [Lucene Query Syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html)
- [OpenSearch Labs & Training](https://opensearch.org/training/)

### 4. Kibana Query Language (KQL) - Elastic Stack / OpenSearch

KQL is natively used within Kibana for Elasticsearch and also seamlessly operates within OpenSearch.

**Features**

- Employs a human-readable syntax with intelligent auto-completion features.
- Significantly simplifies filtering within dashboards, enhancing the user experience.

**Example Query**

```sql
event.category: "authentication" and event.outcome: "failure"
```

**Resources**

- [KQL Documentation](https://www.elastic.co/guide/en/kibana/current/kuery-query.html)
- [Elastic Stack Training](https://www.elastic.co/training/)
- [Kibana Cheat Sheet](https://www.elastic.co/guide/en/kibana/current/kuery-query.html)

### 5. QRadar Query Language (AQL) - IBM QRadar

AQL is IBM QRadar's Ariel Query Language, which adopts a SQL-like structure optimized for searching through security event data.

**Features**

- Employs a familiar SQL-like syntax, making it accessible to many analysts.
- Supports real-time event analysis, enabling immediate threat detection.
- Provides deep filtering capabilities for precise investigation.

**Example Query**

```sql
SELECT LOGSOURCENAME(logsourceid) AS LogSource, username, COUNT(*)
FROM events
WHERE QIDNAME(qid)='Failed Login'
AND starttime >= CURRENT_TIMESTAMP - 86400
GROUP BY LogSource, username
ORDER BY COUNT(*) DESC
```

**Resources**

- [Ariel Query Language Guide](https://www.ibm.com/docs/en/SS42VS_7.4/pdf/b_qradar_aql.pdf)
- [QRadar Labs](https://www.ibm.com/community/qradar/)
- [AQL Cheat Sheet](https://community.ibm.com/community/user/security/discussion/aql-cheat-sheet)
- [Qradar Cheat Sheet](https://github.com/yeknu/Qradar_cheat_sheet)

### 6. Sigma Rules - Vendor-Agnostic Detection Format

Sigma is not a query language itself, but rather a versatile rule format designed to describe security detections in a standardized manner, making them convertible into SIEM-specific queries.

**Features**

- Utilizes a human-readable YAML-based rule format.
- Enables conversion to KQL, SPL, OpenSearch DSL, and numerous other query languages.
- Remains community-driven and extensible, adapting to the ever-changing threat landscape.

#### Example Sigma Rule:

```yaml
title: Failed Logon Attempts
logsource:
  product: windows
  service: security
  category: authentication
selection:
  EventID: 4625
condition: selection
```

**Resources**

- [Sigma Project Repository](https://github.com/SigmaHQ/sigma)
- [Sigma Documentation](https://sigmahq.io/docs/)
- [Sigma Converter Tools](https://uncoder.io/)

## Conclusion

This article isn't designed to make you an overnight expert in any specific SIEM query language. Instead, the intent is to share an approach to quickly and simply gain familiarity with essential languages that you might encounter as a threat hunter. My personal experience with cheat sheets is that they serve as a great way to familiarize myself with essential technologies and quickly understand the fundamentals.

The key takeaway is that while cheat sheets provide a fantastic starting point and ongoing reference, they are a stepping stone to deeper understanding. By using them to grasp the core concepts and then diving into the official documentation, you can effectively build a strong foundation in any SIEM query language and enhance your capabilities as a threat hunter. Embrace cheat sheets as a tool to accelerate your learning and navigate the complex world of cybersecurity with greater confidence and efficiency.