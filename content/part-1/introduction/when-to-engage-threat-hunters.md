---
title: "When to Engage Threat Hunters"
description: "When threat hunters should be involved, how they complement SOC triage, incident response and DFIR, and when hunting is the wrong tool for the job."
date: 2025-03-29T15:04:35+01:00
lastmod: 2026-07-09
draft: false
hidden: false
weight: 11
tags:
    - fundamentals
    - threat hunting
    - SOC
    - incident response
keywords:
    - threat hunters
    - when to engage threat hunters
    - threat hunting
    - SOC triage
    - incident response
    - DFIR
    - detection gaps
    - proactive hunting
    - incident response lifecycle
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Threat hunters should be engaged when the organisation has a question that cannot be answered by alerts alone.**

That question may appear before an incident, during SOC triage, during incident response, after a major incident, or when new threat intelligence raises concern about activity that existing detections may not cover.

Threat hunting is not a replacement for alerting, SOC triage, incident response or digital forensics. It is a complementary capability. The hunter brings a different way of working: hypothesis-driven investigation, deep log analysis, adversary understanding, telemetry knowledge and the ability to search for behaviour that existing rules may not detect.

Threat hunters are not useful because they are “better analysts” who should be added to everything. They are useful when the problem requires deeper investigation, broader context, attacker tradecraft knowledge or custom analysis that normal alert handling does not provide.

> Threat hunters should be engaged when the organisation has a question that cannot be answered by alerts alone.
>
> -- Roger Johnsen

## The Role of Threat Hunters

Threat hunters are useful when the organisation needs to go deeper than ordinary alert handling. They are often brought in when available alerts do not fully explain what is happening, when the SOC needs help interpreting weak or scattered signals, or when threat intelligence must be translated into searches that make sense in the local environment.

A good threat hunter usually has insight into how attackers operate. That insight may come from threat intelligence, incident investigations, log analysis, detection work, red team collaboration or direct experience with adversary behaviour in real environments.

In practice, threat hunters can support the organisation by:

* digging deeply into logs
* testing hypotheses
* correlating data across sources
* interpreting attacker tradecraft
* translating threat intelligence into local searches
* identifying detection gaps
* finding related activity
* building custom search or enrichment tooling
* creating hunting packages, triage notes or detection ideas

I have worked in SOC environments where my threat hunting skills had to be used outside a neatly defined hunting programme. Sometimes the task was deep log analysis. Sometimes it was interpreting threat intelligence and translating it into searches. Sometimes it was building custom search or enrichment tools in Go or Python because the existing tooling did not answer the question well enough.

That experience shaped my view of when threat hunters should be engaged. They are not only useful when the organisation has a formal hunt planned. They are useful when the question requires adversary understanding, telemetry knowledge and enough technical flexibility to find an answer.

## Threat Hunting Is Not Incident Response

Threat hunters should not replace incident response personnel. During an active incident, incident response must own the response process: coordination, containment, eradication, recovery, evidence handling, communication and decision-making. Threat hunters may support that work, but they should not take over the incident response function. Their value is different.

Threat hunters can help identify related activity, search for attacker behaviour across broader telemetry, test whether the incident is isolated or part of a wider pattern, interpret threat intelligence, and enrich the response with context that may not be visible from the original alert or compromised host.

In other words, threat hunters can help pull the load, but incident response should still drive the incident.

> Threat hunters can enrich incident response, but they should not replace incident responders.
>
> -- Roger Johnsen

## Threat Hunting Is Not a Replacement for SOC Triage

Threat hunters should not replace SOC triage either. SOC triage exists to handle alerts, apply playbooks, assess severity, escalate incidents and keep operational monitoring moving. If every alert requires a threat hunter, the SOC model is broken. Threat hunters should be involved when triage reaches the edge of what the normal process can answer.

Examples:

| SOC triage can usually handle                         | Threat hunters may help when                                        |
| ----------------------------------------------------- | ------------------------------------------------------------------- |
| A known alert has a clear playbook.                   | The alert is part of a wider pattern that is not understood.        |
| The event matches expected false-positive conditions. | The false-positive explanation is uncertain or weak.                |
| The alert has clear evidence and escalation criteria. | The alert raises questions about related activity elsewhere.        |
| The activity is already covered by detection logic.   | The team suspects similar activity may be missed by existing logic. |
| The case is contained to one event or host.           | The scope, timeline or technique is unclear.                        |

The point is not to make hunting responsible for triage. The point is to let hunting help when triage needs deeper context.

> If the problem already has a known playbook, start with the playbook. If the problem requires a hypothesis, context and deep log work, bring in the hunters.
>
> -- Roger Johnsen

## When to Engage Threat Hunters

Threat hunters should be engaged when the question is broader, deeper or less certain than normal alert handling.

Useful scenarios include:

| Scenario                                          | Why threat hunters help                                                       |
| ------------------------------------------------- | ----------------------------------------------------------------------------- |
| Existing alerts do not explain the activity       | Hunters can look beyond the alert and test related hypotheses.                |
| SOC sees weak or scattered signals                | Hunters can correlate activity across users, hosts, identities and time.      |
| Alert volume suddenly drops                       | Hunters can investigate whether telemetry, detection or ingestion has failed. |
| New threat intelligence becomes relevant          | Hunters can translate external reporting into local searches.                 |
| A major incident has been contained               | Hunters can look for related activity, persistence or missed scope.           |
| A detection gap is suspected                      | Hunters can test what current rules may fail to see.                          |
| A red team or purple team found weaknesses        | Hunters can search for similar behaviour in historical telemetry.             |
| A critical asset or identity requires assurance   | Hunters can examine activity around high-value targets.                       |
| Behaviour is suspicious but not clearly malicious | Hunters can build context before conclusion.                                  |
| Existing tooling cannot answer the question       | Hunters can build custom queries, enrichment or scripts.                      |

Threat hunters are especially useful when the answer requires several things at once:

* a hypothesis to test
* telemetry that can support or weaken the hypothesis
* context about users, systems and normal behaviour
* adversary understanding
* validation before conclusion

That combination is what separates hunting from ordinary searching.

## When Not to Engage Threat Hunters

Threat hunting is not always the right primary tool. There are situations where another function should lead.

| Situation                                     | Primary owner                               |
| --------------------------------------------- | ------------------------------------------- |
| Known alert with a clear playbook             | SOC triage                                  |
| Confirmed active compromise                   | Incident response                           |
| Forensic preservation and evidence collection | DFIR                                        |
| Malware reverse engineering                   | Malware analysis or DFIR specialist         |
| Vulnerability remediation                     | Vulnerability management or platform owner  |
| Policy violation without adversary hypothesis | Governance, HR, legal or management process |
| Routine alert tuning                          | Detection engineering or SOC engineering    |
| Compliance evidence collection                | GRC or control owner                        |
| User awareness issue                          | Security awareness or management            |

Threat hunters may still support some of these activities, but support is not the same as ownership. This distinction protects the hunting capability from becoming a catch-all function.

If threat hunters are used for everything, they eventually stop hunting.

## How Threat Hunters Support SOC Triage

Threat hunters can help SOC triage when an alert raises questions that the playbook does not answer. For example, a SOC analyst may receive an alert for suspicious PowerShell execution. The playbook may help determine whether the activity is obviously malicious or benign on that host. But the analyst may still need help answering broader questions:

* Is this behaviour rare in the environment?
* Has the same command appeared elsewhere?
* Is the parent process unusual?
* Is the user expected to run this?
* Did similar activity occur before the alert?
* Are there related DNS, proxy or identity events?
* Is this part of a larger pattern?

Those are hunting questions. Threat hunters can help by expanding the investigation across telemetry, building timelines, testing related hypotheses and identifying whether the alert represents an isolated event or a visible piece of something larger.

The output should help the SOC, not bypass it. Possible outputs include:

| Output                    | SOC value                                                      |
| ------------------------- | -------------------------------------------------------------- |
| Baseline                  | Helps analysts understand whether behaviour is common or rare. |
| Triage guidance           | Improves future handling of similar alerts.                    |
| Related activity          | Helps determine whether the alert is isolated.                 |
| Detection gap             | Shows what current alerting did not cover.                     |
| Enrichment logic          | Improves alert context.                                        |
| Escalation recommendation | Helps decide whether incident response should be involved.     |

## How Threat Hunters Support Incident Response and DFIR

Threat hunters can be valuable during and after incident response, but the role must be clear. Incident response should lead the incident. DFIR should lead forensic collection, preservation, forensic analysis and evidence handling where that is required. Threat hunters can support by expanding the view across the environment.

During an incident, threat hunters may help answer:

* Is this activity isolated?
* Are there related events on other hosts?
* Did the same user, tool or command appear elsewhere?
* Are there signs of persistence outside the original system?
* Are there earlier signs of initial access?
* Are there lateral movement indicators?
* Does threat intelligence suggest additional behaviours to search for?
* Are detections missing parts of the attack path?

After an incident, threat hunters may help identify:

* missed detection opportunities
* related techniques
* visibility gaps
* new hypotheses
* follow-up hunts
* detection engineering requirements
* improved triage guidance
* lessons for future response

This is where hunting enriches response. It can help incident responders understand scope, related activity and attacker behaviour across telemetry that may not be part of the original forensic focus. But the response process should remain owned by incident response.

## How Threat Hunters Support Detection Engineering

Threat hunting and detection engineering are closely related. A hunt may find behaviour that should become a detection. It may also reveal that an existing detection is too narrow, too noisy, too dependent on weak fields or missing important context.

