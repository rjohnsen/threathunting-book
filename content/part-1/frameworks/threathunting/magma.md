---

title: "MaGMa"
description: "A practical explanation of MaGMa and how it can support SOC use case management, threat hunting, detection engineering and continuous improvement."
date: 2025-04-13T10:45:06+02:00
lastmod: 2026-07-10
draft: false
hidden: false
weight: 3
tags:
    - frameworks
    - threat hunting
    - SOC
    - use case management
keywords:
    - MaGMa
    - MaGMa Use Case Framework
    - use case management
    - SOC
    - security monitoring
    - threat hunting
    - detection engineering
    - TaHiTI
    - metrics
    - assessment
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**MaGMa is a use case management framework for organising, maintaining, measuring and improving security monitoring use cases.**

MaGMa is not a threat hunting model in the same way that the Diamond Model, MITRE ATT&CK, the Kill Chain models or the OODA Loop are hunting and analysis models. It is better understood as a framework for managing the security monitoring and detection use cases that SOCs, hunters and detection engineers depend on.

The framework was created through collaboration between financial institutions associated with the Dutch Financial Information Sharing and Analysis Community, FI-ISAC. MaGMa stands for **Management, Growth and Metrics & assessment**. The framework document describes MaGMa as both a framework and a supporting tool for managing use cases in a SOC.

For threat hunting, this is useful because hunts should not live as isolated activities. A good hunt may produce a detection idea, a visibility gap, a use case improvement, a response recommendation or a better understanding of normal behaviour. If those outputs are not managed, they can easily disappear into reports, tickets, notes or memory.

MaGMa helps answer a practical question:

```text
How do we make sure that what we learn from security monitoring, threat hunting and incident response becomes managed security capability?
```

That makes MaGMa especially relevant to the parts of threat hunting that happen after the interesting investigation work: documentation, ownership, lifecycle management, metrics, maturity and continuous improvement.

> A hunt that finds something useful but leaves no managed improvement behind is easy to forget.
>
> -- Roger Johnsen

## What MaGMa Is

MaGMa is built around the idea that a SOC needs structured use case management. In the MaGMa documentation, a use case is defined as a security monitoring scenario aimed at detecting manifestations of a cyber threat. The model also connects use cases to strategic, tactical and operational components.

That is important because a detection use case is not only a rule. A mature use case should explain:

* what threat it is meant to address
* why the use case matters to the organisation
* which business or compliance drivers it supports
* what telemetry it requires
* what detection logic or monitoring rules implement it
* who owns it
* how it is validated
* how alerts should be handled
* how it is maintained over time
* how well it performs

Without that structure, use cases tend to decay. Rules are added but not owned. Alerts fire but are not improved. False positives become accepted as normal. Detection gaps are known but not managed. Hunts produce useful findings, but nobody turns them into durable security work.

That is the real cost of poor use case management: knowledge loss, repeated work, inconsistent detections, analyst frustration and security monitoring content that slowly stops matching the environment it was built for.

MaGMa is designed to prevent that kind of drift.

## The Three Layers of a Use Case

A useful part of MaGMa is the separation of use cases into layers. The documentation describes three layers:

| Layer                | Purpose                                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------------------- |
| Business layer       | Connects the use case to business needs, risk, compliance or organisational drivers.                     |
| Threat layer         | Describes the threat the use case is intended to address.                                                |
| Implementation layer | Describes the operational implementation, such as log sources, scope, detection mechanisms and response. |

This layering is valuable because many SOC and detection discussions start too low. A team may begin with a rule, a query or an alert. That is operationally useful, but it does not explain why the use case matters, what threat it addresses or whether the organisation has the necessary telemetry and response process.

MaGMa encourages the team to connect the technical implementation back to threat and business context. That is useful for threat hunting as well. A hunt finding should not only become “new rule created”. It should be possible to connect that rule to the threat being addressed, the business risk being reduced and the operational process that will handle the alert.

## Management

The first part of MaGMa is **Management**. This is about lifecycle management of use cases. The MaGMa documentation describes four phases: onboarding, operational, maintenance and offloading.

