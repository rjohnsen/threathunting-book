+++
title = "Establishing a Timeline"
date = 2024-09-15T14:26:43+02:00
lastmod = 2026-07-24T00:00:00+02:00
weight = 20
chapter = false
+++

Over the years, I have trained many students to become SOC analysts and, eventually, threat hunters. Timelines are one of the concepts people tend to underestimate. They are often imagined as a straight line from point A to point B.

An investigation timeline is more demanding. It is a reconstruction built from observations that were recorded by different systems, at different moments, with different clocks and retention limits. It should help us understand what happened, what may have happened, and where the evidence is incomplete.

{{% notice style="warning" title="A timeline is not the attack itself" %}}
A log timestamp records when a source observed, generated, received, or stored something. It may not be the exact time the underlying action occurred. Keep source, timezone, collection delay, and clock quality beside the event.
{{% /notice %}}

## What belongs in a useful timeline

A strong timeline is not just a timestamp and a sentence. I normally want these fields:

| Field | Why it matters |
| --- | --- |
| Normalised timestamp and timezone | Makes ordering possible and exposes conversion mistakes |
| Original timestamp and timezone | Preserves what the source actually recorded |
| Data source | Identifies the system and viewpoint behind the observation |
| Asset and identity | Connects activity to entities without assuming they are the same thing |
| Event or action | States what the telemetry records |
| Evidence reference | Links back to the raw event, query, case item, or artefact |
| Assessment | Separates interpretation from observation |
| Confidence | Shows how strongly the evidence supports the assessment |
| Caveat or gap | Records missing telemetry, clock drift, parsing, or alternative explanations |

A compact case table might look like this:

| Time (UTC) | Source | Observation | Assessment | Confidence |
| --- | --- | --- | --- | --- |
| 09:02:14 | Email telemetry | Message delivered to a mailbox associated with the account | Possible initial delivery | Moderate |
| 09:17:51 | Identity provider | Successful sign-in from a new source | Account use requires validation | Moderate |
| 09:21:03 | Endpoint | Browser wrote an archive to Downloads | Likely browser-mediated retrieval | High |
| 09:23:40 | Endpoint | Script interpreter started from the archive chain | Suspicious execution sequence | High |

The assessment column matters. Without it, interpretation tends to be written as fact.

### A practical confidence scale

Confidence belongs to the assessment, not to the raw observation. Use the words consistently and record why a level was chosen.

| Confidence | Practical meaning |
| --- | --- |
| High | Reliable evidence strongly supports the assessment, material alternatives were tested, and no material contradiction remains. Independent corroboration exists where reasonably available |
| Moderate | The evidence supports the assessment, but corroboration, coverage, or plausible alternatives remain incomplete |
| Low | The assessment is plausible but rests on limited, indirect, ambiguous, or lower-quality evidence |

Confidence describes how strongly the available evidence supports an assessment. It is separate from both severity and the assessed likelihood of the event. A high-confidence benign explanation and a low-confidence high-impact concern are both possible.

When evidence changes, update the confidence and preserve the reason. Do not quietly rewrite the earlier judgement.

## Start with an anchor, not a beginning

Most investigations do not begin at initial access. They begin with an anchor:

- an alert
- an unusual sign-in
- a suspicious process
- a file
- a user report
- an outbound connection
- a change to an account or system

The anchor is where you start searching, not necessarily where the incident started. Move both backwards and forwards in time:

```text
earlier context <- anchor observation -> later consequences
```

Backward pivots can reveal delivery, authentication, staging, or a parent process. Forward pivots can reveal execution, persistence, lateral movement, collection, exfiltration, or impact.

## Phases are labels, not required boxes

A case may contain behaviours associated with initial access, execution, persistence, privilege escalation, lateral movement, collection, command and control, exfiltration, or impact. These labels can help readers navigate the sequence, but an investigation should not invent a phase merely to complete a familiar attack chain.

| Behavioural area | Questions |
| --- | --- |
| Entry or first observed access | What is the earliest supported foothold? Could activity predate retention? |
| Execution | Which process, identity, command, file, or service acted? |
| Persistence | What could survive a restart, sign-out, or credential change? |
| Privilege and credentials | Which privileges or authentication material changed hands? |
| Discovery and movement | Which systems, shares, accounts, and services were touched? |
| Collection and staging | What data was gathered, transformed, or prepared? |
| Exfiltration or impact | What left, changed, stopped, encrypted, or became unavailable? |

The sequence may branch, overlap, repeat, or remain incomplete.

## An illustrative timeline

The following scenario is deliberately simplified. It demonstrates the shape of a reconstruction, not a universal attack path and not evidence from a live case.

The starting hypothesis is:

> Outbound activity from a sensitive system may represent data exfiltration. Determine what initiated the connection, what data was involved, and how far back the supporting activity can be traced.

The hunter begins at the outbound connection and pivots in both directions. The resulting evidence supports this working sequence:

{{<mermaid align="center">}}
flowchart TD
    A[Email: Message delivered to mailbox 09:00]
    B[Identity: Sign-in associated with account 09:15]
    C[Identity: Session issued 09:16]
    D[Endpoint: Archive retrieval observed 09:20]
    E[Endpoint: Script execution observed 09:23]
    F[Sensitive system: Remote access observed 10:15]
    G[Sensitive system: Data staged 11:35]
    H[Network: Outbound transfer observed 12:00]
    I[Sensitive system: Artefacts deleted 12:30]

    A --> B --> C --> D --> E --> F --> G --> H --> I
{{< /mermaid >}}

