+++
title = "Understanding Data"
date = 2024-09-15T14:10:39+02:00
lastmod = 2026-07-24T00:00:00+02:00
weight = 10
chapter = false
+++

A threat hunter makes a living from understanding data. All kinds of data.

Understanding data is not the same as recognising a field name or knowing a query language. It means knowing where a record came from, what produced it, what each field represents, which transformations occurred, and what the source cannot tell you.

Only then can we use counts, patterns, rarity, relationships, and anomalies without turning convenient shapes into false conclusions.

{{% notice style="warning" title="Telemetry is a representation" %}}
Telemetry is produced from one system's viewpoint. Individual records may be incomplete, delayed, normalised, sampled, or duplicated, while expected records may be absent. Before interpreting a pattern, understand how the data was generated and collected.
{{% /notice %}}

## Questions to ask before analysing

I find it useful to establish a small source profile for the data source:

| Question | Why it matters |
| --- | --- |
| What generated the record? | Identifies the product, component, sensor, and viewpoint |
| What condition, action, or collection process produces it? | Covers actions, state changes, polling, snapshots, periodic jobs, and aggregates |
| When is the timestamp assigned? | Separates action time, event time, receipt time, and ingestion time |
| What is the unit of one row? | Connections, processes, alerts, sessions, aggregations, and snapshots are not interchangeable |
| Which fields are native? | Distinguishes source evidence from enrichment and parser output |
| What is filtered or sampled? | Defines what absence and volume can mean |
| How long is it retained? | Limits how far back comparisons and baselines can reach |
| Can records be duplicated? | Avoids inflated counts and false sequences |
| Which identifiers are scoped locally? | Prevents unsafe joins on reused process, logon, session, or address values |
| What changed recently? | Parser, policy, sensor, product, and environment changes can create artificial anomalies |

### Null, missing, and absent are not the same

Terminology and representation vary by platform. In this note, I use the following analytical distinctions:

| Condition | What it can mean |
| --- | --- |
| Null field | The record exists, but the field has no value |
| Missing field | The field is not present in this event type, schema version, or parser output |
| Missing event | No matching record was found in the searched source and period |
| Telemetry gap | The source could not provide complete coverage because of health, policy, filtering, sampling, retention, or collection failure |

None of them proves that an action did not occur. Before interpreting absence, verify that the source was capable of observing the action, was healthy at the time, retained the event, and was queried correctly.

Filtering and sampling can reshape a distribution. If a connector begins dropping common events while retaining high-severity records, rare values may appear proportionally more common even though the underlying behaviour has not changed. Cardinality also deserves care: timestamps, random identifiers, inconsistent casing, full command lines, and parser variations can create artificial rarity. Normalise only when the transformation is understood, and keep a path back to the original value.

## Searching with intent

Searching is bread and butter for a threat hunter. We search in order to find, but jumping into a large dataset without a question can consume hours while producing very little understanding.

> You do not simply query into Mordor.
>
> Roger Johnsen

Before searching, write down:

1. The observation or hypothesis that brought you to the data.
2. The entity you are following, such as an account, process, device, session, file, or network relationship.
3. The initial time window and why it is appropriate.
4. The fields needed to inspect individual records.
5. What result would support, weaken, or refute the hypothesis.
6. Which other source could corroborate the finding.

A query can return exactly what you asked for and still answer the wrong question.

## An OODA-inspired hunting workflow

The workflow below is a deliberate hunting adaptation of John Boyd's OODA concept, not a complete representation of Boyd's model. It gives us a useful way to approach an unfamiliar source through Observe, Orient, Decide, and Act. The value is not the acronym itself. The value is accepting that understanding develops through repeated cycles.

{{<mermaid align="center">}}
flowchart LR
    O1[Observe raw records] --> O2[Orient to source and context]
    O2 --> D[Decide the next question]
    D --> A[Act: query, pivot, document]
    A --> O1
{{< /mermaid >}}

### Observe

Start close to the raw records.

| Step | Practical action |
| --- | --- |
| Identify the source | Confirm product, table, channel, parser, and collection path |
| Sample records | Inspect several complete events before projecting fields away |
| Establish time | Compare event and ingestion time, timezone, drift, and gaps |
| Inspect fields | Note type, null rate, cardinality, examples, and nested structures |
| Check volume | Determine whether the source is steady, bursty, scheduled, or incomplete |

