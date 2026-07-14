---
title: "Hypothesis-Driven Hunting"
description: "A practical explanation of hypothesis-driven threat hunting and how threat hunters can turn assumptions about adversary behaviour into focused, testable investigations."
date: 2024-07-29T18:34:30+02:00
lastmod: 2026-07-10
draft: false
hidden: false
weight: 1
tags:
   - frameworks
   - threat hunting
   - hypothesis-driven hunting
   - detection engineering
keywords:
   - hypothesis-driven hunting
   - threat hunting
   - threat hunting hypothesis
   - MITRE ATT&CK
   - adversary behaviour
   - detection engineering
   - PEAK
   - TaHiTI
   - Pyramid of Pain
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Hypothesis-driven hunting is a threat hunting approach where the hunter starts with a testable idea about possible adversary behaviour and then uses data to validate, weaken or reject it.**

The hypothesis gives the hunt direction. It tells the hunter what behaviour to look for, which data sources may be relevant, what scope should be used and what kind of evidence would matter.

A weak hunt often starts with a vague question:

```text
Do we have attackers in the environment?
```

That is too broad to be useful. A stronger hypothesis is more specific:

```text
An adversary may be using valid accounts to move laterally from workstations to servers outside normal administrative patterns.
```

This gives the hunter something to test. The value of hypothesis-driven hunting is not that the hunter already knows the answer. The value is that the hunter has a structured question that can be investigated. A good hypothesis should create direction without pretending to be certainty.

> A hypothesis is not a conclusion. It is a disciplined way to decide what to test next.
>
> -- Roger Johnsen

## What Hypothesis-Driven Hunting Is

Hypothesis-driven hunting is built around a simple idea: the hunter makes a testable statement about possible adversary behaviour, then investigates whether the environment supports or weakens that statement.

The hypothesis may come from threat intelligence, previous incidents, red team findings, ATT&CK techniques, anomaly-driven hunting, detection gaps, business risk, crown jewel analysis or analyst experience.

The important part is that the hypothesis must be testable.

A hypothesis-driven hunt needs a few things to work well. The hypothesis must be testable. The scope must define which systems, users, time ranges and data sources are included. The hunter also needs to know what kind of evidence would support, weaken or reject the hypothesis, and what context is required to decide whether the evidence matters.

The outcome does not have to be binary. A hunt may support the hypothesis, weaken it, reject it, remain inconclusive or lead to a better question.

The hypothesis does not have to be perfect at the start. It often improves during the hunt. That is normal. Hunting is investigative work. The hunter starts with a structured assumption, tests it against data and adjusts as evidence appears.

## Why Hypotheses Matter

Without a hypothesis, a hunt can easily become browsing. The hunter opens logs, searches for interesting events, follows a few oddities and eventually stops when time runs out. That may still produce findings, but it is difficult to repeat, measure or explain.

A hypothesis helps prevent that. It gives the hunt a purpose. It also helps the hunter decide what not to do. This matters because security telemetry is enormous. Without scope, a hunter can drown in data.

A useful hypothesis helps answer:

* What behaviour are we looking for?
* Why do we think this behaviour may matter?
* Which systems, users or assets are in scope?
* Which data sources can support the hunt?
* What would support the hypothesis?
* What would weaken the hypothesis?
* What would make the result inconclusive?
* What should happen if the hypothesis is supported?

This is why hypothesis-driven hunting is one of the most practical ways to structure threat hunting.

It turns curiosity into an investigation.

## What Makes a Good Hypothesis

A good hunting hypothesis should be specific enough to guide the hunt, but not so narrow that it only searches for one known indicator.

It should usually include:

| Part                | Example                                                                  |
| ------------------- | ------------------------------------------------------------------------ |
| Actor or behaviour  | An adversary may be using valid accounts.                                |
| Technique or action | The accounts may be used for lateral movement.                           |
| Environment scope   | The activity may involve workstation-to-server authentication.           |
| Expected deviation  | The pattern may differ from normal administrative behaviour.             |
| Evidence direction  | Authentication logs and endpoint telemetry should show related activity. |