Threat hunters can support detection engineering by providing:

| Hunting output        | Detection engineering value                    |
| --------------------- | ---------------------------------------------- |
| Behaviour description | Explains what the detection should look for.   |
| Query logic           | Provides a starting point for detection logic. |
| False-positive notes  | Helps tune the detection.                      |
| Required fields       | Identifies telemetry dependencies.             |
| Baseline information  | Helps define rarity or expected behaviour.     |
| Triage guidance       | Helps analysts interpret future alerts.        |
| Test cases            | Helps validate detection quality.              |
| Visibility gaps       | Shows where detection is not possible yet.     |

A good handover does not simply say:

```text
We saw suspicious activity.
```

It should say:

```text
We observed Office applications spawning command interpreters on ordinary user workstations. This behaviour is rare in our environment outside known packaging activity. Detection should include parent process, child process, command-line content, user context, device role and follow-on network activity.
```

That is useful to detection engineering because it describes behaviour, context and operational constraints.

## Decision Guide

A simple decision guide can help clarify when threat hunters should be engaged.

| Question                                                                          | If yes                             |
| --------------------------------------------------------------------------------- | ---------------------------------- |
| Is there already a clear SOC playbook?                                            | Start with SOC triage.             |
| Is there confirmed active compromise requiring containment?                       | Incident response should lead.     |
| Is forensic preservation required?                                                | DFIR should lead.                  |
| Is the question about attacker behaviour not covered by existing alerts?          | Engage threat hunters.             |
| Does the team need to search broadly across telemetry?                            | Engage threat hunters.             |
| Does threat intelligence need to be translated into local searches?               | Engage threat hunters.             |
| Is the scope unclear or possibly wider than the original alert?                   | Threat hunters can support.        |
| Is the outcome likely to become a detection idea, visibility gap or hunt package? | Threat hunters should be involved. |

This is not a rigid model. It is a way to avoid role confusion. The practical rule is simple:

```text
Known alert and known process: triage.
Active incident: incident response.
Forensic evidence: DFIR.
Unknown behaviour, uncertain scope or detection gap: threat hunting.
```

## What Usually Goes Wrong

Several problems appear when organisations misunderstand when to engage threat hunters.

| Problem                                              | Why it hurts                                                                             |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Threat hunters are treated as senior triage analysts | Hunting becomes alert handling with a different title.                                   |
| Threat hunters replace incident responders           | Incident coordination, containment and recovery suffer.                                  |
| Threat hunters are engaged too late                  | The organisation misses the chance to identify related activity or detection gaps early. |
| Threat hunters are engaged for every unusual alert   | The capability becomes overloaded and unfocused.                                         |
| Hunting output has no owner                          | Findings, gaps and detection ideas do not improve operations.                            |
| Threat intelligence is not translated                | Reports are read, but not turned into local searches.                                    |
| Scope is unclear                                     | Hunters are pulled into open-ended investigations without boundaries.                    |
| Custom tooling becomes invisible                     | Scripts and searches help once, but are not documented or reused.                        |

Most of these problems come from unclear role boundaries. Threat hunters should not be treated as a replacement for other security functions. They should be used where their way of thinking adds value.

## Working Position for This Book

Threat hunters should be engaged when the organisation needs hypothesis-driven investigation, adversary understanding, deep telemetry work or context that alerts alone cannot provide. They can support SOC triage, incident response, DFIR and detection engineering. They can help find related activity, identify detection gaps, translate threat intelligence, build custom searches and enrich the organisation’s understanding of attacker behaviour.

But they should not replace the functions they support. A mature security organisation understands when to use each capability.

* SOC triage handles known alerts.
* Incident response leads active incidents.
* DFIR handles forensic evidence and analysis.
* Detection engineering builds and improves detections.
* Threat hunting tests questions that alerts alone cannot answer.

That is the distinction that matters.

> Threat hunters are not there to own every hard problem. They are there to help answer the questions that require hypotheses, telemetry, adversary understanding and context.
>
> -- Roger Johnsen

## Resources

* [NIST SP 800-61 Rev. 3: Incident Response Recommendations and Considerations for Cybersecurity Risk Management](https://csrc.nist.gov/pubs/sp/800/61/r3/final)
* [The DFIR Report](https://thedfirreport.com/)
* [MITRE ATT&CK](https://attack.mitre.org/)
* [SANS Threat Hunting Summit](https://www.sans.org/cyber-security-training-events/threat-hunting-summit/)
* [Open Threat Hunting Framework](https://www.threathunting.net/)

## Revision

| Revised Date | Comment                                                                                                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-09   | Major rewrite. Reframed the article as a decision guide for when to engage threat hunters and how they complement SOC, incident response, DFIR and detection engineering. |
| 2025-03-29   | Added page                                                                                                                                                                |
