---
title: "Diamond Model"
description: "A practical explanation of the Diamond Model of Intrusion Analysis and how threat hunters can use it to structure observations, connect evidence and improve handover."
date: 2024-06-23T20:15:39+02:00
lastmod: 2026-07-10
draft: false
weight: 1
tags:
    - frameworks
    - threat hunting
    - intrusion analysis
    - diamond model
keywords:
    - Diamond Model
    - Diamond Model of Intrusion Analysis
    - threat hunting
    - intrusion analysis
    - adversary
    - infrastructure
    - capability
    - victim
    - threat intelligence
    - MITRE ATT&CK
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**The Diamond Model of Intrusion Analysis is a useful framework for structuring what we know, what we think we know, and what we still need to investigate during threat hunting and intrusion analysis.**

The model was introduced by Sergio Caltagirone, Andrew Pendergast and Christopher Betz in 2013. It describes intrusion activity through four connected elements:

* adversary
* infrastructure
* capability
* victim

Those four elements form the “diamond”. For threat hunters, the value of the model is not that it magically identifies an attacker. It does not. The value is that it gives the analyst a structured way to organise observations, connect evidence and avoid treating isolated indicators as complete understanding. A suspicious IP address is not the full story. A malware sample is not the full story. A compromised host is not the full story.

The Diamond Model helps the hunter ask:

* What do we know about the adversary?
* What infrastructure is involved?
* What capability or behaviour is being used?
* Who or what is the victim?
* How do these elements connect?

That makes it useful as both an analytical framework and a documentation aid.

> The Diamond Model does not solve the intrusion for you. It helps you organise the intrusion so you can reason about it.
>
> -- Roger Johnsen

## The Four Core Elements

The Diamond Model is built around four vertices.

![Example Diamond Model from Sergio Caltagirone](/images/diamond-model.png)

| Vertex         | Meaning                                                                                 |
| -------------- | --------------------------------------------------------------------------------------- |
| Adversary      | The individual, group or organisation responsible for the activity.                     |
| Infrastructure | The systems, services, networks or communication channels used to conduct the activity. |
| Capability     | The tools, techniques, malware, exploits or tradecraft used by the adversary.           |
| Victim         | The person, organisation, account, device, system or process targeted by the activity.  |

These elements are connected. That connection is the point. An adversary uses a capability over infrastructure against a victim.

In practice, we may not know all four elements at the beginning of an investigation. That is normal. The model still helps because it gives us a place to put what we do know and a way to see what is missing.

For example:

| Observation                         | Diamond element |
| ----------------------------------- | --------------- |
| Suspicious PowerShell execution     | Capability      |
| External command-and-control domain | Infrastructure  |
| Compromised workstation             | Victim          |
| Unknown actor                       | Adversary       |
| Spear-phishing delivery             | Capability      |
| Abused cloud service                | Infrastructure  |
| Finance department user             | Victim          |

The model does not require perfect knowledge. It supports structured uncertainty.

## Adversary

The adversary represents the actor behind the intrusion activity. This may be:

* a criminal group
* a state-sponsored actor
* an insider
* an initial access broker
* a ransomware affiliate
* a contractor or third party
* an unknown actor

In real investigations, the adversary is often the hardest part of the diamond to complete. Attribution is difficult, and threat hunters should be careful not to overstate confidence.

It is usually safer to describe what we observe than to claim who did it too early.

Weak statement:

```text
This was actor X.
```

Better statement:

```text
The observed behaviour overlaps with techniques reported for actor X, but the available evidence is not sufficient for attribution.
```

That distinction matters. A threat hunter may use threat intelligence, MITRE ATT&CK mappings, infrastructure overlaps, malware characteristics, timing, targeting and tradecraft to form an assessment. But assessment is not proof.

> Attribution is useful when it is supported. It is dangerous when it becomes a shortcut.
>
> -- Roger Johnsen

## Infrastructure

Infrastructure describes what the adversary uses to communicate, deliver, stage or control activity. This may include:

* IP addresses
* domains
* URLs
* command-and-control servers
* phishing infrastructure
* compromised hosts
* cloud services
* VPNs
* proxies
* malware delivery sites
* email infrastructure
* remote access tooling
* abused legitimate platforms

Infrastructure is often where investigations start because it is visible in logs. DNS logs, proxy logs, firewall logs, EDR telemetry and email logs may all reveal infrastructure.

But infrastructure should not be treated in isolation. An IP address may be suspicious, benign, compromised, shared, reused or short-lived. A domain may be part of attacker infrastructure, but it may also be a legitimate service abused by the attacker.

The useful question is not only:

```text
Is this infrastructure bad?
```

The better question is:

```text
How is this infrastructure connected to the capability, the victim and the wider activity?
```