A strong hypothesis might look like this:

```text
An adversary may be using valid accounts for lateral movement by authenticating from user workstations to servers outside normal administrative patterns, followed by remote service creation or suspicious process execution.
```

This hypothesis is useful because it suggests:

* identity data
* endpoint telemetry
* server authentication logs
* remote service creation events
* administrative activity baselines
* peer group comparison
* follow-up pivots

A weak hypothesis would be:

```text
Attackers may be using valid accounts.
```

That may be true, but it is too broad. It does not give enough direction. A hypothesis should narrow the investigation without closing the hunter’s mind.

## Sources of Hypotheses

Hypotheses can come from many places.

| Source                 | Example hunting idea                                                        |
| ---------------------- | --------------------------------------------------------------------------- |
| Threat intelligence    | A reported actor uses service accounts for lateral movement.                |
| MITRE ATT&CK           | A technique such as Remote Services may be relevant to the environment.     |
| Previous incidents     | Similar behaviour may have occurred before or remained undetected.          |
| Red team findings      | A technique worked during an exercise and should be hunted historically.    |
| Detection gaps         | A known behaviour is not covered by current monitoring.                     |
| Anomaly-driven hunting | An unusual pattern may suggest a broader behaviour to test.                 |
| Crown jewel analysis   | Critical systems may attract specific adversary behaviours.                 |
| Business change        | New infrastructure or SaaS adoption may create new exposure.                |
| Analyst experience     | A hunter recognises behaviour that has mattered in previous investigations. |

This is where hypothesis-driven hunting connects naturally to TaHiTI. Threat intelligence can create the hunting idea. The hypothesis turns that idea into something testable.

## From Idea to Hypothesis

Many hunting ideas start too vague. For example:

```text
We should hunt for ransomware.
```

That is not a useful hypothesis. Ransomware is an outcome, not a specific behaviour to test. A better path is to translate the idea into behaviour:

```text
Ransomware operators often prepare by accessing backup systems, disabling recovery options or moving laterally using privileged accounts.
```

Then the hunter can create a testable hypothesis:

```text
An adversary may be preparing for ransomware activity by using privileged or service accounts to access backup infrastructure outside normal administrative patterns.
```

Now the hunt has direction. The hunter can define:

* relevant accounts
* backup systems
* authentication paths
* administrative tools
* process execution
* change windows
* expected behaviour
* suspicious deviations
* escalation criteria

That is the difference between a topic and a hypothesis:

* A topic says what the hunt is about.
* A hypothesis says what the hunt will test.

## Hypothesis-Driven Hunting Process

A hypothesis-driven hunt can be structured as a simple process.

| Step              | Purpose                                                                              |
| ----------------- | ------------------------------------------------------------------------------------ |
| Select topic      | Choose the behaviour, threat, gap or risk area.                                      |
| Form hypothesis   | Create a testable statement about possible adversary behaviour.                      |
| Define scope      | Decide which users, systems, time ranges and data sources are included.              |
| Identify evidence | Decide what would support, weaken or refute the hypothesis.                          |
| Gather data       | Collect or query the telemetry required to test the hypothesis.                      |
| Analyse           | Look for patterns, relationships, deviations and supporting context.                 |
| Refine            | Adjust the hypothesis if the evidence suggests a better question.                    |
| Conclude          | Confirm, weaken, reject or mark the hypothesis as inconclusive.                      |
| Act               | Escalate, document, create detections, update baselines or generate follow-up hunts. |

This process should be disciplined, but not rigid. The hunter should not force the data to fit the hypothesis. The hypothesis is a tool for investigation, not something that must be proven.

## Evidence: Supporting, Weakening and Inconclusive

A common mistake is to think that a hypothesis-driven hunt has only two outcomes:

