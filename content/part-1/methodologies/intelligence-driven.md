---
title: "Intelligence-Driven Hunting"
description: "A practical explanation of intelligence-driven threat hunting and how threat hunters can turn threat intelligence into focused, relevant and testable investigations."
date: 2024-07-29T18:34:54+02:00
lastmod: 2026-07-10
draft: false
hidden: false
weight: 11
tags:
   - frameworks
   - threat hunting
   - threat intelligence
   - intelligence-driven hunting
   - detection engineering
keywords:
   - intelligence-driven hunting
   - threat hunting
   - threat intelligence
   - CTI
   - adversary behaviour
   - MITRE ATT&CK
   - TaHiTI
   - PEAK
   - Pyramid of Pain
   - detection engineering
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Intelligence-driven hunting is a threat hunting approach where threat intelligence is used to decide what to hunt, why it matters and how the investigation should be shaped.**

The basic idea is simple: intelligence should not only be something the organisation reads. It should help the organisation decide what to investigate.

Threat intelligence may describe adversaries, campaigns, tools, infrastructure, vulnerabilities, targets, tactics, techniques and procedures. Some of that intelligence may be useful for detection. Some may be useful for blocking. Some may be useful for awareness. Some may not be relevant at all.

Intelligence-driven hunting asks a practical question:

```text
What does this intelligence mean for our environment?
```

That question is important because threat intelligence is not automatically useful just because it is interesting.

A report may describe a real adversary. The behaviour may be valid. The campaign may be active. But the hunter still has to ask whether the intelligence is relevant, testable and actionable in the local environment.

Intelligence-driven hunting is the process of making that translation.

## What Intelligence-Driven Hunting Is

Intelligence-driven hunting uses threat intelligence to guide hunting activity.

That intelligence may come from:

* threat reports
* intelligence feeds
* incident reports
* information sharing communities
* ISACs
* vendor research
* government advisories
* internal incident response
* red team activity
* vulnerability intelligence
* malware analysis
* adversary emulation
* previous hunts

The intelligence may include indicators, techniques, actor behaviour, targeting patterns, victimology, infrastructure, tools, procedures or campaign context.

But intelligence-driven hunting is not the same as searching for every IOC in a report.

IOC searches can be useful. They may help scope exposure, identify known infrastructure or validate whether something already seen elsewhere exists locally. But if the hunt stops at hashes, domains and IP addresses, it may miss the more important behaviour.

A stronger intelligence-driven hunt asks:

```text
What behaviour does this intelligence describe, and can we test for that behaviour here?
```

That moves the team from intelligence consumption to intelligence-led investigation.

## Why Intelligence-Driven Hunting Matters

Many organisations receive more threat intelligence than they can realistically use.

Reports arrive. Feeds update. Advisories are published. Indicators are imported. Technique names are mapped. Dashboards grow.

But more intelligence does not automatically mean better hunting.

The real challenge is selection and translation.

The team needs to decide:

* Which intelligence is relevant to the organisation?
* Which threats match the organisation’s exposure?
* Which behaviours are plausible in the environment?
* Which assets or users would matter most?
* Which data sources can support the hunt?
* Which intelligence should become a hypothesis?
* Which intelligence should be ignored, monitored or added to a backlog?
* Which findings should feed back into intelligence, detection or response?

This is where intelligence-driven hunting becomes valuable.

It helps the team focus.

Instead of hunting whatever seems interesting, the team hunts what is relevant, testable and useful.

## Intelligence Quality and Local Relevance

The quality of the intelligence matters, but relevance matters just as much.

A high-quality report may still be irrelevant to the organisation.

A lower-confidence report may still be worth exploring if the behaviour matches the organisation’s exposure.

The hunter should therefore ask two questions:

```text
Is the intelligence credible?
```

and:

```text
Is it relevant to us?
```

Credibility and relevance are not the same thing.

