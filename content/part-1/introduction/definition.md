---
title: "Definition"
description: "A practical definition of threat hunting, how it differs from SOC alert triage and detection engineering, and why hunts should leave useful security work behind."
date: 2024-06-22T11:37:31+02:00
lastmod: 2026-07-09
draft: false
weight: 1
tags:
   - fundamentals
   - threat hunting
   - definition
keywords:
   - threat hunting
   - threat hunting definition
   - cyber threat hunting
   - SOC
   - detection engineering
   - threat detection
   - IOC
   - IOA
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Ask ten people what threat hunting is, and you will probably get ten slightly different answers. One SOC may mean IOC searches. Another may mean detection engineering based on threat intelligence. A vendor may describe it as a platform capability. In newer AI-driven SOC discussions, someone may even claim that threat hunting can be done automatically by an AI agent. Some of those perspectives are useful. Some are incomplete. Some are just old activities with a newer label.**

**That is why this book needs to start with a definition. Not because one perfect definition exists, but because the rest of the book needs a clear position. When I use the term threat hunting, I do not mean ordinary alert triage with a better title. I do not mean simply searching for known bad indicators. I do not mean detection engineering, although hunting and detection engineering should feed each other.**

To me, threat hunting is structured investigation into what existing alerts, dashboards, detections and assumptions may not show. It is proactive, but not random. It is hypothesis-driven, but still grounded in operational reality. A good hunt should test something: a behaviour, an assumption, a blind spot, a visibility gap, or a possible attacker technique. It should also leave something behind: better understanding, better telemetry, better detection logic, better documentation, or better questions.

---

## Definition

Several organisations and researchers have tried to define threat hunting. Cyborg Security, later acquired by Intel 471, provides one useful definition:

