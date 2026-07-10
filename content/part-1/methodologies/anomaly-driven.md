---
title: "Anomaly-Driven Hunting"
description: "A practical explanation of anomaly-driven threat hunting and how threat hunters can use baselines, deviations and context to identify suspicious behaviour."
date: 2024-07-29T18:35:05+02:00
lastmod: 2026-07-10
draft: false
hidden: false
weight: 9
tags:
   - frameworks
   - threat hunting
   - anomaly-driven hunting
   - baseline hunting
   - detection engineering
keywords:
   - anomaly-driven hunting
   - anomaly detection
   - baseline hunting
   - threat hunting
   - behavioural analytics
   - UEBA
   - outliers
   - baselines
   - detection engineering
   - PEAK
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Anomaly-driven hunting is a threat hunting approach where the hunter looks for meaningful deviations from expected behaviour.**

The basic idea is simple: malicious activity often creates behaviour that does not fit the normal pattern of an environment. A user logs in from an unusual location. A service account authenticates to systems it normally never touches. A workstation starts talking to infrastructure it has never contacted before. A process launches from an unusual parent. A backup server receives interactive logons from unexpected accounts.

Those deviations may be malicious. They may also be completely benign. That is why anomaly-driven hunting is not only about finding strange things. It is about understanding whether the strange thing matters.

This makes anomaly-driven hunting different from simply reviewing alerts. The hunter is not waiting for a known signature to fire. The hunter is asking what normal looks like, where the environment deviates from that normal and whether those deviations can be explained.

A useful anomaly-driven hunting question is:

```text
What is unusual here, and is it unusual for a reason that matters?
```

That question is important because unusual does not automatically mean suspicious, and suspicious does not automatically mean malicious.

> An anomaly is not a finding. It is an invitation to ask a better question.
>
> -- Roger Johnsen

## What Anomaly-Driven Hunting Is

Anomaly-driven hunting starts with a comparison between expected behaviour and observed behaviour. The expected behaviour may come from a formal baseline, historical data, analyst experience, business knowledge, asset roles, identity patterns, peer groups or known administrative processes. The observed behaviour comes from telemetry.

The hunter then looks for deviations that may indicate attacker activity, misconfiguration, policy violations, misuse, operational drift or visibility problems.

| Element       | Meaning                                                            |
| ------------- | ------------------------------------------------------------------ |
| Baseline      | A representation of what normal or expected behaviour looks like.  |
| Anomaly       | A deviation from that expected behaviour.                          |
| Context       | The information needed to decide whether the deviation matters.    |
| Hypothesis    | A testable explanation for what the anomaly may represent.         |
| Investigation | The process of validating, rejecting or refining that explanation. |

This is where anomaly-driven hunting becomes useful. It does not require the hunter to know the exact indicator in advance. Instead, the hunter starts from behaviour that does not fit the environment. That makes it useful for discovering unknown, emerging or poorly detected activity.

But it also makes it dangerous if used carelessly. Anomaly-driven hunting can produce endless noise if the team does not understand the environment, scope the hunt properly or distinguish between rare and relevant.

## Why Anomalies Matter

Attackers often need to interact with the environment in ways that differ from normal business activity. They may need to:

* authenticate using stolen credentials
* access systems outside a user’s normal pattern
* execute tools from unusual paths
* create new services
* move laterally between hosts
* access sensitive data stores
* stage files before exfiltration
* disable logging, backup or security controls
* use administrative tools from unexpected devices
* communicate with external infrastructure

Each of these behaviours may create anomalies. However, legitimate activity can create anomalies too. A system administrator may perform emergency maintenance. A developer may test a new deployment tool. A service account may be changed by an application owner. A user may travel. A business unit may migrate to a new SaaS platform. A backup job may be rescheduled.

This is why anomaly-driven hunting depends heavily on context. The hunt is not finished when the anomaly is found. It starts there.

## Baseline Before Anomaly

Anomaly-driven hunting depends on some understanding of normal behaviour. A baseline does not have to be perfect, but the hunter needs a reference point. Without a baseline, almost everything can look unusual, and the hunt becomes guesswork.

Baselines can be built around many things:

