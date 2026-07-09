---
title: "Hierarchy of Needs"
description: "Why threat hunting depends on basic operational foundations such as asset inventory, telemetry, detection, triage and incident response capability."
date: 2025-02-22T17:46:03+01:00
lastmod: 2026-07-09
draft: false
weight: 2
tags:
    - fundamentals
    - threat hunting
    - telemetry
    - incident response
keywords:
    - threat hunting
    - incident response hierarchy of needs
    - SOC
    - telemetry
    - asset inventory
    - detection
    - triage
    - security operations
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Threat hunting does not start with a clever hypothesis. It starts with the boring layers underneath it: assets, telemetry, detection, triage and incident response capability.**

**Many organisations want threat hunting before they have the foundations needed to support it. They want proactive investigation, but they do not know what they own. They want behavioural analysis, but they do not collect the right telemetry. They want hunters to find what detections miss, but the SOC is still struggling to understand ordinary alerts.**

That gap matters.

Threat hunting can only be as good as the environment it operates in. If the asset inventory is poor, the hunter does not know what matters. If telemetry is missing, the hunter cannot see the behaviour. If triage and incident response are immature, findings may have nowhere useful to go.

This page uses Swannman’s Incident Response Hierarchy of Needs to explain why threat hunting depends on lower layers of operational maturity. The model is useful because it makes one thing clear: advanced security work must be built on something.

---

## Swannman’s Incident Response Hierarchy of Needs