> Threat hunting is an iterative and proactive process whereby threat hunters seek out anomalous activity, artifacts, and
> behaviors within an environment with the objective of identifying previously unknown and undetected threats. This definition has
> two critical components:
>
> Repeatable: A hunt (the commonly accepted term for activity carried out by these teams) has value in its execution, but only for the duration
> of its execution. Once the hunt is complete, any subsequent malicious activity may remain unidentified. Therefore, hunts need to
> be carried out in an iterative fashion based on the prevalence of the technique, and the relative risk to the organization.
>
> Threat hunting must be proactive: The objective of threat hunting is, ultimately, to identify previously undetected malicious activity in an environment. This
> objective is accomplished through a variety of analysis methods, especially those involving behavioral and statistical analysis.
> This process, however, absolutely does not rely on searching through an environment using atomic indicators of compromise
> (IOC). That practice belongs strictly to the domain of traditional security operations, not threat hunting.
>
> Source: [Cyborg Threat Hunting Framework](https://intel471.com/resources/whitepapers/threat-hunting-framework)

That definition captures two important points: threat hunting is proactive, and it must be repeatable enough to have value beyond a single analyst looking at data for a few hours.

My own working definition is:

> Threat hunting is a structured and proactive investigation method used to test assumptions, search for suspicious behaviour, identify visibility gaps, and turn observations into better security work.
>
> -- Roger Johnsen

That last part matters. If a hunt does not improve understanding, telemetry, detection logic, documentation, response guidance, or future hunting questions, it probably did not leave enough behind.

You might wonder where AI fits into this definition. It does, but as an assisting tool. AI may help suggest hypotheses, draft queries, summarise data, or explain unfamiliar artefacts. It may speed up parts of the work, but it does not remove the need for the underlying craft.

Automation without subject-matter understanding is just a faster way to repeat weak assumptions.

There is also a more direct failure mode: AI can hallucinate certainty. It may generate a plausible explanation, map benign activity to a severe technique, or turn harmless code examples from an RSS article into something that looks like malicious behaviour. If the analyst does not understand the data, the environment, and the question being tested, the result may be a confident but wrong conclusion.

My intention with this book is to show the base knowledge and practical handcraft I believe a threat hunter still needs: how to ask better questions, understand the data, test assumptions, and decide whether the evidence supports the conclusion.

## What Threat Hunting Is Not

Threat hunting is often confused with adjacent disciplines. That is understandable, because the same people, tools and datasets may be involved. The difference is in the intent:

| Activity                                                                               | What it is                                                 |
| -------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Investigating an alert to decide whether it is true or false                           | Security monitoring or alert triage                        |
| Searching for known IP addresses, domains, hashes or URLs                              | Indicator search, retrospective investigation or IOC sweep |
| Writing detection rules for known behaviour                                            | Detection engineering                                      |
| Gathering information about threat actors, infrastructure and TTPs                     | Threat intelligence                                        |
| Investigating a confirmed or suspected compromise                                      | Incident response                                          |
| Asking whether an environment would show signs of a behaviour, technique or assumption | Threat hunting                                             |

These activities can support each other. A hunt may produce detection logic. Threat intelligence may produce a hunt hypothesis. Incident response may trigger follow-up hunts. A SOC alert may become the starting point for a wider investigation.

But they should not be collapsed into the same thing.

A useful rule is this:

> If you already know exactly what you are looking for, and you are only checking whether it exists, you are usually not hunting. You are searching.
>
> -- Roger Johnsen

That search may still be valuable. It just should not be dressed up as threat hunting.

## Why Threat Hunting Matters

Threat hunting matters because security operations are built on assumptions. You assume that important systems are logging correctly. You assume that detections cover the behaviours you care about. You assume that alert names describe what happened. You assume that your telemetry is complete enough to answer the questions you ask of it.

Those assumptions are often wrong. Threat hunting gives you a structured way to test them.

The value is not only that a hunter might find an attacker. That can happen, and it is obviously important when it does. But a more common and still valuable outcome is that the hunt reveals something about the environment:

* A detection does not cover what people think it covers.
* A log source is missing from important systems.
* A field needed for investigation is not populated.
* A rule catches a symptom, but not the behaviour.
* An alert name leads analysts towards the wrong conclusion.
* A common attacker technique would not be visible with current telemetry.
* A SOC runbook tells analysts what to click, but not what to reason about.

This is why threat hunting is useful even when no compromise is found.

{{% notice tip %}}
A hunt with no malicious findings can still be a good hunt if it improves the organisation’s ability to see, understand and respond.
{{% /notice %}}

## Threat Hunting and SOC Work

SOC analysts and threat hunters often work with the same data, but they usually work from different starting points.

| SOC work often starts with                          | Threat hunting often starts with                                       |
| --------------------------------------------------- | ---------------------------------------------------------------------- |
| Is this suspicious, and what should we do about it? | If this behaviour happened in our environment, would we see it?        |
| What does this alert mean?                          | What would this technique look like in our telemetry?                  |
| Is this a true positive or a false positive?        | Are our existing detections giving us a useful view of this behaviour? |

That difference matters. SOC work is often alert-driven, time-sensitive and queue-based. The analyst must make decisions with the information available. Threat hunting should have more room for exploration, comparison, baselining and hypothesis testing. That does not make threat hunting better than SOC analysis. It is simply a different mode of work.

In mature environments, the two should feed each other:

* SOC alerts reveal behaviours worth hunting further.
* Hunts reveal detection gaps.
* Detection engineering turns repeatable hunt logic into monitored content.
* Runbooks and playbooks are updated with what the hunt learned.
* Analysts get better context for future investigations.


The mistake is treating threat hunting as a heroic function sitting above the SOC. It should be part of a feedback loop.


## Threat Detection vs Threat Hunting

Threat detection and threat hunting overlap, but they are not the same discipline:

| Threat Detection                                                              | Threat Hunting                                                                                       |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Usually starts with a known behaviour, indicator or rule condition.           | Usually starts with a question, assumption, behaviour or hypothesis.                                 |
| Looks for activity that detection logic has already been written to identify. | Looks for activity that may not already be covered by existing detections.                           |
| Produces alerts for investigation.                                            | Produces findings, questions, detection ideas, telemetry requirements or documentation improvements. |
| Must be reliable enough to run repeatedly in production.                      | Can be exploratory, investigative and iterative.                                                     |
| Often works best when the behaviour is well understood.                       | Often works best when the environment, behaviour or visibility is uncertain.                         |
| Alert logic must consider noise, severity, routing and analyst workload.      | Hunt logic can be broader, more exploratory and more tolerant of manual analysis.                    |

A hunt query is not automatically a detection rule. This is a common mistake. A query that works well during a hunt may be too noisy, too expensive, too context-dependent or too vague to become a production alert. Before hunt logic becomes detection logic, it needs to be refined, tested, tuned and documented.

A good hunt should therefore ask two follow-up questions:

1. Did we learn something useful?
2. Can any part of this be made repeatable?

Sometimes the answer becomes a detection rule. Sometimes it becomes a parser fix, a logging requirement, a dashboard improvement, a runbook update, or a note that says: "we cannot currently see this well enough."

That is still valuable.

## Outputs from Threat Hunts

A threat hunt should produce more than a yes-or-no answer. The exact output depends on the hunt, but useful outcomes often include the following:

### Findings

The obvious output is a finding: suspicious or malicious activity that should be investigated, contained or escalated.  This is the outcome people tend to think about first, but it is not the only valid outcome.

A finding should be clear about:

* what was observed
* why it matters
* what evidence supports it
* what remains uncertain
* what should happen next

The finding should not rely on an alert name alone. Alert names are not evidence.

### Visibility Gaps

A hunt may show that the organisation cannot answer the question it wanted to ask. That is not a failed hunt. It is useful knowledge.

Examples:

* Endpoint telemetry is missing from critical systems.
* DNS logging is incomplete.
* Authentication logs do not contain the fields needed for analysis.
* Proxy logs are retained for too short a period.
* Cloud audit logging is enabled, but not collected centrally.
* The SIEM has the data, but the schema makes the behaviour hard to investigate.

A visibility gap should be documented as clearly as a detection finding. Otherwise the same weak assumption will return later.

### Detection Ideas

Many hunts produce detection ideas. These should be written as behaviour, not as vague observations.

| Weak observation                | Detection-ready behaviour                                                                                                    |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Suspicious executable observed. | Unsigned executable launched from a user-writable directory and initiated outbound network communication within two minutes. |

The second version is closer to something detection engineering can work with. Detection ideas should include enough detail for someone else to understand the behaviour, test it, and decide whether it can be monitored reliably.


### Improved Documentation

A hunt may reveal that analysts do not have enough guidance. That may lead to:

* updated runbooks
* improved playbooks
* better triage notes
* clearer escalation criteria
* better examples of benign and suspicious behaviour
* known false positive patterns
* investigation questions for future analysts

This is not glamorous work, but it matters. Documentation is one of the ways a hunt leaves value behind.

### Incident Response Support

Threat hunters can support incident response when the incident requires wider analysis, historical investigation or behaviour-based searching. This does not mean that threat hunters replace incident responders. It means that hunting methods can help answer questions such as:

* Where else has this behaviour occurred?
* Did this activity happen before the known incident window?
* Are there related systems, users or processes?
* Does the attacker behaviour appear elsewhere?
* Can we identify lateral movement, staging, persistence or exfiltration attempts?
* Do our detections cover this behaviour going forward?

During incidents, hunting should be disciplined. It should not become uncontrolled exploration. The questions must be tied to the incident objectives.

### Better Questions

Sometimes the most useful output from a hunt is a better question.

| Starting question                    | Better hunting question                                                                                                                   |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Are there signs of PowerShell abuse? | Which users execute encoded PowerShell commands from Office child processes, and how often does that happen outside administrative hosts? |

The first question is too broad. The second is testable, scoped and closer to operational reality. Threat hunting is partly the discipline of turning vague suspicion into better questions.


## Types of Threat Hunting

Threat hunting is often divided into structured and unstructured hunting. That distinction is useful, but it should not be treated as a rigid taxonomy. In practice, hunts often move between these modes.

| Type                 | Description                                                                                                            | Example                                                                                                                                                                                 |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Structured hunting   | Starts with a defined hypothesis, behaviour, technique or intelligence requirement.                                    | Do we observe signs of scheduled task abuse on endpoints used by privileged accounts?                                                                                                   |
| Unstructured hunting | Starts with exploration of data, often using baselining, stacking, clustering, frequency analysis or anomaly analysis. | Stack rare parent-child process relationships and investigate why certain combinations appear only once.                                                                                |
| Combined hunting     | Uses one hunting mode to support another.                                                                              | Use frequency analysis to identify unusual authentication patterns, then turn those observations into a hypothesis about password spraying, service account misuse or lateral movement. |

The method is less important than the discipline.

A hunt should be scoped, documented and explainable. If another analyst cannot understand what was tested and why, the hunt is too dependent on the person who ran it.


## Hunting Approaches

The following approaches are common in threat hunting. They are not mutually exclusive.

| Approach                    | Key idea                                                                   | Example question                                                                                                                           |
| --------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Intelligence-driven hunting | Use threat intelligence to shape the hypothesis.                           | A report says a threat actor uses a specific persistence technique. Do we see behaviour consistent with that technique in our environment? |
| Technique-driven hunting    | Use an attacker technique, often from MITRE ATT&CK, as the starting point. | Can we identify suspicious use of scheduled tasks, remote services or PowerShell in our endpoint telemetry?                                |
| Crown jewel-driven hunting  | Start from the systems, identities or data that matter most.               | What suspicious authentication, process or network activity touches our most critical systems?                                             |
| Anomaly-driven hunting      | Start from unusual behaviour in the environment.                           | Which hosts show rare process execution, unusual traffic volumes or uncommon authentication patterns?                                      |
| Baseline-driven hunting     | Compare current behaviour against expected behaviour.                      | Which users or systems have changed behaviour significantly compared with their normal pattern?                                            |
| Volume analysis             | Look at the amount of activity and whether it makes sense.                 | Why did one host generate far more authentication failures than similar systems?                                                           |
| Frequency analysis          | Look at how often something occurs.                                        | Which commands, domains, parent processes or logon types are rare in this environment?                                                     |
| Clustering analysis         | Group similar activity to find outliers.                                   | Which process chains do not fit the common clusters observed across the fleet?                                                             |
| Stack counting              | Count and sort observed values to identify rare or extreme cases.          | Which command lines occur only once, and are any of them suspicious?                                                                       |
| Grouping analysis           | Look for combinations of conditions.                                       | Which events combine unusual parent process, user context and network destination?                                                         |

These approaches are tools for thinking. They do not replace judgement. MITRE ATT&CK, threat intelligence, baselines and statistics can all help shape a hunt, but none of them will tell you what the data means by themselves.

## What Usually Goes Wrong

Threat hunting often fails for ordinary reasons. The organisation may call IOC searches "hunting" because it sounds more mature. The SOC may not have time to do anything beyond alert triage. Telemetry may be incomplete. Detection engineers may receive vague hunt findings that cannot be turned into reliable rules. Management may expect every hunt to find an attacker, which creates pressure to overstate weak findings.

Another common problem is that hunts are not written down properly. The analyst remembers what was tested, but nobody else can reproduce the reasoning. Three months later, the same question returns and the organisation has learned very little.

Threat hunting needs curiosity, but curiosity is not enough. It also needs structure. At minimum, a hunt should document:

* the question or hypothesis
* the scope
* the data used
* the logic or queries used
* the observations
* the interpretation
* the limitations
* the output
* what should happen next

That is how hunting becomes organisational knowledge instead of personal exploration.

## Working Definition for This Book

For the rest of this book, I use the following practical definition:

> Threat hunting is the structured investigation of behaviours, assumptions and visibility gaps that may not be covered by existing alerts.

The purpose is not only to find attackers. The purpose is to understand the environment better and improve the security work that follows.

A useful hunt should produce at least one of these:

* a confirmed or suspected finding
* a disproven or refined hypothesis
* a visibility gap
* a detection idea
* a telemetry requirement
* improved documentation
* a better investigation method
* a better question

If the hunt leaves nothing behind, it was probably just browsing logs.

## References

Resources and references:

* [Threat Hunting vs. Threat Detecting: What's The Difference?](https://www.splunk.com/en_us/blog/learn/threat-hunting-vs-threat-detecting.html)
* [Cyborg Security - Threat Hunting Framework](https://info.cyborgsecurity.com/hubfs/Gated%20Content/Cyborg%20Security%20-%20Threat%20Hunting%20Framework.pdf)
* [50 Threat Hunting Hypothesis Examples](https://www.cyborgsecurity.com/blog/50-threat-hunting-hypothesis-examples/)
* [Protecting the ‘Crown Jewels’ with Threat Hunting](https://techspective.net/2019/12/13/protecting-the-crown-jewels-with-threat-hunting/)
* [How to Identify Cyber Critical Systems with a Crown Jewel Analysis](https://www.dragos.com/blog/how-to-identify-cyber-critical-systems/)
* [Cyber Threat Hunting: Types, Methodologies, Best Practices](https://www.knowledgehut.com/blog/security/cyber-threat-hunting)
* [SOC Analyst vs Threat Hunter](https://www.siemxpert.com/blog/soc-analyst-vs-threat-hunter/)
* [What is Cyber Threat Hunting?](https://www.crowdstrike.com/cybersecurity-101/threat-hunting/)
* [IOA vs IOC](https://www.crowdstrike.com/cybersecurity-101/indicators-of-compromise/ioa-vs-ioc/)
* [What is Proactive Cyber Threat Hunting and Why Should You Do It?](https://www.securebrain.co.jp/eng/blog/what-is-proactive-threat-hunting/)
* [Corelight Threat Hunting Guide](https://go.corelight.com/threat-hunting-guide-ws)
* [Security Analytics Use Cases for Threat Hunting](https://atos.net/wp-content/uploads/2021/01/security-analytics-use-cases-for-threat-hunting.pdf)
* [Threat Hunting Use Case: Web Proxy](https://www.reliaquest.com/blog/threat-hunting-use-case-web-proxy/)
* [Threat Hunting with MITRE’s ATT&CK Framework: Part 1](https://www.digitalguardian.com/blog/threat-hunting-mitres-attck-framework-part-1)
* [Threat Hunting with MITRE’s ATT&CK Framework Part 2 – Advanced Use Cases](https://www.digitalguardian.com/blog/threat-hunting-mitre%E2%80%99s-attck-framework-part-2-%E2%80%93-advanced-use-cases)
* [Cyber Wardog Lab by Roberto Rodriguez](https://cyberwardog.blogspot.com/)
* [10 Benefits of Threat Hunting](https://resources.infosecinstitute.com/topic/10-benefits-of-threat-hunting)
* [Flavio Queiro on what Threat Hunting isn't](https://www.linkedin.com/posts/flavioqueiroz_threathunting-threatdetection-threatanalysis-activity-7207336024216133632-bdvv/)

## Revision

| Revised Date | Comment                                                                                          |
| ------------ | ------------------------------------------------------------------------------------------------ |
| 09-07-2026   | Rewritten to establish a clearer practitioner voice, improve structure, and add AI reflections   |
| 22-02-2025   | Improved formatting and revised wording                                                          |
| 29-07-2024   | Added page                                                                                       |