```text
Confirmed or not confirmed.
```

Reality is usually more nuanced.

| Outcome      | Meaning                                                              |
| ------------ | -------------------------------------------------------------------- |
| Supported    | The evidence suggests that the hypothesis may be true.               |
| Weakened     | Some evidence exists, but important parts do not fit.                |
| Rejected     | The available evidence does not support the hypothesis.              |
| Inconclusive | The data is incomplete, unreliable or insufficient.                  |
| Refined      | The original hypothesis led to a better or more specific hypothesis. |

A rejected hypothesis can still be valuable. It may show that a technique is not currently visible in the environment, that controls are working, or that the original risk assumption was weak. An inconclusive result can also be valuable. It may reveal that logs are missing, retention is too short, endpoint coverage is incomplete or the available data does not support the hunt. That is not wasted effort. That is knowledge.

> A hypothesis-driven hunt does not fail because the hypothesis is rejected. It fails when the team learns nothing from the test.
>
> -- Roger Johnsen

## Practical Example: PowerShell Execution

Consider a hunt based on suspicious PowerShell usage. A weak hypothesis might be:

```text
Attackers may be using PowerShell.
```

That is too broad. PowerShell is used for many legitimate administrative tasks. A better hypothesis might be:

```text
An adversary may be using PowerShell to execute encoded commands from user workstations where PowerShell is not normally used for administration.
```

This is more useful because it defines behaviour, scope and expected deviation.

### Prepare

The hunter defines:

* user workstations in scope
* expected administrative hosts
* PowerShell logging availability
* command line logging quality
* script block logging availability
* normal administrative patterns
* time range
* known management tools
* existing detections

The hunter also defines what evidence would matter:

* encoded commands
* suspicious parent processes
* unusual user context
* network connections after execution
* downloaded scripts
* execution from office applications or browsers
* activity outside normal administration patterns

### Execute

The hunter queries endpoint telemetry and PowerShell logs.

Possible pivots include:

* parent process
* command line
* user
* host
* script content
* network destination
* file creation
* related authentication
* timeline around execution

The hunter compares findings against normal administrative behaviour.

### Act

Possible outputs include:

* confirmed suspicious PowerShell activity
* rejected hypothesis with documented reasoning
* improved baseline of legitimate PowerShell use
* detection logic for encoded PowerShell from unusual parent processes
* recommendation to enable missing PowerShell logging
* triage guidance for analysts
* follow-up hunt for related credential access or lateral movement

The value is not only whether malicious PowerShell is found. The value is that the organisation now understands the behaviour better.

## Hypothesis-Driven Hunting and Detection Engineering

Hypothesis-driven hunts can produce valuable detection engineering output. A hunt may identify behaviour that should become a detection. It may also reveal that an existing detection is too narrow, too noisy or missing important context.

Possible outputs include:

* new detections
* improved detections
* triage guidance
* enrichment requirements
* suppression logic
* visibility gaps
* validation tests
* baseline updates
* follow-up hunts

The team should ask:

| Question                                | Why it matters                                            |
| --------------------------------------- | --------------------------------------------------------- |
| What behaviour did the hypothesis test? | Helps avoid detections based only on one artefact.        |
| What evidence supported the hypothesis? | Shows which telemetry and logic mattered.                 |
| What evidence weakened it?              | Helps prevent overfitting.                                |
| What context was required?              | Makes the detection easier to triage.                     |
| How repeatable is the behaviour?        | Determines whether it can become durable detection logic. |
| What should analysts do when it fires?  | Connects detection to response.                           |

A hunt query is not automatically a production detection. The query may be exploratory, expensive, broad or dependent on manual context. Turning hunt output into detection engineering requires tuning, validation, testing and ownership.

## Where Hypothesis-Driven Hunting Fits

Hypothesis-driven hunting fits naturally with the other frameworks in this book.