| Question                                                    | Why it matters                                                                |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Is the source reliable?                                     | Helps determine whether the intelligence should influence hunting priorities. |
| Is the reporting recent enough?                             | Prevents the team from chasing stale activity.                                |
| Is the behaviour described clearly?                         | Helps the hunter translate the intelligence into something testable.          |
| Does the threat target our sector, geography or technology? | Helps determine local relevance.                                              |
| Are the affected assets present in our environment?         | Prevents hunting for threats that cannot realistically apply.                 |
| Do we have the telemetry required to test it?               | Determines whether the hunt can be executed properly.                         |
| Can we act on the result?                                   | Avoids work that produces no practical security improvement.                  |

A useful intelligence-driven hunt starts when intelligence is both relevant enough and testable enough to justify investigation.

## From Intelligence to Hunting Question

The most important step is translation.

A threat report may say:

```text
The actor uses compromised valid accounts for lateral movement.
```

That is useful, but it is not yet a hunt.

The hunter needs to turn it into a local question:

```text
Do we see valid accounts authenticating to unusual systems, followed by remote administration activity or suspicious process execution?
```

That question can become a hypothesis:

```text
An adversary may be using valid accounts for lateral movement by authenticating from user workstations to servers outside normal administrative patterns, followed by remote service creation or suspicious process execution.
```

This translation step is where many intelligence-driven efforts fail.

The team reads the report but does not turn it into a hunt.

Or the team extracts indicators but never investigates the behaviour.

Or the team maps a technique to ATT&CK but does not identify the data needed to test it.

Intelligence-driven hunting works when the intelligence changes the investigation.

## Indicator-Led and Behaviour-Led Hunting

Intelligence-driven hunting can start from indicators or behaviours.

Both can be useful, but they are not equal.

| Intelligence type  | Hunting use                                       | Limitation                                       |
| ------------------ | ------------------------------------------------- | ------------------------------------------------ |
| Hashes             | Search for known files.                           | Easy for adversaries to change.                  |
| IP addresses       | Search for known infrastructure connections.      | Infrastructure may be reused, retired or shared. |
| Domains            | Search for known malicious or suspicious domains. | Domains can be changed quickly.                  |
| URLs               | Search proxy, email or browser telemetry.         | Often short-lived and campaign-specific.         |
| Tools              | Search for known tool behaviour or artefacts.     | Tools may be renamed, modified or replaced.      |
| Techniques         | Hunt for adversary behaviour.                     | Requires stronger telemetry and analysis.        |
| Procedures         | Hunt for how a technique is used in practice.     | Requires context and local understanding.        |
| Targeting patterns | Prioritise assets, users or business areas.       | May be incomplete or uncertain.                  |

Low-level indicators are not useless. They can help with scoping, blocking, enrichment and fast validation.

But behaviour-led hunting is often more durable.

The Pyramid of Pain is useful here because it helps the hunter ask whether the hunt is focused on something the adversary can easily change, or something the adversary depends on operationally.

For intelligence-driven hunting, the goal is usually to move from:

```text
Search for this indicator.
```

toward:

```text
Test whether this adversary behaviour appears locally.
```

## Intelligence-Driven Hunting Process

Intelligence-driven hunting can be approached as a structured process.

| Step                   | Purpose                                                                                |
| ---------------------- | -------------------------------------------------------------------------------------- |
| Select intelligence    | Choose intelligence that is credible, relevant and useful enough to investigate.       |
| Assess local relevance | Determine whether the threat, behaviour, technology or exposure applies locally.       |
| Translate behaviour    | Convert reporting into local hunting questions or hypotheses.                          |
| Define scope           | Select relevant users, assets, systems, time ranges and data sources.                  |
| Test with telemetry    | Query and analyse data to support, weaken or reject the hypothesis.                    |
| Add context            | Use asset, identity, business, vulnerability and threat context to interpret findings. |
| Act on findings        | Escalate, document, create detections, improve controls or update baselines.           |
| Feed back              | Use local findings to improve intelligence, detection and future hunting priorities.   |

This process prevents a common failure pattern: treating intelligence-driven hunting as a feed-to-SIEM exercise.

Importing indicators is not the same as hunting. A hunt requires a question, a scope, data, analysis and a decision about what the result means.

