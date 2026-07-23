+++
title = "SIEM Query Languages"
date = 2025-02-15T15:29:12+01:00
lastmod = 2026-07-23T00:00:00+02:00
weight = 30
chapter = false
+++

{{% notice style="info" title="Illustrative queries" %}}
Queries on this page are illustrative starting points. They demonstrate investigation logic, not production-ready detections. Table availability, field names, action types, parsing, retention, and normal behaviour vary by environment. Inspect raw records and validate every query against your local schema and telemetry before relying on the result.
{{% /notice %}}

Query syntax changes from platform to platform. The investigation method does not. I normally begin with a small time window and a narrow question, inspect raw records, confirm the schema, and only then aggregate or turn the query into a detection.

{{% notice style="info" title="Two different KQLs" %}}
Microsoft's **Kusto Query Language** and Elastic's **Kibana Query Language** share the abbreviation KQL, but they are different languages. Kusto KQL is a full analytics language. Kibana KQL is primarily a filter syntax.
{{% /notice %}}

## The portable hunting pattern

Most useful hunting queries follow the same progression:

1. Select the data source and constrain time.
2. Filter on the behaviour you want to test.
3. Project the fields needed to understand individual records.
4. Normalise inconsistent fields only after inspecting the source.
5. Aggregate by a meaningful entity: user, host, process, IP address, or session.
6. Retain timestamps and examples so the summary can be pivoted back to evidence.
7. Test assumptions against known-good activity before operationalising the query.

The examples below express the same rough question: *which accounts produced repeated failed sign-ins?* Field names and event semantics are illustrative. Your parser, product version, and data source decide what is actually available.

## Microsoft Kusto Query Language

Kusto Query Language is used in Microsoft Sentinel, Microsoft Defender XDR advanced hunting, Azure Data Explorer, Azure Monitor, and Microsoft Fabric Real-Time Intelligence. Data flows through pipe-separated tabular operators.

```sql
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID == 4625
| summarize Failures=count(),
            FirstSeen=min(TimeGenerated),
            LastSeen=max(TimeGenerated),
            SourceIPs=make_set(IpAddress, 10)
    by TargetAccount, Computer
| where Failures >= 10
| order by Failures desc
```

Useful tabular operators include `where`, `project`, `extend`, `summarize`, `join`, `union`, and `mv-expand`. `parse_json()` is a scalar function, while `arg_max()` and `make_set()` are aggregation functions commonly used with `summarize`.

A high count is a lead, not a conclusion. Scheduled jobs, stale credentials, health checks, and noisy applications can create the same shape.