| Phase       | Meaning                                                                                                |
| ----------- | ------------------------------------------------------------------------------------------------------ |
| Onboarding  | Plan, document and operationalise a new use case.                                                      |
| Operational | Run the use case as part of daily security monitoring.                                                 |
| Maintenance | Improve or change the use case when threats, business needs, infrastructure or lessons learned change. |
| Offloading  | Decommission the use case when it is no longer required.                                               |

This is a simple idea, but it solves a real problem. Security monitoring content has a lifecycle whether the organisation manages it or not. A detection can become outdated. A log source can disappear. A business process can change. A threat can evolve. A noisy alert can become ignored. A useful hunt can reveal that an existing use case needs better telemetry or better triage guidance.

If there is no lifecycle process, these changes happen silently. Management is the part of MaGMa that makes those changes visible.

## Growth

The second part of MaGMa is **Growth**. Growth is about improving capability and maturity over time.

The MaGMa documentation separates these ideas. Capability deals with how well the SOC is able to detect security threats, while maturity deals with whether the SOC can provide effective security monitoring continuously and improve through a standardised process.

Capability asks:

* Can we detect this?
* Do we have the telemetry?
* Do we have the logic?
* Do we have the analyst knowledge?
* Do we have enough coverage?

Maturity asks:

* Is this documented?
* Is it repeatable?
* Is it owned?
* Is it maintained?
* Is it measured?
* Does it improve over time?

A SOC may have strong technical capability but weak maturity. That usually means smart people are doing good work, but the work is not repeatable enough. Another SOC may have documentation and process, but weak capability. That usually means the framework looks organised while the detection content does not actually perform well.

MaGMa is useful because it forces both questions into the conversation.

## Metrics and Assessment

The third part of MaGMa is **Metrics & assessment**. This is where the framework becomes measurable. The MaGMa documentation describes metrics for assessing effectiveness, implementation level and coverage, along with control metrics and output metrics such as alert counts, incident counts, false-positive ratio and false-negatives.

Metrics are useful because they help the SOC understand whether use cases are working. But metrics can also mislead if they are treated carelessly.

For example, a high alert count does not necessarily mean strong detection. It may mean noise. A low alert count does not necessarily mean the threat is absent. It may mean the use case has poor visibility. A low false-positive rate may look good, but only if the detection still catches the behaviour it was designed to catch.

Metrics can also be gamed, intentionally or unintentionally. A team can reduce false positives by making a detection so narrow that it barely detects anything. A use case can look healthy on a dashboard while failing to cover the behaviour it was created for.

Useful MaGMa-style questions include:

| Question                                                 | Why it matters                                                   |
| -------------------------------------------------------- | ---------------------------------------------------------------- |
| Does the use case have the required telemetry?           | Determines whether detection is possible.                        |
| Is the use case implemented across the right scope?      | Prevents partial coverage from being mistaken for full coverage. |
| Is the use case effective?                               | Tests whether it actually detects meaningful behaviour.          |
| Is the false-positive rate acceptable?                   | Helps measure operational usability.                             |
| Are false-negatives known and tracked?                   | Keeps detection gaps visible.                                    |
| Is the use case maintained when the environment changes? | Prevents silent decay.                                           |

Metrics should help steer improvement. They should not become decorative reporting.

> Metrics are useful when they improve decisions. They are dangerous when they only make weak capability look controlled.
>
> -- Roger Johnsen

## MaGMa and Threat Hunting

MaGMa is not the hunting process itself. It does not replace hypothesis creation, data analysis, pivoting, validation or adversary reasoning. Those parts still belong to the hunter. Where MaGMa becomes useful is in managing what hunting produces.

A threat hunt may result in:

* a confirmed finding
* a rejected hypothesis
* a new detection idea
* a telemetry gap
* a noisy use case
* a response gap
* a new enrichment requirement
* a lesson learned
* a recommendation for control improvement

Without a management structure, these outputs may be handled inconsistently. MaGMa gives the organisation a way to ask:

* Does this hunt output affect an existing use case?
* Should it create a new use case?
* Does it change detection logic?
* Does it reveal a telemetry gap?
* Does it change response guidance?
* Does it affect maturity or capability?

That is why MaGMa can be useful in a threat hunting programme. It helps ensure that hunts contribute to managed detection and monitoring capability instead of remaining one-off investigations.

## MaGMa and Detection Engineering

Detection engineering also fits naturally with MaGMa. A detection engineer needs more than a query or rule. A useful detection should have context, purpose, telemetry requirements, validation criteria, expected response and lifecycle ownership. MaGMa helps structure those elements.

For example:

| Detection engineering concern | MaGMa-style question                                       |
| ----------------------------- | ---------------------------------------------------------- |
| Purpose                       | What threat or risk does this use case address?            |
| Telemetry                     | Which data sources are required?                           |
| Scope                         | Which systems, identities or environments are covered?     |
| Logic                         | Which detection rules or analytics implement the use case? |
| Validation                    | How has the use case been tested?                          |
| Response                      | What should analysts do when it fires?                     |
| Maintenance                   | What should trigger review or update?                      |
| Metrics                       | How do we know whether it is useful?                       |

In practice, this does not have to live in one dedicated MaGMa tool. The structure can be implemented through a combination of SIEM content repositories, case management, SOAR workflows, detection-as-code, documentation platforms and use case registers. The important part is not the tool name. The important part is that the use case has ownership, context, lifecycle, metrics and a path for improvement.

This is where MaGMa complements frameworks such as MITRE ATT&CK and the Pyramid of Pain. ATT&CK may help describe the behaviour being detected. The Pyramid of Pain may help assess whether the detection targets something fragile or durable. MaGMa helps manage the use case so that it remains documented, owned, measured and improved.

## Practical Example: Phishing Campaign

Consider a threat intelligence report describing a phishing campaign targeting employees. The hunting team decides to investigate whether the campaign affected the organisation.

Without use case management, the hunt might produce a report, a few indicators, a detection idea and some informal recommendations. That may be useful in the moment, but it is easy to lose over time.

With MaGMa thinking, the output can be connected to managed use cases.

| Hunt activity         | MaGMa use case angle                                                           |
| --------------------- | ------------------------------------------------------------------------------ |
| Define objective      | What threat or business risk does this hunt address?                           |
| Define scope          | Which users, mailboxes, endpoints, proxies and identity logs are in scope?     |
| Write hypotheses      | Which behaviours should indicate successful phishing or credential harvesting? |
| Execute queries       | Which data sources and queries were used?                                      |
| Record evidence       | Which users, hosts, messages, links or logins were observed?                   |
| Document outcome      | Was the hypothesis supported, rejected or partially supported?                 |
| Recommend improvement | Does an existing phishing, identity or endpoint use case need improvement?     |
| Track follow-up       | Who owns remediation, detection tuning or telemetry improvement?               |

For example, the hunt may show that several users received the phishing email, one user clicked the link and one identity later showed suspicious login behaviour. The immediate response may be account protection and incident handling.

The MaGMa value appears after that:

* the phishing detection use case may need better link-click visibility
* the identity monitoring use case may need improved impossible-travel handling
* the response guidance may need a clearer password reset and token revocation process
* the hunt result may become evidence for increasing email telemetry coverage
* the lesson learned may feed future phishing hunts

This is how a hunt becomes more than a report. It becomes input to managed security monitoring improvement.

## MaGMa and TaHiTI

MaGMa is often mentioned together with TaHiTI, the Targeted Hunting integrating Threat Intelligence framework. That pairing makes sense, but they should not be confused. TaHiTI is a threat hunting methodology. It helps structure how hunting integrates threat intelligence and how hunts are planned and executed.

MaGMa is better understood as a use case management framework. It helps manage security monitoring use cases, including the use cases that may be created, improved or validated through threat hunting.

A simple way to separate them is:

| Framework | Useful for                                                                 |
| --------- | -------------------------------------------------------------------------- |
| TaHiTI    | Planning and executing threat hunts informed by threat intelligence.       |
| MaGMa     | Managing, measuring and improving security monitoring use cases over time. |

Together, they can support a more mature hunting and detection programme. TaHiTI can help guide the hunt. MaGMa can help make sure useful outputs become managed capability.

## Where MaGMa Fits With Other Frameworks

MaGMa fits differently from the analytical frameworks in this section.

| Framework                  | Main value                                                                         |
| -------------------------- | ---------------------------------------------------------------------------------- |
| Lockheed Martin Kill Chain | Describes intrusion progression and disruption opportunities.                      |
| Unified Kill Chain         | Describes broader adversary progression and operational phases.                    |
| MITRE ATT&CK               | Provides behavioural vocabulary for adversary techniques.                          |
| Diamond Model              | Structures relationships between adversary, infrastructure, capability and victim. |
| OODA Loop                  | Structures decision-making under uncertainty.                                      |
| Pyramid of Pain            | Helps prioritise indicators and detections by adversary disruption.                |
| MaGMa                      | Manages use cases, lifecycle, metrics and continuous improvement.                  |

The other frameworks help the analyst understand the adversary, the evidence or the decision process. MaGMa helps the organisation manage the resulting security monitoring work. That makes it more operational and programme-oriented than most of the other models in this section.

## What Usually Goes Wrong

Several mistakes are common when teams try to manage detection and hunting outputs.

| Problem                               | Why it hurts                                                                                                  |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Treating detections as isolated rules | The organisation loses the connection between threat, risk, telemetry, response and ownership.                |
| No use case lifecycle                 | Detections decay silently as threats, systems and business processes change.                                  |
| No ownership                          | Nobody knows who should maintain or improve the use case.                                                     |
| Metrics without interpretation        | Alert counts and false-positive rates are reported without understanding what they mean.                      |
| Optimising the wrong metrics          | The team improves dashboard numbers while detection quality, coverage or operational usefulness remains weak. |
| Hunt outputs are not operationalised  | Useful findings remain in reports and never become durable security capability.                               |
| Business context is missing           | The SOC cannot explain why a use case matters to the organisation.                                            |
| Documentation becomes paperwork       | The framework exists, but it does not improve detection, response or decision-making.                         |

MaGMa should not become a bureaucratic wrapper around weak detections. It should help the organisation stay in control of use cases and improve them over time.

## Working Position for This Book

For this book, MaGMa is best treated as a management and improvement framework for security monitoring use cases.

It does not replace the hunter’s reasoning. It preserves and manages the security value that hunting, detection engineering and monitoring produce.

The practical workflow is:

```text
Hunt or detection output.
Connect it to a use case.
Document purpose, scope, telemetry, response and ownership.
Measure whether it works.
Improve it over time.
```

That is where MaGMa becomes useful.

A mature hunting programme should not only find interesting things. It should improve the organisation’s ability to detect, respond and learn. MaGMa gives that improvement somewhere to live.

> MaGMa is useful when it turns security monitoring and hunting output into managed capability.
>
> -- Roger Johnsen

## Resources

* [MaGMa: a framework and tool for use case management](https://www.betaalvereniging.nl/wp-content/uploads/FI-ISAC-use-case-framework-verkorte-versie.pdf)
* [MaGMa](https://www.nvb.nl/themas/veilig-bankieren/fi-isac/magma/)
* [ATT&CK Use Cases with MaGMa](https://medium.com/adarma-tech-blog/att-ck-use-cases-with-magma-3a5c83775d86)
* [SOC-CMM MaGMa UCF - Tools for effective Cyber Defense](https://www.first.org/resources/papers/amsterdam2019/2019.04.03-SOC-CMM-MaGMa-FIRST.pdf)

## Revision

| Revised Date | Comment                                                                                                                                                               |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical explanation of MaGMa as a use case management framework for SOC, detection engineering and threat hunting outputs. |
| 2025-04-20   | Article rewritten                                                                                                                                                     |
| 2025-04-13   | Added page                                                                                                                                                            |