A hypothesis gives the hunt something to test, but other frameworks help shape where the hypothesis comes from, how it is executed and what happens afterwards.

| Framework       | How it supports hypothesis-driven hunting                                                              |
| --------------- | ------------------------------------------------------------------------------------------------------ |
| PEAK            | Provides the lifecycle: prepare the hypothesis, execute the investigation and act on what was learned. |
| TaHiTI          | Helps turn threat intelligence into focused hunting hypotheses.                                        |
| MITRE ATT&CK    | Provides behavioural vocabulary for describing adversary techniques and shaping what to look for.      |
| Pyramid of Pain | Helps move hypotheses away from fragile indicators and toward adversary behaviour.                     |
| OODA Loop       | Helps the hunter adapt as evidence changes during the investigation.                                   |
| MaGMa           | Helps preserve and manage resulting use cases, detections, metrics and improvement work.               |

For example, TaHiTI may provide the intelligence-driven trigger, ATT&CK may help describe the relevant behaviour, and the Pyramid of Pain may help the hunter ask whether the hypothesis is focused on something durable or something the adversary can easily replace.

PEAK then gives the hunt a lifecycle:

```text
Prepare → Execute → Act with Knowledge
```

In that lifecycle, the hypothesis is not the whole hunt. It is the steering mechanism. It helps the hunter decide what to test, what evidence matters and how the result should be interpreted.

## What Usually Goes Wrong

A common failure pattern is simple: the team starts with a threat topic, calls it a hypothesis and begins querying.

```text
Hunt for lateral movement.
```

That is not a hypothesis. It is a topic.

A useful hypothesis explains what kind of lateral movement may be happening, where it may appear, how it may differ from normal behaviour and what evidence would matter.

Several problems are common:

| Problem                      | Why it hurts                                                                          |
| ---------------------------- | ------------------------------------------------------------------------------------- |
| Vague hypothesis             | The hunt becomes too broad and turns into log browsing.                               |
| Untestable hypothesis        | The team cannot validate or reject the idea with available telemetry.                 |
| Overly narrow hypothesis     | The hunt becomes an IOC search and misses related behaviour.                          |
| Confirmation bias            | The hunter tries to prove the hypothesis instead of testing it.                       |
| No evidence criteria         | The team does not define what would support, weaken or refute the hypothesis.         |
| Ignoring rejected hypotheses | The team treats “not found” as wasted effort instead of documenting what was learned. |
| No follow-through            | Findings do not become detections, baselines, documentation or future hunts.          |

A hypothesis-driven hunt should create focus, not tunnel vision.

## Working Position for This Book

For this book, hypothesis-driven hunting is best treated as a practical approach for turning a threat idea into a testable investigation.

It helps answer a simple question:

```text
What do we believe may be happening, and how can we test it?
```

The value of hypothesis-driven hunting is focus.

It helps the hunter avoid aimless searching, define relevant data, test assumptions and produce a result that can be explained.

But the hunter must remain honest.

The goal is not to prove the hypothesis. The goal is to learn whether the evidence supports it, weakens it, rejects it or points to a better question.

## Resources

* [MITRE ATT&CK](https://attack.mitre.org/)
* [A Guide to Threat Hunting: Hypothesis-Driven Approach](https://www.sans.org/white-papers/39875/)
* [Pyramid of Pain by David Bianco](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html)
* [ThreatHunting.org](https://threathunting.org)
* [DEF-TaHiTI Threat Hunting Methodology](https://www.betaalvereniging.nl/wp-content/uploads/DEF-TaHiTI-Threat-Hunting-Methodology.pdf)

## Revision

| Revised Date | Comment                                                                                                                                                                       |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide to hypothesis-driven threat hunting, testable assumptions, evidence, investigation and detection engineering output. |
| 2024-10-06   | Improved formatting and wording                                                                                                                                               |
| 2024-07-29   | Added page                                                                                                                                                                    |