This page is based on [Swannman’s Incident Response Hierarchy of Needs](https://github.com/swannman/ircapabilities). I use it because it explains a problem I have seen many times: organisations try to build advanced security functions before the basic operational layers are ready.

Threat hunting is high in the model for a reason. It depends on the layers below it. A hunter without asset context, telemetry, detection coverage and response capability is mostly left with assumptions.

![Hierarchy](/images/hierarchy.png)

Swannman’s model describes a set of capabilities that build on each other. The lower layers are not glamorous, but they decide whether the upper layers can function.

That is the point.

A SOC cannot reliably detect and respond if it does not know what systems exist, what data is collected, and which alerts matter. Threat hunting has the same dependency. It may be more exploratory than alert triage, but it is still limited by the available context and telemetry.

## Why the Model Matters for Threat Hunting

Threat hunting is often presented as an advanced function, and that is fair enough. But “advanced” does not mean detached from the basics.

A hunt usually needs answers to ordinary questions:

* Which systems are in scope?
* Which identities matter?
* Which logs are collected?
* Which fields are populated?
* How far back does the data go?
* What is normal for this environment?
* Who owns the finding if the hunt discovers something?
* Can the SOC, incident response team or platform team act on the output?

If those questions cannot be answered, the hunt may still be useful, but the organisation should be honest about the limitation. Sometimes the best output from a hunt is not a detection rule. Sometimes it is a visibility gap, a logging requirement, or a clear statement that the current data is not good enough to test the hypothesis.

## The Layers

Each layer in Swannman’s model supports the layers above it. This is how I read the model from a SOC and threat hunting perspective:

| #  | Layer           | Why it matters                                                                                    |
| --- | --------------- | ------------------------------------------------------------------------------------------------- |
| 1  | Asset Inventory | You cannot protect, monitor or hunt across systems you do not know exist.                         |
| 2  | Telemetry       | Hunting depends on observable data. Missing telemetry means missing behaviour.                    |
| 3  | Detection       | Detection gives the SOC known signals and gives hunters something to test, challenge and improve. |
| 4  | Triage          | Alerts must be understood and prioritised before they can become useful hunting input.            |
| 5  | Threats         | Threat intelligence helps shape questions about actors, behaviours and likely techniques.         |
| 6  | Behaviours      | Behavioural understanding moves the team beyond simple indicators and isolated alerts.            |
| 7  | Hunt            | Hunting tests assumptions, searches for missed behaviour and identifies visibility gaps.          |
| 8  | Track           | Tracking keeps attention on adversary activity over time, not just isolated events.               |
| 9  | Act             | Findings must lead to containment, remediation, detection improvement or other action.            |
| 10 | Collaboration   | Mature teams share useful intelligence, context and lessons with trusted partners.                |

The order matters, but it should not be read as a perfect maturity staircase. Real organisations are uneven. They may have strong endpoint telemetry but poor asset ownership. They may have good detections but weak triage. They may have incident response plans that look good on paper but are rarely exercised.

The model is still useful because it forces a basic question: _Which layer is weak, and what does that weakness do to the work above it?_ That question is often more useful than asking whether the organisation is “mature”.

## The Plateaus Model

Swannman’s Plateaus Model complements the hierarchy by showing how organisations move through levels of capability over time.

![Plateaus](/images/plateaus.png)

Most maturity models become too abstract. This one is useful because it is easy to understand: an organisation does not become good at incident response, SOC operations or threat hunting in one leap. It reaches plateaus. Each plateau gives the organisation a stronger foundation for the next type of work.

The plateaus are not strictly linear. An organisation may need to go back and fix weak telemetry after it has already started building detections. It may discover during threat hunting that its asset inventory is not good enough. It may build an incident response process and later realise that triage does not produce the context responders need.

That is normal.

The important part is not pretending that the upper layers can compensate for weak lower layers indefinitely.

## My Reading of the Plateaus

The following table is my practical reading of the plateaus from a SOC and threat hunting perspective:

| # | Plateau                                                | My reading                                                                                            |
| --- | ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| 1 | Basic Detection and Awareness                          | The organisation starts to understand what it owns and what it can observe. This is the bare minimum. |
| 2 | Incident Response and Initial Triage                   | Alerts and incidents become structured enough that people know what to do next.                       |
| 3 | Advanced Detection and Threat Intelligence Integration | Detection and intelligence begin to shape each other, but the work is still mostly reactive.          |
| 4 | Proactive Threat Hunting and Vulnerability Management  | The team can start asking better questions because enough data, process and context exist.            |
| 5 | Full Operational Maturity                              | Hunting, detection, response and collaboration form a feedback loop instead of separate activities.   |

The fourth plateau is where threat hunting starts to make more sense. That does not mean an organisation must be perfect before it can hunt. It means the organisation needs enough structure to make the hunt useful.

A weak environment can still run hunts, but the output will often expose foundation problems rather than hidden attackers. That is not failure. It is useful knowledge, as long as someone acts on it.

## What Usually Goes Wrong

The common failure mode is building the visible function before the supporting layers.

Some patterns repeat:

* **SOC as a log viewer:** the organisation has a SOC, but the analysts mostly watch disconnected alerts without enough asset context, telemetry quality or response authority.
* **SIEM before asset understanding:** logs are collected before anyone has a clear view of what systems exist, which ones matter, and who owns them.
* **Detection without data ownership:** rules are written, but nobody owns the log source quality, parsing, retention or missing fields needed to make those rules reliable.
* **Threat hunting as a reporting function:** hunters are expected to produce interesting reports, but findings do not become detections, logging requirements, response actions or better analyst guidance.
* **Maturity by job title:** the organisation hires a threat hunter and assumes it has created threat hunting capability.

That does not mean the organisation should wait for perfection. It means the team must be honest about which layer is weak:

* If the asset inventory is poor, hunting will miss scope.
* If telemetry is poor, hunting will miss behaviour.
* If detection is poor, hunting will not have a reliable baseline.
* If triage is poor, hunting output will not feed cleanly back into operations.
* If response capability is poor, findings may create awareness without action.

Threat hunting can help expose these weaknesses, but it cannot magically compensate for all of them.

## Why This Matters in Practice

The hierarchy is useful because it prevents a common misunderstanding: that threat hunting is mainly a staffing or tooling decision. It is not.

Hiring a threat hunter does not automatically create threat hunting capability. Buying a tool with a hunting module does not automatically create threat hunting capability. Adding “proactive hunting” to a SOC service description does not automatically create threat hunting capability. The capability depends on the layers underneath.

A threat hunter needs data, access, context, time, documentation and a place for the output to go. Without that, the work becomes fragile. It may depend too much on individual effort. It may produce findings that cannot be operationalised. It may create reports that people read once and then forget.

That is not how hunting becomes useful. Threat hunting becomes useful when it feeds the rest of security operations:

* visibility gaps become logging requirements
* suspicious behaviours become detection ideas
* repeated observations become baselines
* findings become response actions
* weak triage logic becomes better analyst guidance
* assumptions become testable questions

That is the real value of the hierarchy. It shows that hunting is not separate from the rest of security operations. It depends on it and should improve it.

## Working Position for This Book

Swannman’s model is useful because it makes threat hunting less mystical. Threat hunting is not something an organisation simply decides to have. It becomes possible when enough operational layers are in place: asset knowledge, telemetry, detection, triage, threat understanding, behavioural analysis and response capability.

The layers do not need to be perfect. They rarely are. But they need to be understood. For the rest of this book, this matters because every hunt depends on the same basic question: _Do we have enough context and telemetry to test what we claim to be testing?_

If the answer is no, the hunt may still produce value. It may reveal a visibility gap, a logging requirement, a weak process or an assumption that needs to be fixed. That is still useful.

But it also means the organisation should be honest about where it is in the hierarchy.

> Threat hunting built on weak foundations usually becomes guesswork with better branding.
>
> -- Roger Johnsen

## Revision

| Revised Date | Comment                                                                                                     |
| ------------ | ----------------------------------------------------------------------------------------------------------- |
| 2026-07-09   | Rewritten to establish a clearer practitioner voice and align the page with the book’s fundamentals section |
| 2025-02-22   | Added page                                                                                                  |
