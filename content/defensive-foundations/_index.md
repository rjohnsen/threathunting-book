---
title: "Defensive Foundations"
date: 2025-12-29T11:17:16+01:00
draft: true
weight: 2
---

## Why foundations matter

Advanced defensive practices fail surprisingly often, not because they are flawed, but because the environment they are introduced into cannot support them. Threat hunting, detection engineering, and incident response all assume that certain foundational conditions exist. When those conditions are missing or unstable, defensive work becomes reactive, noisy, or performative. Defensive Foundations describe the **preconditions that make learning, validation, and correction possible**.

---

## Foundations are not controls

Defensive foundations are frequently mistaken for tooling or compliance controls. They are neither. They are properties of the system:

- what is known
- what is observable
- what is owned
- what can be changed

Without these properties, defensive work cannot reliably improve over time.

---

## The minimum viable foundations

The following foundations are not exhaustive, but they are non-negotiable.

They do not need to be perfect.
They do need to exist.

---

## Asset awareness

You cannot defend what you do not know exists.

Asset awareness does not mean a static CMDB or an inventory spreadsheet. It means having a **reasonable and current understanding** of:

- systems and services in scope
- ownership and responsibility
- where data lives
- which assets matter most

Blind spots at this level propagate upward.
Hunts miss systems.
Detections fail silently.
Incidents surprise people.

---

## Telemetry you can trust

Telemetry is the raw material of defense.

Logs that are:
- incomplete
- inconsistent
- delayed
- or unaudited

do not create visibility - they create confidence without evidence.

Usable telemetry is:

- intentional
- understood
- verified
- and owned

If you cannot explain *why* a log exists, *what* it should show, and *how* you would notice if it stopped working, it is not a foundation. It is decoration.

---

## Detection ownership

Detections that belong to “the system” belong to no one.

A defensive organization must know:

- who owns a detection
- what it is meant to catch
- how it is validated
- when it should be changed or removed

Ownership enables learning. Without it, alerts accumulate, quality degrades, and failures repeat. Detection ownership is a structural property, not a SOC maturity level.

---

## The ability to change the environment

Threat-Informed Defense assumes that observations can lead to correction.

This requires:

- the authority to adjust logging
- the ability to modify detections
- the capacity to change architecture or configuration
- feedback paths between teams

If defensive teams can observe problems but not act on them, the system cannot learn. In such environments, hunting produces insight - but no improvement.

---

## Foundations as dependencies

Defensive work is layered, whether acknowledged or not.

```mermaid
flowchart TB
    A[Asset Awareness]
    T[Telemetry]
    D[Detections]
    H[Threat Hunting]
    R[Incident Response]
    F[Feedback & Correction]

    A --> T --> D --> H --> R --> F
    F --> D
    F --> T
````

When lower layers are weak, higher layers absorb the strain. This is often misinterpreted as analyst failure or skill gaps, when the real cause is architectural.

---

## When foundations are missing

Common symptoms of weak foundations include:

* hunting that never produces durable outcomes
* detections that are never validated
* incidents that feel “unexpected”
* repeated rediscovery of the same gaps
* growing alert volume with declining confidence

These are not maturity problems.
They are dependency problems.

---

## Readiness is not binary

Foundations are rarely fully present or fully absent.

The relevant question is not:

> "Are we mature enough?"

but:

> "Which assumptions currently hold, and which do not?"

Being explicit about missing foundations allows teams to:

* sequence work correctly
* avoid premature optimization
* explain constraints honestly
* reduce frustration and blame

Saying "not yet" is often the most threat-informed decision available.

---

## Why this matters

Most defensive disappointment originates below the surface.

By treating foundations as first-class concerns, defensive work becomes:

* more predictable
* more honest
* and more correctable

Everything that follows in Huntbook - threat hunting, intelligence handling,
and response - assumes that these foundations are understood.

---

## References and further reading

The following resources have influenced the framing and thinking used on this page.

* Swannman – Incident Response Hierarchy of Needs
  [https://www.incidentresponse.com/blog/incident-response-hierarchy-of-needs/](https://www.incidentresponse.com/blog/incident-response-hierarchy-of-needs/)

* MITRE – Threat-Informed Defense
  [https://www.mitre.org/our-impact/cybersecurity/threat-informed-defense](https://www.mitre.org/our-impact/cybersecurity/threat-informed-defense)

* Detection Engineering
  [https://detectionengineering.net/](https://detectionengineering.net/)

* The DFIR Report
  [https://thedfirreport.com/](https://thedfirreport.com/)

* SANS Institute – Blue Team and SOC research
  [https://www.sans.org/](https://www.sans.org/)