| Baseline area           | Example questions                                                         |
| ----------------------- | ------------------------------------------------------------------------- |
| User behaviour          | When does this user normally log in, from where and using which devices?  |
| Service accounts        | Which systems does this account normally access, and how often?           |
| Hosts                   | Which processes normally run on this host?                                |
| Network traffic         | Which destinations does this system normally communicate with?            |
| Administrative activity | Which users normally perform privileged operations?                       |
| Cloud activity          | Which applications, tokens, roles and regions are normally used?          |
| Data access             | Which users normally access this file share, database or storage account? |
| Process behaviour       | Which parent-child process relationships are expected on this system?     |

The baseline should also be scoped. A baseline for all users may be too broad. A baseline for finance users, domain administrators, backup operators, developers or point-of-sale systems may be more useful. The more specific the peer group, the more meaningful the anomaly may become.

For example:

```text
This user logged in at 03:00.
```

That may be meaningless.

But this is more useful:

```text
This finance user logged in at 03:00 from a country they have never used before, then accessed a privileged application and downloaded unusually large amounts of data.
```

The difference is context.

## Types of Anomalies

Not all anomalies are the same. A good hunter should understand what kind of deviation they are looking at.

| Anomaly type         | Example                                                                           |
| -------------------- | --------------------------------------------------------------------------------- |
| Time anomaly         | A user logs in outside their normal working hours.                                |
| Location anomaly     | Authentication occurs from an unusual country, region or network.                 |
| Volume anomaly       | A host transfers much more data than usual.                                       |
| Frequency anomaly    | A service account authenticates far more often than normal.                       |
| Peer group anomaly   | One workstation behaves differently from similar workstations.                    |
| Sequence anomaly     | A process chain occurs in an unusual order.                                       |
| Relationship anomaly | A user accesses a system they normally never touch.                               |
| Tooling anomaly      | Administrative tools are used from an unexpected host.                            |
| Persistence anomaly  | New scheduled tasks, services or autoruns appear outside expected change windows. |
| Control anomaly      | Logging, EDR, backup or security controls are modified unexpectedly.              |

These categories help the hunter ask better questions. For example, "unusual PowerShell" is too vague. The hunter should ask what makes it unusual.

* Is it the parent process?
* The command line?
* The user?
* The host?
* The destination?
* The timing?
* The frequency?

The answer changes the investigation.

## Anomaly-Driven Hunting Process

Anomaly-driven hunting can be approached as a structured process.

| Step                      | Purpose                                                                 |
| ------------------------- | ----------------------------------------------------------------------- |
| Define the behaviour area | Decide what kind of behaviour the hunt will examine.                    |
| Establish the baseline    | Understand what normal or expected behaviour looks like.                |
| Identify deviations       | Find activity that differs from the baseline.                           |
| Add context               | Enrich the anomaly with user, host, asset, business and threat context. |
| Form a hypothesis         | Explain what the anomaly may represent.                                 |
| Investigate               | Validate, reject or refine the hypothesis using additional data.        |
| Act                       | Escalate, document, tune, create detections or update baselines.        |
| Feed back                 | Improve the baseline, detection logic or future hunts.                  |

This process matters because anomaly-driven hunting can otherwise become a random walk through strange data. The goal is not to collect anomalies. The goal is to identify deviations that can be explained, tested and acted on.

## From Anomaly to Hypothesis

An anomaly is only a starting point. The hunter must turn it into a hypothesis.

For example:

```text
A service account authenticated to five new servers this week.
```

That is an observation.

A possible hypothesis could be:

```text
The service account may be used for lateral movement because it authenticated to systems outside its normal application scope and was followed by remote service creation.
```

The hypothesis is stronger because it gives direction. It suggests what to validate next:

* Is the service account expected to access those servers?
* Did the authentication originate from a normal host?
* Was the logon type unusual?
* Was there process execution after authentication?
* Were remote services, scheduled tasks or administrative shares used?
* Did the activity happen during a change window?
* Did the account recently receive new privileges?
* Are there related alerts, tickets or deployments?

This is where anomaly-driven hunting becomes real investigation.

The hunter moves from “this is strange” to “this is strange in a way that may indicate a specific behaviour”.

## Practical Example: Unusual Service Account Activity

Consider a service account used by an internal application. For months, the account has authenticated only between a small group of application servers and a database server. Suddenly, the account begins authenticating to several file servers, backup servers and administrative workstations.

That is an anomaly.

### Baseline

The hunter first establishes expected behaviour:

* normal source systems
* normal destination systems
* usual logon types
* normal authentication frequency
* expected maintenance windows
* normal process activity after authentication
* known application owners
* related change tickets

