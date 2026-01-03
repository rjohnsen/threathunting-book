---
title: "Threat Informed Defense"
date: 2025-12-29T11:16:55+01:00
draft: true
weight: 4
---

## Threat-Informed Defense as an operating model

Threat-Informed Defense (TID) is not a framework, a control set, or a maturity model. It is a way of thinking about defense where **real adversary behavior is used to shape, test, and correct defensive capability over time**.

In a threat-informed organization, threats are not used to generate urgency or justify tooling. They are used to **prioritize effort, validate assumptions, and force learning**.

Defense is treated as a system - one that can be observed, challenged, and improved.

---

## What Threat-Informed Defense is not

Threat-Informed Defense is **not**:

- mapping controls to threat actors and calling it strategy  
- adding detections because a report says “this is trending”  
- consuming threat intelligence without changing defensive posture  
- compensating for missing foundations with activity  

When threats are used only as justification, naming, or decoration, defense becomes performative.

---

## The core idea

At its core, Threat-Informed Defense is simple:

> **Threats are used to test assumptions about visibility, detection, and response. The outcome of those tests feeds back into the system.**

This implies several uncomfortable but necessary truths:

- claimed coverage must be validated  
- detections must be exercised, not assumed  
- blind spots must be acknowledged, not rationalized  
- failure is expected - and documented  

Threats are not predictions. They are probes.

---

## Defense as a feedback loop

Threat-Informed Defense only works when feedback exists. The purpose is not activity, but **correction**.

```mermaid
flowchart LR
    TI[Threat Intelligence] --> H[Hypotheses]
    H --> V[Hunting & Validation]
    V --> O[Observed Results]
    O -->|Gaps & Failures| C[Corrections]
    O -->|Confirmed Assumptions| C
    C --> D[Detections & Telemetry]
    D --> H
````

Without this loop, security work drifts toward compliance, ritual, or noise.

---

## What threat intelligence means in this book

In Huntbook, **threat intelligence is contextualized knowledge about adversary behavior that can influence defensive decisions**.

Its value is not measured by volume, freshness, or attribution.
It is measured by whether it changes one or more of the following:

* what is logged
* what is detected
* what is tested
* how response is shaped

Threat intelligence that only informs awareness, but does not alter defensive posture, remains informational - not operational.

This book treats intelligence as something that is both **consumed and produced**.

---

## Intelligence flows in both directions

A threat-informed system does not rely solely on external input.

External intelligence:

* highlights relevant behaviors
* informs prioritization
* suggests where assumptions should be challenged

Internal intelligence is produced through:

* threat hunting
* detection validation
* incident response
* observed failure and absence

If intelligence does not survive contact with telemetry, detection, or response, it does not meaningfully contribute to defense.

---

## Where threat hunting fits

Threat hunting is not the goal of Threat-Informed Defense.
It is **one of its mechanisms**.

Hunting is used to:

* validate detection claims
* discover unknown or unexpected behavior
* surface blind spots in telemetry
* challenge optimistic assumptions

Hunting without feedback is exploration.
Hunting without correction is theater.

---

## Preconditions matter

Threat-Informed Defense assumes that certain foundations exist, or are at least understood:

* awareness of assets
* usable telemetry
* ownership of detections
* the ability to change the environment

Without these, TID becomes aspirational rather than practical.

Recognizing missing foundations is part of being threat-informed.

---

## Why this matters

Most defensive failure is not caused by a lack of tools, effort, or intelligence.
It is caused by **systems that cannot learn from reality**.

Threat-Informed Defense anchors defensive work in evidence:
what was observed, what failed, what changed.

It replaces reassurance with correction.

---

## References and further reading

The following resources have influenced the thinking and framing used on this page.
They are provided for readers who want additional depth or alternative perspectives.

* MITRE – Threat-Informed Defense
  [https://www.mitre.org/our-impact/cybersecurity/threat-informed-defense](https://www.mitre.org/our-impact/cybersecurity/threat-informed-defense)

* MITRE ATT&CK Framework
  [https://attack.mitre.org/](https://attack.mitre.org/)

* The Pyramid of Pain – David Bianco
  [https://www.sans.org/blog/the-pyramid-of-pain/](https://www.sans.org/blog/the-pyramid-of-pain/)

* Detection Engineering
  [https://detectionengineering.net/](https://detectionengineering.net/)

* The DFIR Report
  [https://thedfirreport.com/](https://thedfirreport.com/)

* SANS Institute – Blue Team and Threat Hunting Research
  [https://www.sans.org/](https://www.sans.org/)