That shift prevents the investigation from becoming a simple indicator lookup.

## Capability

Capability describes what the adversary uses or does. This may include:

* malware
* scripts
* exploits
* credential theft
* phishing
* scheduled tasks
* remote services
* command-line activity
* living-off-the-land techniques
* lateral movement
* persistence mechanisms
* data staging
* exfiltration methods

In threat hunting, capability is often the most useful part of the model because it connects directly to behaviour. Infrastructure changes quickly. Indicators expire. Actor names may be uncertain. But capability often leaves patterns that can be searched for, baselined and turned into detections.

For example:

| Capability           | Possible hunting question                                              |
| -------------------- | ---------------------------------------------------------------------- |
| PowerShell execution | Which parent processes launch PowerShell in unusual ways?              |
| Scheduled tasks      | Which newly created tasks have unusual names, paths or commands?       |
| Remote services      | Which systems are using remote administration outside normal patterns? |
| Credential dumping   | Which processes access credential material unexpectedly?               |
| Data staging         | Where do we see unusual archive creation or bulk file movement?        |

This is why the Diamond Model works well together with MITRE ATT&CK. ATT&CK can help describe the behaviour. The Diamond Model can help place that behaviour in relation to the adversary, infrastructure and victim.

## Victim

The victim is the target of the activity. This may be:

* a user
* a workstation
* a server
* a cloud tenant
* an identity
* an application
* a business process
* a department
* an organisation
* a third party

Threat hunters should be careful not to define the victim too narrowly. A single compromised endpoint may be the first visible victim, but the real target may be a business process, privileged identity, database, file share or cloud environment.

For example:

| Visible victim        | Possible broader victim context                |
| --------------------- | ---------------------------------------------- |
| User workstation      | Initial access into corporate environment      |
| Finance user          | Access to payment processes                    |
| Domain controller     | Credential access and control plane compromise |
| Cloud admin account   | Tenant-level control                           |
| File server           | Sensitive data collection                      |
| Developer workstation | Source code or deployment pipeline access      |

The victim vertex helps the hunter ask why this target matters. That question is important. Without victim context, the investigation may remain technically interesting but operationally incomplete.

## Meta-Features

The Diamond Model also includes meta-features that help describe the activity more precisely. Useful meta-features include:

| Meta-feature | Meaning                                             |
| ------------ | --------------------------------------------------- |
| Timestamp    | When the activity occurred.                         |
| Phase        | Where the activity fits in the intrusion lifecycle. |
| Result       | What happened as a result of the activity.          |
| Direction    | How the activity moved between entities.            |
| Methodology  | How the activity was performed.                     |
| Resources    | What assets, tools or accounts were used.           |

These features help the analyst build chronology and context. Meta-features make the model operational. Timestamp and phase help the hunter understand sequence. Direction helps describe how activity moved between systems, users or infrastructure. Result helps separate attempted activity from successful activity. Methodology helps connect the observation to tradecraft.

For example:

| Meta-feature combination   | Why it matters                                                                              |
| -------------------------- | ------------------------------------------------------------------------------------------- |
| Timestamp + phase          | Helps distinguish initial access from later command-and-control activity.                   |
| Direction + infrastructure | Helps separate inbound delivery, outbound beaconing and possible exfiltration.              |
| Result + capability        | Helps distinguish attempted execution from successful execution.                            |
| Phase + victim             | Helps show whether the victim was the initial target, a pivot point or the final objective. |
| Methodology + capability   | Helps turn observed behaviour into detection or hunting logic.                              |

For example, two events may involve the same domain and same workstation. But if one event is an initial download and the other is command-and-control traffic three hours later, they represent different parts of the intrusion.

The diamond is useful, but the timeline matters too. This is where the Diamond Model starts to support timeline reconstruction, detection engineering and intrusion graph thinking.

## Example From The DFIR Report