### Orient

Place the observations in their operational context.

| Step | Practical action |
| --- | --- |
| Understand the environment | Asset purpose, identity type, network segment, business process, and working pattern |
| Compare peers | Similar users, devices, servers, applications, or time periods |
| Review collection | Sensor health, policy, parser version, filtering, and retention |
| Identify transformations | Normalisation, enrichment, deduplication, aggregation, and field extraction |
| List alternatives | Legitimate and technical explanations that could create the same pattern |

### Decide

Choose the next question, not the final verdict.

| Step | Practical action |
| --- | --- |
| Refine the hypothesis | State what the current evidence supports and what remains uncertain |
| Select the entity | Decide whether to group by account, host, process, relationship, or session |
| Choose a pivot | Move to a source that can confirm a missing part of the sequence |
| Set stopping criteria | Define what is enough to close, escalate, or widen the investigation |

### Act

Perform the next bounded action.

| Step | Practical action |
| --- | --- |
| Query or collect | Retrieve the evidence needed for the current question |
| Validate | Inspect the raw records behind an aggregate or visualisation |
| Document | Record query, time, source, result, caveat, and next step |
| Respond when authorised | Escalate, preserve, contain, or remediate within the case scope |
| Repeat | Return to Observe with what you have learned |

Restarting the loop is not failure. It is how the model is intended to work.

### Keep a record of each loop

| Record | What to capture |
| --- | --- |
| Question or hypothesis | What this iteration is trying to learn |
| Query and scope | Exact query, source, entities, time window, and relevant parameters |
| Result | What was observed, including useful negative results |
| Limitation | Coverage, quality, assumptions, and alternative explanations |
| Assessment | What the evidence currently supports and with what confidence |
| Next pivot | The next question, source, or stopping decision |

## Grouping and clustering

The terms are often used interchangeably, but they answer different questions.

### Grouping

Grouping applies explicit keys or criteria chosen by the analyst.

Examples include:

- events by account and destination
- connections by process and remote domain
- files by hash and device
- authentication attempts by source, target, and logon type
- observations in fixed time windows

The analyst decides what makes records belong together. This makes grouping reproducible, but the chosen key can hide important variation. Grouping only by account, for example, can conceal differences between source systems and session types.

### Clustering

Clustering brings observations together based on similarity across selected features. It can be algorithmic, visual, or exploratory. The boundaries are inferred from the data and method rather than defined only by a fixed key.

Possible features include:

- event frequency and timing
- command-line tokens
- process ancestry
- destination or certificate characteristics
- byte counts and connection duration
- identity, device, and application behaviour

A cluster is not automatically a threat. It is a set of observations that look similar according to the selected features, distance measure, preprocessing, and parameters.

| Question | Grouping | Clustering |
| --- | --- | --- |
| Who defines membership? | Analyst-defined keys or rules | Similarity method and selected features |
| Is the result deterministic? | Usually, given the same keys | Depends on algorithm, parameters, and preprocessing |
| Best use | Summarising known dimensions | Exploring structure and finding unusual groups |
| Common trap | Grouping away meaningful context | Treating a cluster label as ground truth |

## Stack counting

Stack counting means counting the occurrences of each distinct value in a field, sorting the result, and inspecting both the common and rare ends of the distribution.

Useful fields include:

- process names
- parent-child process relationships
- command-line fragments
- user agents
- destination domains
- service names
- file extensions
- authentication methods
- error and status codes

The count is a lead. It does not tell you whether the value is legitimate or malicious.

The following illustrative distribution shows user-agent families:

{{<mermaid align="center">}}
xychart-beta
    title "Illustrative User-Agent Counts"
    x-axis [Chrome, Firefox, Safari, Edge, Opera, "Py Requests", "Py Urllib", Scrapy, "curl/wget"]
    y-axis "Occurrences" 0 --> 160
    bar [150, 90, 80, 60, 30, 20, 15, 10, 5]
{{< /mermaid >}}

The same data in a table is easier to annotate:

| User-agent family | Count | Initial interpretation |
| --- | ---: | --- |
| Chrome | 150 | Common browser family |
| Firefox | 90 | Common browser family |
| Safari | 80 | Common browser family |
| Edge | 60 | Common browser family |
| Opera | 30 | Less common browser family |
| Python Requests | 20 | Automation or application library |
| Python urllib | 15 | Automation or application library |
| Scrapy | 10 | Crawling or automation framework |
| curl/wget | 5 | Command-line transfer tools |
| **Browser-looking total** | **410** | Requires version, device, and behaviour context |
| **Automation/tool-looking total** | **50** | Not equivalent to malicious |
| **Total** | **460** | Illustrative dataset |

A rare value can be interesting, but rarity is not maliciousness. A common value can also be abused or spoofed. Follow the value into identity, process, device, destination, and historical context.

### Questions after counting

- Is the value rare globally, or only for this peer group?
- Is it new, or merely infrequent?
- Did collection or parsing change?
- Is the value user-controlled or easy to spoof?
- Does the count represent events, sessions, devices, or alerts?
- Can one noisy entity dominate the total?
- What raw records sit behind the count?
- What happened immediately before and after?

## Baselining

Baselining is the practice of describing expected behaviour for a defined entity, peer group, and period. A baseline is not simply “what happened most often”.

A useful baseline states its scope:

| Dimension | Example |
| --- | --- |
| Entity | Privileged administrator accounts |
| Peer group | Accounts with the same operational role |
| Behaviour | Interactive sign-in source-to-destination relationships |
| Time | Previous 30 comparable working days |
| Exclusions | Approved jump hosts and documented exercises |
| Coverage | Identity and endpoint telemetry, with a known weekend gap |
| Review date | When the baseline was last validated |

Baselines drift. Roles change, software is deployed, staff travel, infrastructure moves, and logging changes. A baseline must therefore be versioned and reviewed.

Baselining is also something of a unicorn. It is rare, extremely rare, to find a system or network that has been fully baselined and documented. What I usually find are dashboards that explain part of what is going on. I still consider that useful baselining, as long as we are honest about the scope. A partial, documented view is better than an imaginary complete truth.

### Baseline and profile

In this book, I use the terms as follows:

A profile describes characteristics and relationships associated with an entity or peer group: usual devices, applications, destinations, working hours, authentication methods, and process relationships. A baseline describes the expected level, range, frequency, or variation of selected behaviour over a defined period.

The two support each other. A profile provides context for choosing a relevant population. A baseline helps identify change within that context. Neither is a permanent definition of what is benign.

## Useful summaries

Simple statistics can expose useful questions when their limitations are understood.

| Summary | Useful question | Caveat |
| --- | --- | --- |
| Count | Which values or relationships dominate? | Duplicates and noisy entities can distort totals |
| Distinct count | How many entities participated? | Identifier quality and nulls matter |
| Minimum | What is the smallest or earliest value? | A minimum may be caused by truncation, parsing, or collection boundaries |
| Maximum | What is the largest or latest value? | A maximum may be an outlier or collection artefact |
| Frequency distribution | Which values occur most and least often? | Common does not mean benign, and rare does not mean malicious |
| Sum | What is the cumulative volume? | Aggregation can hide how the total is distributed |
| Percentage | How does a subset compare with the whole? | The denominator and population must be explicit |
| Median | What does the middle observation look like? | Can conceal multiple distinct populations |
| Percentile | Where does an observation sit in the distribution? | Depends on a relevant comparison group |
| Rate | How quickly is activity occurring? | Window size changes the apparent pattern |
| Change | What differs from a previous period or peer? | Collection changes can look like behavioural change |

Combining summaries can tell a stronger story, but the narrative must remain tied to raw evidence.

## A practical workflow

1. Inspect raw records.
2. Write down the source semantics and limitations.
3. Define the entity, population, and time window.
4. Group or count using a deliberate key.
5. Inspect the top, bottom, null, and unexpected values.
6. Pivot back to individual records.
7. Compare with a relevant peer group or historical period.
8. Test technical and legitimate explanations.
9. Corroborate with another data source.
10. Document the query, result, caveat, and assessment.

The objective is not to make data confess. It is to learn what the data can support.

## A small hunt from count to assessment