## Practical Example: Cloud Token Abuse

Consider a threat report describing an actor that uses phishing to obtain cloud session tokens and then abuses them to access sensitive SaaS applications.

A weak intelligence-driven response would be:

```text
Search for the domains and IP addresses mentioned in the report.
```

That may be useful, but it is not enough.

A stronger hunting question would be:

```text
Do we see targeted users showing unusual cloud session behaviour after interacting with suspicious links or phishing-related infrastructure?
```

A possible hypothesis:

```text
An adversary may be using stolen cloud session tokens to access sensitive SaaS applications from unusual locations, devices or user agents after phishing activity.
```

### Scope

The hunter may scope the hunt around:

* targeted users
* recent phishing campaigns
* cloud sign-in logs
* SaaS audit logs
* identity provider telemetry
* device compliance state
* user agent changes
* impossible travel patterns
* OAuth consent activity
* sensitive application access

### Investigation

The hunter may review:

* suspicious link clicks
* sign-ins from unusual locations
* session token reuse patterns
* changes in device or user agent
* access to sensitive SaaS applications
* new OAuth grants
* mailbox rules
* file downloads
* failed MFA or interrupted sign-ins
* activity from unmanaged devices

The goal is not only to prove that a listed IOC was present.

The goal is to determine whether the behaviour described in the intelligence appears in the local environment.

### Output

Possible outputs include:

* confirmed suspicious cloud session activity
* rejected or inconclusive hypothesis
* new detections for suspicious SaaS access
* improved triage guidance for identity alerts
* updated phishing investigation playbooks
* visibility gap around SaaS logging
* new internal intelligence about user targeting
* follow-up hunt for OAuth abuse or mailbox manipulation

This is intelligence-driven hunting in practice: external reporting becomes local investigation.

## Telemetry Reality

Intelligence often describes behaviour that the organisation cannot fully observe.

That is not unusual.

A report may describe endpoint behaviour, but the organisation lacks endpoint coverage on the relevant servers. It may describe SaaS activity, but audit logs are not retained long enough. It may describe network infrastructure, but proxy logs are incomplete. It may describe PowerShell behaviour, but script block logging is disabled.

This does not mean the hunt has failed.

It means the intelligence has revealed a visibility gap.

A useful outcome may be:

```text
We cannot currently test this behaviour properly.
```

That should be documented and acted on.

The result may become:

* a logging recommendation
* a telemetry onboarding request
* a detection limitation
* a risk acceptance discussion
* a follow-up hunt after visibility improves
* a note in the hunt record explaining what could and could not be tested

Intelligence-driven hunting should be honest about what the organisation can observe.

## Feedback to Threat Intelligence

Intelligence-driven hunting should not be a one-way process.

Threat intelligence informs hunting, but hunting should also inform intelligence.

A hunt may show that:

* the reported behaviour is relevant locally
* the organisation is not exposed to the described technique
* the threat targets assets the organisation actually owns
* the reported indicators were not seen, but related behaviour was
* the adversary behaviour appears with local variations
* existing controls disrupt the behaviour
* telemetry is insufficient to assess exposure
* new internal observations should be shared with the intelligence team

This feedback improves future intelligence assessments.

It also helps the organisation move from generic external reporting to local intelligence.

That local knowledge is often more useful than another feed entry.

## What Usually Goes Wrong

A common failure pattern is simple: a team receives a report, extracts indicators, searches for them, finds nothing and closes the activity.

That may be useful scoping.

It is not enough for intelligence-driven hunting.

The more important question is what behaviour the intelligence described and whether that behaviour can be tested locally.

| Problem                   | Why it hurts                                                                             |
| ------------------------- | ---------------------------------------------------------------------------------------- |
| Treating feeds as hunting | Importing indicators does not replace investigation.                                     |
| Chasing every report      | The team spends effort on intelligence that is not relevant, testable or actionable.     |
| IOC-only hunting          | The hunt misses the behaviour behind the indicators.                                     |
| No local relevance check  | The team hunts for threats that do not match its exposure, assets or technology.         |
| No telemetry check        | The team cannot test the intelligence properly but does not document the visibility gap. |
| Over-mapping to ATT&CK    | Techniques are tagged but do not guide analysis or data selection.                       |
| No feedback loop          | Findings do not improve intelligence, detection, triage or future hunt priorities.       |
| Vendor-report dependency  | Hunting priorities are driven by what vendors publish rather than what matters locally.  |