One strong practical use of the Diamond Model can be seen in reports from [The DFIR Report](https://thedfirreport.com/).

For example, in their report [IcedID Brings ScreenConnect and CSharp Streamer to ALPHV Ransomware Deployment](https://thedfirreport.com/2024/06/10/icedid-brings-screenconnect-and-csharp-streamer-to-alphv-ransomware-deployment/#diamond-model), they use the model to organise important intrusion details from the investigation.

![Example Diamond Model from DFIR Report](/images/dfir-report-diamond-model.png)

This is useful because it shows the Diamond Model as more than a theoretical framework. It becomes a compact way to document adversary, infrastructure, capability and victim information from real intrusion activity.

## Using the Diamond Model During Threat Hunting

During a threat hunt, the Diamond Model can be used as a note-taking and reasoning structure. As observations appear, place them into the model.

| Hunt observation                             | Diamond element               |
| -------------------------------------------- | ----------------------------- |
| Rare external domain contacted by endpoint   | Infrastructure                |
| PowerShell launched by Word                  | Capability                    |
| User workstation in finance department       | Victim                        |
| Unknown actor using phishing-like tradecraft | Adversary                     |
| DNS lookup before outbound HTTPS session     | Infrastructure / meta-feature |
| Encoded command-line argument                | Capability                    |
| Follow-on authentication from same host      | Capability / victim context   |

This helps the hunter avoid losing track of the investigation. It also makes gaps visible.

For example:

| Known                  | Unknown                                     |
| ---------------------- | ------------------------------------------- |
| Victim workstation     | Whether other hosts are affected            |
| Suspicious PowerShell  | Whether this is malicious or administrative |
| External domain        | Whether the domain is attacker-controlled   |
| User context           | Whether the user expected this activity     |
| Possible phishing path | Whether the email delivery can be confirmed |

The model helps structure both evidence and uncertainty.

> A good framework does not remove uncertainty. It gives you a disciplined way to handle it.
>
> -- Roger Johnsen

## Pivoting Through the Diamond

The Diamond Model becomes especially useful when it is used for pivoting. Each vertex can become a starting point for the next question. The hunter does not only document adversary, infrastructure, capability and victim. The hunter moves between them.

| Starting point       | Pivot question                                                                                        |
| -------------------- | ----------------------------------------------------------------------------------------------------- |
| Infrastructure       | Which other victims communicated with the same domain, IP address, service or infrastructure pattern? |
| Capability           | Where else do we see the same behaviour, tool, command line, technique or execution pattern?          |
| Victim               | What other activity surrounds this user, host, identity, application or business process?             |
| Adversary assessment | Which reported techniques, infrastructure patterns or behaviours should we test locally?              |

For example, a hunt may start with one suspicious workstation contacting an unusual external domain. That first observation gives the hunter infrastructure and victim context.

From there, the hunter can pivot:

* Infrastructure → other hosts contacting the same domain
* Victim → other suspicious activity on the same workstation
* Capability → similar PowerShell or script execution elsewhere
* Adversary assessment → threat intelligence describing related behaviour

This is where the Diamond Model becomes practical. It helps the hunter decide which direction to investigate next without losing the structure of the case.

In practice, investigations often involve multiple connected diamonds rather than one static diamond. One event may describe initial access. Another may describe command and control. Another may describe lateral movement or exfiltration. Linked together over time, those diamonds start to form an intrusion graph.

That is also why the timeline matters. A domain contacted during payload download may have a different meaning than the same domain contacted repeatedly during command-and-control activity.

Repeated pivots across capability, infrastructure and victim context often reveal stable patterns. Those patterns are useful beyond the current investigation. They may become detection logic, triage guidance, enrichment requirements or follow-up hunts.


> The Diamond Model is not only a way to document what happened. It is a way to decide where to pivot next.
>
> -- Roger Johnsen

## Practical Example: Insider Threat and Data Exfiltration

Consider an overly simplified insider threat scenario. The concern is that sensitive data may be leaving the organisation through unusual network activity. The hunter starts by reviewing network traffic and identifies several unusual communication patterns. Each communication point is documented under infrastructure.

| Observation                                 | Diamond element            |
| ------------------------------------------- | -------------------------- |
| External file-sharing domain                | Infrastructure             |
| Repeated outbound HTTPS sessions            | Infrastructure             |
| Large transfers outside business hours      | Capability                 |
| Workstation used by privileged employee     | Victim                     |
| Internal user with access to sensitive data | Adversary / victim context |
| Compressed archive created before transfer  | Capability                 |

The investigation then moves through the diamond.

* First, the hunter documents infrastructure: external domains, IP addresses, cloud services and communication patterns.
* Next, the hunter documents capability: archive creation, large data movement, access to sensitive file paths and outbound transfer behaviour.
* Then the hunter documents victim context: the device, user, department, data location and business role.

The adversary vertex is more difficult. In an insider case, the adversary may be an internal user, a compromised account or an external actor using a valid account. That distinction cannot be assumed.

A careful statement would be:

```text
The observed activity involves a valid internal account transferring large amounts of data to an external file-sharing service outside normal business hours. The current evidence does not determine whether this is malicious insider activity, compromised-account activity or an approved business process.
```

That is more useful than jumping directly to “insider threat”. The Diamond Model helps keep that distinction visible.

## Combining the Diamond Model With MITRE ATT&CK

The Diamond Model and MITRE ATT&CK answer different questions.

| Framework     | Useful for                                                                             |
| ------------- | -------------------------------------------------------------------------------------- |
| Diamond Model | Structuring the relationship between adversary, infrastructure, capability and victim. |
| MITRE ATT&CK  | Describing adversary behaviour using tactics and techniques.                           |

They work well together. For example, if the hunter observes scheduled task creation, ATT&CK can help describe the technique. The Diamond Model helps connect that technique to the victim, infrastructure and possible adversary context.

| Observation                            | ATT&CK angle                | Diamond angle        |
| -------------------------------------- | --------------------------- | -------------------- |
| Scheduled task created                 | Persistence                 | Capability           |
| Task runs suspicious binary            | Execution / Persistence     | Capability           |
| Binary contacts external domain        | Command and Control         | Infrastructure       |
| Activity occurs on finance workstation | Targeting / impact context  | Victim               |
| Behaviour overlaps known reporting     | Threat intelligence context | Adversary assessment |

This combination is useful because ATT&CK provides vocabulary, while the Diamond Model provides relationship structure.

When I map observed TTPs, I often use the [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/) to organise techniques and compare behaviour against known reporting. This does not prove attribution, but it helps structure the adversary assessment and makes the reasoning easier to share.

The mistake is to use either framework mechanically. ATT&CK mapping alone does not explain the intrusion. The Diamond Model alone does not validate the behaviour. The analyst still has to reason.

## Using the Model for Handover

One practical value of the Diamond Model is handover. Threat hunts often produce findings that need to be passed to SOC, incident response, detection engineering, threat intelligence or another team. A diamond-structured summary helps the receiving team understand what was found and what remains uncertain.

Example handover:

| Diamond element    | Summary                                                                                                  |
| ------------------ | -------------------------------------------------------------------------------------------------------- |
| Adversary          | Unknown. Activity may involve valid-account misuse. Attribution not established.                         |
| Infrastructure     | External file-sharing service, rare destination for this user group.                                     |
| Capability         | Archive creation followed by large outbound transfer outside business hours.                             |
| Victim             | Workstation and account belonging to user with access to sensitive data.                                 |
| Timestamp          | Activity observed between 22:14 and 23:02 local time.                                                    |
| Result             | Data transfer confirmed. Business justification not yet established.                                     |
| Recommended action | Validate with business owner, review user activity, check for related transfers, preserve relevant logs. |

This is much clearer than handing over a collection of raw indicators. It also helps avoid overstating conclusions. The receiving team can see what is known, what is suspected and what should happen next.

## What Usually Goes Wrong

Several mistakes are common when analysts use the Diamond Model.

| Problem                                       | Why it hurts                                                    |
| --------------------------------------------- | --------------------------------------------------------------- |
| Treating the model as an attribution tool     | Attribution is difficult and should not be forced.              |
| Filling every vertex too early                | Unknowns are normal and should remain visible.                  |
| Treating indicators as complete understanding | Infrastructure alone does not explain the intrusion.            |
| Ignoring victim context                       | The business relevance of the activity may be missed.           |
| Mapping capability too generically            | The behaviour becomes too vague to hunt or detect.              |
| Forgetting the timeline                       | Relationships lose meaning without sequence.                    |
| Using the model only after the investigation  | It is most useful while structuring observations and questions. |

The model should help the analyst reason. It should not become a decorative diagram added at the end of a report.

## Working Position for This Book

The Diamond Model is useful because it helps threat hunters structure intrusion activity around relationships.

It gives the analyst a disciplined way to organise:

```text
Adversary.
Infrastructure.
Capability.
Victim.
```

But the model is not a shortcut to attribution, and it does not replace investigation.

For this book, the Diamond Model is best treated as a practical reasoning and documentation tool. It helps the hunter connect observations, identify gaps, avoid isolated indicator thinking and prepare better handover.

> Use the Diamond Model to organise what you know, expose what you do not know, and explain how the pieces connect.
>
> -- Roger Johnsen

## Resources

* [Diamond Model of Intrusion Analysis - Summary](https://www.threatintel.academy/wp-content/uploads/2020/07/diamond_summary.pdf)
* [Recorded Future: What is the Diamond Model of Intrusion Analysis?](https://www.recordedfuture.com/blog/what-is-the-diamond-model-of-intrusion-analysis)
* [Threat Intelligence Lab: Understanding the Diamond Model of Intrusion Analysis](https://www.threatintelligencelab.com/understanding-the-diamond-model-of-intrusion-analysis/)
* [ThreatConnect: The Importance of the Diamond Model](https://threatconnect.com/blog/importance-of-diamond-model-cyber-threat-intelligence/)
* [The DFIR Report](https://thedfirreport.com/)
* [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/)

## Revision

| Revised Date | Comment                                                                                                                                           |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide for using the Diamond Model during threat hunting, pivoting, documentation and handover. |
| 2024-10-06   | Improved formatting and wording                                                                                                                   |
| 2024-06-23   | Added page                                                                                                                                        |