### Anomaly

The new activity differs from the baseline:

* new destination systems
* unusual authentication paths
* access to backup infrastructure
* authentication outside normal hours
* remote administration behaviour after logon

### Hypothesis

The hunter formulates a hypothesis:

```text
The service account may be compromised and used for lateral movement or preparation for ransomware activity.
```

### Investigation

The hunter reviews:

* authentication logs
* endpoint telemetry
* process execution
* remote service creation
* administrative share access
* privilege changes
* backup system access
* related alerts
* change management records

The activity may turn out to be a legitimate application change. It may also reveal compromised credentials, poor service account hygiene, excessive privileges or early-stage attacker activity.

Either outcome has value.

* If it is malicious, the team escalates.
* If it is legitimate but undocumented, the team improves the baseline.
* If it cannot be validated because logs are missing, the team has identified a visibility gap.

## Techniques and Tools

Anomaly-driven hunting can use many techniques. The important part is not the tool itself, but whether the technique helps the hunter identify meaningful deviations.

| Technique             | Use                                                                               |
| --------------------- | --------------------------------------------------------------------------------- |
| Statistical analysis  | Identify outliers using frequency, volume, distribution or time-series patterns.  |
| Peer group comparison | Compare users, hosts or accounts against similar entities.                        |
| Behavioural analytics | Identify deviations in user, entity, process or network behaviour.                |
| Clustering            | Group similar behaviours and identify items that do not fit.                      |
| Time-series analysis  | Detect unusual changes over time.                                                 |
| Rule-based logic      | Identify specific deviations that are known to be suspicious.                     |
| Machine learning      | Model expected behaviour and surface unusual activity for review.                 |
| Visual analysis       | Use timelines, graphs or heatmaps to identify unusual relationships or sequences. |

Machine learning and UEBA tools can support anomaly-driven hunting, but they do not replace the hunter. A tool may say that something is rare. The hunter still has to decide whether it is relevant. Rare activity may be malicious, but it may also be a business exception, maintenance activity, onboarding, travel, testing or telemetry noise.

## Anomaly-Driven Hunting and PEAK

Anomaly-driven hunting fits naturally inside PEAK. PEAK provides the hunt lifecycle:

```text
Prepare → Execute → Act with Knowledge
```

Anomaly-driven hunting can use that lifecycle directly.

| PEAK phase         | Anomaly-driven hunting activity                                                                          |
| ------------------ | -------------------------------------------------------------------------------------------------------- |
| Prepare            | Select the behaviour area, define the peer group, identify data sources and establish the baseline.      |
| Execute            | Identify deviations, enrich with context, form hypotheses and investigate suspicious anomalies.          |
| Act with Knowledge | Document findings, update baselines, tune detections, create new detections or identify visibility gaps. |

This is important because anomaly-driven hunting can easily become endless exploration. PEAK helps keep the hunt disciplined. It forces the team to ask what they are hunting, how they will test it and what should happen with the result.

## Anomaly-Driven Hunting and TaHiTI

TaHiTI can also support anomaly-driven hunting, especially when threat intelligence suggests a behaviour but the organisation does not have a simple indicator to search for. For example, threat intelligence may say that an actor abuses valid accounts for lateral movement. That does not give the hunter one perfect IOC. Instead, it suggests a behaviour pattern.

The hunter can then use anomaly-driven methods to look for deviations such as:

* users authenticating to unusual systems
* service accounts appearing on new hosts
* remote administration activity from unexpected devices
* privileged access outside normal patterns
* unusual authentication sequences before suspicious process execution

In this sense, TaHiTI can provide the hunting trigger, while anomaly-driven hunting provides one way to investigate the behaviour locally.

```text
TaHiTI gives the intelligence-driven question.
Anomaly-driven hunting helps test whether the behaviour appears locally.
```

## Anomaly-Driven Hunting and Detection Engineering

Anomaly-driven hunts often produce useful detection engineering output. The output may be:

* a new detection
* a tuned detection
* a better suppression rule
* a baseline update
* a triage note
* an enrichment requirement
* a visibility gap
* a new hunt idea

However, not every anomaly should become a detection. A detection should be based on behaviour that is meaningful, repeatable and actionable. If the anomaly is too broad, too noisy or too dependent on one narrow context, it may not be suitable as a detection without additional logic.

The team should ask:

| Question                       | Why it matters                                           |
| ------------------------------ | -------------------------------------------------------- |
| Is the anomaly meaningful?     | Prevents detections from being based on random outliers. |
| Is the behaviour repeatable?   | Helps determine whether the detection can run reliably.  |
| Is the signal actionable?      | Ensures analysts know what to do when it fires.          |
| What context is required?      | Prevents alerts from losing meaning outside the hunt.    |
| How noisy is it likely to be?  | Helps estimate tuning and operational cost.              |
| What is the expected response? | Connects detection to triage and incident handling.      |

This is where anomaly-driven hunting can strengthen detection engineering. The hunt does not simply find strange things. It teaches the team which deviations matter.

## What Usually Goes Wrong

Several problems are common in anomaly-driven hunting.

| Problem                              | Why it hurts                                                                     |
| ------------------------------------ | -------------------------------------------------------------------------------- |
| Treating every anomaly as suspicious | The team burns time investigating rare but harmless behaviour.                   |
| No baseline                          | The team cannot explain why something is unusual.                                |
| Weak peer groups                     | Behaviour is compared across entities that should not be compared.               |
| No context                           | The anomaly is reviewed without user, host, asset, business or change context.   |
| Alert queue thinking                 | The team treats anomaly hunting as normal alert triage instead of investigation. |
| Over-reliance on tools               | The tool says something is rare, but nobody validates whether it matters.        |
| Static baselines                     | The baseline becomes outdated as the environment changes.                        |
| No feedback loop                     | Findings do not improve baselines, detections, triage or future hunts.           |
| Ignoring visibility gaps             | Missing telemetry is treated as failure instead of a useful finding.             |

A common failure pattern is simple: the team opens an anomaly dashboard, sorts by rare events, investigates a few strange things and stops when nothing obvious is found. That is not enough.

Anomaly-driven hunting needs a question, a scope, a baseline and a way to decide whether the deviation matters.

## Where Anomaly-Driven Hunting Fits With Other Frameworks

Anomaly-driven hunting fits naturally with the other frameworks in this section.

| Framework       | Main value                                                                                         |
| --------------- | -------------------------------------------------------------------------------------------------- |
| PEAK            | Structures the hunt lifecycle from preparation to execution and action.                            |
| TaHiTI          | Helps turn intelligence into focused hunting questions that may be investigated through anomalies. |
| MITRE ATT&CK    | Provides behavioural vocabulary for describing suspicious activity.                                |
| Pyramid of Pain | Helps move from low-level indicators toward behaviour that is harder for adversaries to change.    |
| OODA Loop       | Helps the hunter observe, orient, decide and act as evidence changes.                              |
| MaGMa           | Helps manage resulting use cases, detections, metrics and improvement work.                        |

Anomaly-driven hunting is not a replacement for these frameworks. It is a hunting approach that can operate inside them.

PEAK gives it a lifecycle. TaHiTI can give it an intelligence-driven trigger. ATT&CK can describe the behaviour. The Pyramid of Pain can help judge the durability of the signal. MaGMa can help preserve and manage the outputs.

## Working Position for This Book

For this book, anomaly-driven hunting is best treated as a practical hunting approach for finding meaningful deviations from expected behaviour.

It helps answer a simple question:

```text
What is unusual in this environment, and does it matter?
```

The value of anomaly-driven hunting is that it can reveal activity that was not already known, signed or explicitly detected.

But the weakness is equally important.

Anomaly-driven hunting can create noise if the hunter does not understand the baseline, scope the hunt properly or add enough context.

A good anomaly-driven hunt should not celebrate strangeness. It should explain it.

> The goal is not to find weird things. The goal is to find weird things that matter.
>
> -- Roger Johnsen

## Resources

* [A Complete Guide to Anomaly Detection in Cyber Security](https://www.varonis.com/blog/anomaly-detection)
* [How to Implement Anomaly Detection for Security Monitoring](https://www.exabeam.com/information-security/anomaly-detection/)
* [Proactive Threat Detection Using Anomaly Detection](https://ieeexplore.ieee.org/document/8424917)
* [MITRE ATT&CK](https://attack.mitre.org/)
* [Pyramid of Pain by David Bianco](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html)

## Revision

| Revised Date | Comment                                                                                                                                                        |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide to anomaly-driven threat hunting, baselines, context, investigation and detection engineering output. |
| 2024-10-06   | Improved formatting and wording                                                                                                                                |
| 2024-07-29   | Added page                                                                                                                                                     |