Intelligence-driven hunting should create focus, not dependency on whatever report arrived most recently.

## Where Intelligence-Driven Hunting Fits

Intelligence-driven hunting fits naturally with the other frameworks in this book.

It is not a replacement for hypothesis-driven or anomaly-driven hunting. It can feed both. Intelligence may become a hypothesis when it gives the hunter a behaviour to test. It may also drive anomaly hunting when the intelligence suggests patterns that should look unusual in the local environment.

| Framework             | How it supports intelligence-driven hunting                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| TaHiTI                | Provides a methodology for turning intelligence into hunt triggers, backlog items, hypotheses, investigation scope and final outputs. |
| PEAK                  | Provides the lifecycle for preparing, executing and acting on the hunt.                                                               |
| MITRE ATT&CK          | Provides behavioural vocabulary for describing adversary techniques and shaping what to look for.                                     |
| Pyramid of Pain       | Helps move from fragile indicators toward durable behaviours and procedures.                                                          |
| OODA Loop             | Helps the hunter adapt as new evidence changes the understanding of the hunt.                                                         |
| MaGMa                 | Helps preserve and manage resulting use cases, detections, metrics and improvement work.                                              |
| Detection engineering | Turns useful hunt findings into tested, tuned and owned detection logic where appropriate.                                            |

A simple way to separate the roles is:

```text
TaHiTI gives intelligence-driven hunting a methodology.
PEAK gives the hunt a lifecycle.
ATT&CK gives the behaviour a language.
The Pyramid of Pain helps judge the durability of what we hunt for.
Detection engineering turns useful findings into operational detection.
MaGMa helps preserve and manage the outputs.
```

For example, a threat report may become a TaHiTI trigger. ATT&CK may help describe the reported behaviour. The Pyramid of Pain may help the hunter move beyond indicators and focus on durable adversary behaviour. PEAK then structures the hunt from preparation to execution and action. If the hunt produces useful logic, detection engineering turns that logic into something that can be tested, tuned and maintained.

In that sense, intelligence-driven hunting is not a separate management framework. It is a hunting approach that uses intelligence to decide what should be investigated and why it matters.

## Working Position for This Book

For this book, intelligence-driven hunting is best treated as a practical approach for turning threat intelligence into local investigation.

It helps answer a simple question:

```text
What does this intelligence mean for us, and how can we test it?
```

The value of intelligence-driven hunting is focus.

It helps the team prioritise relevant threats, translate reporting into local questions and connect external intelligence to internal telemetry.

But the hunter must remain selective.

Not every report deserves a hunt. Not every IOC deserves investigation. Not every ATT&CK mapping means the organisation is exposed.

A good intelligence-driven hunt should be relevant, testable and useful.

> The point is not to consume more intelligence. The point is to use intelligence to ask better local questions.
>
> -- Roger Johnsen

## Resources

* [Understanding Threat Intelligence and Its Role in Cyber Defense](https://www.sans.org/white-papers/37978/)
* [DEF-TaHiTI Threat Hunting Methodology](https://www.betaalvereniging.nl/wp-content/uploads/DEF-TaHiTI-Threat-Hunting-Methodology.pdf)
* [MITRE ATT&CK](https://attack.mitre.org/)
* [Pyramid of Pain by David Bianco](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html)
* [ThreatHunting.org](https://threathunting.org)

## Revision

| Revised Date | Comment                                                                                                                                                              |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide to intelligence-driven threat hunting, local relevance, telemetry reality and detection engineering output. |
| 2024-10-06   | Improved formatting and wording                                                                                                                                      |
| 2024-07-29   | Added page                                                                                                                                                           |