Reference: [Kusto Query Language overview](https://learn.microsoft.com/en-us/kusto/query/)

## Splunk SPL and SPL2

Splunk's Search Processing Language transforms search results through pipe-separated commands. SPL remains common in operational environments; Splunk also documents SPL2 for products and workflows that support it.

```sql
index=windows EventCode=4625 earliest=-24h
| stats count AS failures
        min(_time) AS first_seen
        max(_time) AS last_seen
        values(src_ip) AS source_ips
    BY user host
| where failures >= 10
| sort - failures
```

Common commands include `search`, `fields`, `eval`, `rex`, `stats`, `eventstats`, `streamstats`, `transaction`, `lookup`, and `timechart`.

Be deliberate with `transaction`: it can be expensive and can hide which join condition actually matters. In many hunts, `stats`, time bucketing, and explicit identifiers are easier to reason about.

References: [SPL search language](https://help.splunk.com/en/splunk-enterprise/search/search-manual/search-overview/about-the-search-language) and [SPL2 overview](https://help.splunk.com/en/splunk-cloud-platform/search/spl2-overview/what-is-spl2)

## Elastic ES|QL

ES|QL is Elastic's piped language for filtering, transforming, and analysing Elasticsearch data.

```sql
FROM logs-windows.*
| WHERE event.code == "4625" AND @timestamp > NOW() - 24 hours
| STATS failures = COUNT(*),
        first_seen = MIN(@timestamp),
        last_seen = MAX(@timestamp)
    BY user.name, host.name
| WHERE failures >= 10
| SORT failures DESC
```

ES|QL is not the same as Kibana Query Language. Elastic KQL is useful for filtering in Kibana:

```sql
event.code: "4625" and user.name: *
```

Kibana KQL does not itself provide the complete aggregation pipeline shown above.

References: [ES|QL](https://www.elastic.co/docs/reference/query-languages/esql) and [Kibana Query Language](https://www.elastic.co/docs/reference/query-languages/kql)

## OpenSearch Query DSL and PPL

OpenSearch supports several interfaces, including JSON Query DSL, SQL, and the pipe-based Piped Processing Language (PPL). Do not assume that a Kibana KQL query can be pasted into OpenSearch unchanged.

```sql
search earliest=-24h `event.code`="4625" source=windows-logs
| stats count() as failures,
        min(`@timestamp`) as first_seen,
        max(`@timestamp`) as last_seen
    by `user.name`, `host.name`
| where failures >= 10
| sort - failures
```

For API-driven searches, Query DSL remains important because it exposes the underlying Boolean, range, aggregation, and full-text primitives.

References: [OpenSearch SQL and PPL](https://docs.opensearch.org/latest/sql-and-ppl/) and [PPL search command and time modifiers](https://docs.opensearch.org/latest/sql-and-ppl/ppl/commands/search/)

## IBM QRadar AQL

Ariel Query Language uses a SQL-like form over QRadar's event and flow stores.

```sql
SELECT username,
       destinationhostname,
       COUNT(*) AS failures,
       MIN(starttime) AS first_seen,
       MAX(starttime) AS last_seen
FROM events
WHERE (
    QIDNAME(qid) ILIKE '%failed%log%on%'
    OR QIDNAME(qid) ILIKE '%login%failed%'
)
GROUP BY username, destinationhostname
HAVING COUNT(*) >= 10
ORDER BY failures DESC
LAST 24 HOURS
```

In production, prefer stable event identifiers, categories, and tested custom properties over a broad text match on `QIDNAME()`. Verify whether the event is normalised as expected before trusting the aggregation.

Reference: [QRadar advanced search with AQL](https://www.ibm.com/docs/en/qradar-on-cloud?topic=searches-advanced-search-options)

## Sigma as a portable detection format

Sigma is not a SIEM query language. It is a vendor-neutral detection format expressed in YAML. Converters and processing pipelines render Sigma logic into backend-specific query languages such as Kusto KQL, SPL, ES|QL, Lucene-based queries, or AQL.

```text
Sigma rule
    -> processing pipeline and field mapping
    -> target backend
    -> Kusto KQL, SPL, ES|QL, Lucene, AQL, or another query form
```

You can convert rules with the [Sigma browser converter](https://sigmahq.io/docs/digging-deeper/convert.html), run conversions locally with [`sigma-cli`](https://sigmahq.io/docs/), or integrate [`pySigma`](https://github.com/SigmaHQ/pySigma) into a detection pipeline. Available targets depend on installed and maintained backends.

```yaml
title: Windows Logon Failure
id: 21e4f8bf-28d1-4d43-a38d-1b5c48b63c4e
status: test
description: Detects a Windows Security logon failure for backend conversion and local tuning.
author: Roger C.B. Johnsen
date: 2026-07-23
modified: 2026-07-23
logsource:
  product: windows
  service: security
detection:
  selection:
    EventID: 4625
  condition: selection
falsepositives:
  - Mistyped passwords
  - Stale service or scheduled-task credentials
level: low
```

A converter can cast this selection into backend-specific syntax. That makes Sigma useful for sharing detection ideas, migrating between platforms, comparing implementations, and rapidly creating a query to test during a hunt.

{{% notice style="warning" title="Conversion is not validation" %}}
The generated query is a starting point, not a production-ready detection. A backend cannot know whether your local parser uses `EventID`, `event.code`, or another field; whether the data source is enabled; how identities are normalised; or which legitimate activity belongs in the baseline.
{{% /notice %}}

Review at least these layers after conversion:

| Layer | What to validate |
| --- | --- |
| Log source | Does the rule describe telemetry you actually collect? |
| Field mapping | Do Sigma fields map to the correct local schema and types? |
| Pipeline | Were product, service, and category transformations applied? |
| Backend | Does the target support every modifier, correlation, and aggregation used? |
| Time and grouping | Are the window, entity key, threshold, and sequence preserved? |
| Evidence | Does the result retain enough fields to investigate a match? |
| Baseline | What legitimate behaviour produces the same selection? |
| Performance | Is the generated query safe and efficient at your data volume? |

The small rule above deliberately describes only an event selection. Repeated failures require correlation or aggregation: account, source, destination, logon type, threshold, and time window all affect what the detection means. Some of that logic may be expressed in Sigma correlation rules; some may still need platform-specific implementation and testing.

References: [About Sigma and its converters](https://sigmahq.io/docs/guide/about), [Sigma specification](https://sigmahq.io/sigma-specification/), and the [SigmaHQ rule repository](https://github.com/SigmaHQ/sigma).

## What makes a query reusable

| Item | Why it matters |
| --- | --- |
| Hypothesis | Prevents a convenient filter from becoming the question |
| Data source and schema | Makes hidden field assumptions visible |
| Time field and timezone | Avoids misleading sequences and gaps |
| Entity key | Defines what is counted or correlated |
| Known exclusions | Records environmental knowledge without silently hiding it |
| Expected false positives | Helps another hunter challenge the result |
| Validation date | Signals when the query was last tested |
| Evidence fields | Preserves the path from aggregation to raw records |

## Revision

| Revised Date | Comment |
| --- | --- |
| 2025-02-15 | Added page |
| 2026-07-23 | Updated product coverage, Sigma conversion guidance, and the portable hunting workflow |