This example shows how the techniques fit together. Assume the hunter wants to identify uncommon process names and investigate one result.

{{<mermaid align="center">}}
flowchart TD
    A[Aggregate the data] --> B[Select an interesting value]
    B --> C[Inspect raw records]
    C --> D[Add entity and environment context]
    D --> E[Corroborate with other telemetry]
    E --> F[Test alternative explanations]
    F --> G[Write a bounded assessment]
    G --> H[Choose the next pivot or stop]
    H -. New question .-> A
{{< /mermaid >}}

{{% notice style="warning" title="Illustrative queries" %}}
These queries are teaching examples. Table availability, field names, retention, enrichment, and expected behaviour vary between environments. Validate the local schema, including the available `ActionType` values, inspect raw records, and tune the scope before using a query operationally.
{{% /notice %}}

Start with a bounded count:

```sql
DeviceProcessEvents
| where Timestamp > ago(7d)
| where ActionType == "ProcessCreated"
| summarize
    EventCount = count(),
    DeviceCount = dcount(DeviceId)
    by FileName
| order by DeviceCount asc, EventCount asc
```

Suppose `mshta.exe` appears on one device. That is a lead, not a verdict. Return to the underlying events:

```sql
let HuntStart = ago(7d);
DeviceProcessEvents
| where Timestamp > HuntStart
| where ActionType == "ProcessCreated"
| where FileName =~ "mshta.exe"
| project Timestamp, DeviceId, DeviceName, AccountUpn,
    FileName, ProcessCommandLine,
    InitiatingProcessFileName,
    InitiatingProcessCommandLine, SHA1
| order by Timestamp asc
```

The raw records might show that a browser launched `mshta.exe` with an external URL. The hunter should now test that interpretation:

1. Confirm that the process event and command line are complete.
2. Check whether the device role or approved software explains the execution.
3. Inspect the parent process and nearby process activity.
4. Pivot to network telemetry for the device, process context, destination, and time window.
5. Look for downloaded or created files and later execution.
6. Compare with similar devices and a longer historical period.
7. Check sensor health and whether the apparent rarity was created by retention or filtering.

| Record | Example |
| --- | --- |
| Observation | `mshta.exe` executed once on one device during the seven-day window |
| Context | A browser parent and an external URL were recorded in the process chain |
| Supporting evidence | Endpoint process telemetry; related network and file records require validation |
| Alternative explanations | Approved application workflow, administrator testing, or incomplete parent attribution |
| Assessment | Suspicious execution requiring corroboration |
| Confidence | Moderate |
| Next pivot | Validate destination, file activity, process ancestry, user context, and peer history |

If the network and file pivots support the same sequence, confidence may increase. If the activity maps to an approved application used across comparable devices, the hypothesis weakens. If telemetry is missing, the correct result may remain inconclusive.

The important part is not `mshta.exe` itself. The important part is the movement from aggregate, to raw record, to context, to corroboration, to a bounded assessment.

## Resources

- [MITRE ATT&CK Detection Strategies](https://attack.mitre.org/detectionstrategies/)
- [MITRE ATT&CK Data Components](https://attack.mitre.org/datacomponents/)
- [PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html)
- [Baseline Hunting with the PEAK Framework](https://www.splunk.com/en_us/blog/security/peak-baseline-hunting.html)
- [Check the Stats, Your Threat Hunting is Probably Broken](https://www.activecountermeasures.com/check-the-stats-your-threat-hunting-is-probably-broken/)
- [John Boyd, A Discourse on Winning and Losing](https://www.airuniversity.af.edu/Portals/10/AUPress/Books/B_0151_Boyd_Discourse_Winning_Losing.pdf)
- [NIST IR 8354, Digital Investigation Techniques](https://nvlpubs.nist.gov/nistpubs/ir/2022/NIST.IR.8354.pdf)
- [Microsoft Defender XDR DeviceProcessEvents](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-deviceprocessevents-table)

## Revision

| Revised Date | Comment |
| --- | --- |
| 2024-10-06 | Improved formatting and wording |
| 2026-07-24 | Rewritten around source semantics, OODA, grouping, clustering, stack counting, scoped baselines, missing-data semantics, and a visual illustrative mini-hunt |