This diagram does not prove causality by itself. Each arrow should be backed by evidence and assessed separately. For example, a sign-in after a phishing message does not prove credential theft unless identity, session, user, and interaction evidence support that link.

## Ordering problems to expect

### Event, ingestion, and processing time

Several timestamps can describe the same record:

| Time | Meaning |
| --- | --- |
| Event time | When the source says the observed activity occurred |
| Ingestion time | When the collection or analytics platform received the record |
| Processing time | When a parser, rule, enrichment, correlation, or workflow handled the record |

{{<mermaid align="center">}}
flowchart LR
    A[Activity occurs] -->|Event time recorded| B[Event timestamp assigned]
    B --> C[Record collected]
    C -->|Ingestion time recorded| D[Record ingested]
    D -->|Processing begins| E[Record processed]
    E --> F[Alert or analytic result created]
{{< /mermaid >}}

Use event time as the primary reconstruction axis when its quality is understood. Use ingestion and processing time to investigate collection delay, rule execution, alert timing, and pipeline behaviour. Preserve all available timestamps rather than replacing one with another.

### Clock drift and timezone conversion

Endpoint, network, SaaS, identity, and email systems may use different timezones or have imperfect clocks. Preserve original values and document normalisation. Do not silently shift timestamps until the sequence looks plausible.

### Collection and ingestion delay

An event can arrive in the SIEM minutes or hours after it occurred. Use event time for reconstruction and ingestion time to understand collection behaviour. They answer different questions.

### Duplicate and transformed events

Forwarders, connectors, retries, and normalisation pipelines can duplicate or alter records. Deduplicate only when you understand which identifiers and transformations are stable.

### Missing events

An absence may be consistent with an action not occurring, but it can also indicate collection gaps, retention limits, filtering, clock problems, parsing failures, or deliberate removal. State which explanation the evidence supports.

### Reused identifiers

Process IDs, logon IDs, IP addresses, filenames, and cloud session values can be reused or scoped locally. Pair them with device, timestamp, boot or session context, and stronger identifiers where available.

### Conflicting observations

Two sources can disagree because they observe different stages, use different clocks, transform fields, or attach time at different points. Preserve both observations. Compare source semantics, raw timestamps, identifiers, clock quality, ingestion delay, and collection path before deciding whether they describe the same activity.

If the conflict cannot be resolved, keep the competing interpretations in the case record. An unresolved contradiction is evidence about the limits of the reconstruction, not a reason to select the tidier version.

## Building the timeline

1. Preserve the anchor event and its raw fields.
2. Convert time carefully while retaining the original timestamp and timezone.
3. Identify the source viewpoint and what the record can actually establish.
4. Add earlier and later observations without deciding the full story yet.
5. Correlate on stable identifiers where possible.
6. Separate observations from assessments.
7. Test alternative explanations for each important link.
8. Mark gaps, contradictions, clock concerns, and collection boundaries.
9. Summarise the supported sequence at the level of confidence the evidence allows.
10. Keep references that let another analyst reproduce every key event.

## Evidence and narrative timelines

One timeline rarely serves every audience equally well.

{{<mermaid align="center">}}
flowchart LR
    A[Endpoint telemetry] --> E[Evidence timeline]
    B[Identity telemetry] --> E
    C[Network telemetry] --> E
    D[Other case evidence] --> E
    E --> F[Correlation, caveats, and confidence]
    F --> N[Narrative timeline]
    N --> R[Audience-specific report]
    E -. Trace every material statement .-> R
{{< /mermaid >}}

| Timeline | Purpose | Typical content |
| --- | --- | --- |
| Evidence timeline | Preserve a detailed and reproducible reconstruction | Original and normalised timestamps, raw observations, source references, identifiers, queries, assessments, confidence, caveats, and contradictions |
| Narrative timeline | Explain the supported sequence to a particular audience | Selected milestones, material decisions, scope, impact, confidence, and known gaps |

Build the narrative timeline from the evidence timeline. Do not let the shorter report become the only record of the technical reconstruction. A reader should be able to trace every material narrative statement back to its supporting evidence.

## Timelines in reports

A report timeline should help different readers answer different questions:

| Reader | Question |
| --- | --- |
| Investigator | Which pivot or evidence supports the next step? |
| Incident responder | What must be contained, preserved, or remediated now? |
| Detection engineer | Which observable sequence could be detected earlier? |
| System owner | Which asset, account, or control was involved? |
| Management | What happened, when was it known, and what was the impact? |
| Legal or compliance | Which facts are documented, and what remains uncertain? |

Avoid filling the report with every benign event. Include what explains the sequence, changes the assessment, defines scope, or documents a meaningful gap.

A good timeline does not make the case look cleaner than it was. It makes the reasoning inspectable.

## Resources

- [NIST IR 8354, Digital Investigation Techniques](https://nvlpubs.nist.gov/nistpubs/ir/2022/NIST.IR.8354.pdf)
- [NIST SP 800-86, Guide to Integrating Forensic Techniques into Incident Response](https://csrc.nist.gov/pubs/sp/800/86/final)
- [ODNI ICD 203, Analytic Standards](https://www.dni.gov/files/documents/ICD/ICD-203.pdf)

## Revision

| Revised Date | Comment |
| --- | --- |
| 2024-10-06 | Improved formatting and wording |
| 2026-07-24 | Rewritten around evidence, time quality, bidirectional pivots, confidence criteria, conflicting observations, visual time flow, evidence and narrative timelines, and defensible reporting |
