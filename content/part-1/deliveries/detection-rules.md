---
title: "Detection Rules"
description: "A practical explanation of detection rules as threat hunting deliverables, how they should be designed, documented, validated and handed over to SOC operations."
date: 2026-07-13T00:00:00+02:00
lastmod: 2026-07-14
draft: false
hidden: false
weight: 9
tags:
    - detection rules
    - threat hunting
    - detection engineering
    - SOC
    - alert triage
    - baselines
keywords:
    - detection rules
    - threat hunting deliverables
    - detection engineering
    - SOC detections
    - detection logic
    - detection validation
    - alert triage
    - false positives
    - detection lifecycle
    - security monitoring
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**A detection rule is an operationalised way to identify behaviour that should be reviewed. In threat hunting, a hunt may produce a finding, a baseline, a visibility gap or a better question. Sometimes it also produces something that should continue working after the hunt is finished. That output may become a detection rule.**

A detection rule is not just a query. It is not just a condition in a SIEM, EDR or XDR platform. It is not just a mapping to MITRE ATT&CK.

A useful detection rule explains what behaviour should be detected, why the behaviour matters, which data source supports it, what assumptions are being made, what false positives are expected and how the SOC should respond when the rule fires.

Many weak detections are not weak because the query is wrong. They are weak because nobody can explain why the alert exists, what the analyst should do with it, who owns it or when it should be retired.

At its simplest:

```text
A detection rule turns behaviour worth reviewing into repeatable monitoring.
```

That does not mean every hunt should become a detection rule. Some hunts produce useful knowledge without producing a durable signal. Some behaviours are too noisy, too rare, too environment-specific or too poorly supported by telemetry to become reliable detections.

A weak detection rule creates noise. A strong detection rule creates useful work. Adetection is an operational commitment.

## What a Detection Rule Is

A detection rule is a defined piece of detection logic supported by operational context. The logic identifies behaviour, artefacts or conditions that may indicate malicious activity, policy violation, suspicious behaviour, control failure or activity that requires review.

A detection rule may be implemented in a SIEM, EDR, XDR, NDR, cloud security platform, identity platform, email security tool or custom monitoring pipeline. The platform matters, but it is not the whole point.

A detection rule should include:

* detection purpose
* behaviour being detected
* data source
* detection logic
* assumptions
* ATT&CK mapping where useful
* severity rationale
* expected false positives
* known limitations
* triage guidance
* response guidance
* validation method
* owner
* review cadence
* change history
* retirement criteria

The important point is that detection is not only technical logic. Detection is an operational commitment.

If a rule creates alerts, someone must understand them, triage them, document them and decide what happens next.

## What a Detection Rule Is Not

* A detection rule is not automatically good because it matches an attacker technique.
* A detection rule is not automatically useful because it has a MITRE ATT&CK mapping.
* A detection rule is not automatically mature because it fires.
* A detection rule should not be treated as finished simply because the query runs without errors.

For example, this is weak:

```text
Detect service account activity.
```

That is too broad. Service account activity may be normal, administrative, suspicious or malicious depending on account purpose, source host, logon type, business process and context.

This is better:

```text
Detect interactive logon by a service account from a standard user workstation
where the related baseline shows that the account is expected to authenticate
non-interactively from known application servers.
```

The second version describes behaviour, scope and local context. It is much closer to something the SOC can use.

A detection rule should not only answer:

```text
Can we detect this?
```

It should also answer:

```text
Should we alert on this, and can the SOC do something useful with the alert?
```

That is where many weak detections fail.

## The Cost of Detection

Every production detection creates cost. Sometimes that cost is worth it. Sometimes it is not.

The cost may include alert volume, analyst time, false-positive handling, case documentation, tuning effort, engineering maintenance, escalation load and loss of trust when analysts repeatedly receive alerts they cannot act on.

A detection rule that fires often but produces no useful decision is not free. It consumes attention. It competes with other alerts. It may train analysts to close similar alerts quickly. Over time, it can damage confidence in the detection programme.

The cost of detection should therefore be considered before production deployment.

Useful questions include:

- How often is this expected to alert?
- Who will triage it?
- What evidence will the analyst receive?
- What decision should the analyst make?
- What response path exists?
- How much tuning will be required?
- Who owns maintenance?
- When should the rule be reviewed or retired?

A good detection is not the one that alerts the most. A good detection is the one that creates useful, reviewable work at a cost the organisation is willing and able to carry.

## Detection Rule, Baseline and Hunt

Detection rules often come from hunting, but they should not be created blindly from every hunt idea. A hunt may test a hypothesis. A baseline may describe expected behaviour. A detection rule may turn a useful signal into repeatable monitoring.

| Deliverable    | Main question                            | Example                                                                                         |
| -------------- | ---------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Hunt           | Is this behaviour present or meaningful? | Are service accounts logging on interactively from workstations?                                |
| Baseline       | What is expected here?                   | Scoped service accounts normally authenticate non-interactively from known application servers. |
| Detection rule | What should alert?                       | Service account interactive logon from a workstation or unmanaged device.                       |

The relationship is not always linear. A detection rule may lead to alerts that reveal the original baseline was incomplete. A noisy detection may create a new hunt. A no-result hunt may show that a proposed detection has little local value.

Still, the general movement is useful:

```text
Hunt observation → local context → detection candidate → validation → production detection
```

A detection rule should preserve the reasoning that created it. Otherwise, future analysts may see the logic but not understand the decision behind it.

## From Hunt Finding to Detection Candidate

Not every hunt finding should become a detection rule. A detection candidate is a behaviour, condition or pattern that may be suitable for monitoring, but still needs validation before it becomes production detection.

A good candidate usually has:

* clear behaviour
* relevant data source
* local evidence
* security value
* expected alert volume
* known false positives
* triage path
* response path
* validation plan
* owner

For example:

```text
Hunt observation:
The scoped Finance service accounts authenticated non-interactively from known
application servers during the 30-day baseline period. Interactive logons from
standard user workstations, VPN clients and unmanaged devices were not observed.

Detection candidate:
Alert when a scoped service account performs an interactive logon from a
standard user workstation, VPN client, unmanaged device or previously unseen
source host.

Reason:
The behaviour falls outside the documented baseline and may indicate credential
misuse, human use of a service account or application misconfiguration.
```

This is not yet a finished detection rule. It is a candidate. It still needs testing, tuning and operational review.

A candidate should be rejected or parked if the team cannot explain why it matters, cannot observe it reliably or cannot define what the SOC should do with the result.

## When Not to Create a Detection Rule

Not every useful observation should become a detection rule. A detection rule is useful when the behaviour is observable, meaningful, actionable and suitable for repeatable monitoring. It may be the wrong output when the behaviour is too noisy, too poorly understood, too dependent on missing telemetry or too difficult for the SOC to triage.

A detection rule may not be useful when:

* the behaviour cannot be observed reliably
* the expected alert volume would overwhelm the SOC
* the team cannot explain why the behaviour matters
* the SOC has no reasonable triage or response path
* the behaviour is better suited for a hunt, baseline or audit
* the rule would only detect one lab scenario
* the logic depends on fields that are often missing
* the team already knows the control is broken and needs remediation
* no team or role can own the detection
* no action would be taken even if the alert fired

A simple rule of thumb is:

```text
No action, no detection.
```

That does not mean every alert must lead to containment. It means the alert should support a decision. The decision may be to investigate, escalate, document, tune, validate, close as expected activity or update a baseline. If the alert supports no useful decision, it probably should not be a production detection.

For example, if service account ownership is unknown, the first task may be an identity review rather than a detection rule. If interactive logon data is missing, the first task may be telemetry improvement. If a detection would fire thousands of times a day with no useful triage path, it is not ready for production.

A detection rule should create useful work. It should not create alert volume just because a behaviour looks interesting.

## Detection Rules as Threat Hunting Deliverables

A detection rule becomes a threat hunting deliverable when it preserves useful hunting knowledge as repeatable monitoring. The hunter should not only hand over a query. The hunter should hand over enough context for detection engineering and SOC operations to understand the behaviour.

A useful handover may include:

```text
Detection name:
Service Account Interactive Logon from Workstation

Behaviour:
A service account performs an interactive logon from a standard user workstation
or unmanaged device.

Why it matters:
Service accounts are usually expected to support application or system activity,
not human interactive logons from user devices. Interactive use may indicate
credential misuse, shared account use, application misconfiguration or
unauthorised administrative activity.

Local context:
The scoped service accounts were observed authenticating non-interactively from
known application servers during the baseline period. Interactive logons from
standard user workstations were not observed.

Data source:
Authentication telemetry with account, source host, destination system, logon
type and device context where available.

Expected false positives:
Approved maintenance workflows, legacy application support, vendor support
activity or documented break-fix procedures.

SOC action:
Review account owner, source device, logon type, authentication method, recent
password changes, related privileged activity, maintenance windows and whether
the activity matches a documented workflow.
```

That is a deliverable. A query without this context is only a fragment.

## Anatomy of a Detection Rule

A detection rule should be documented as an operational package. The exact format depends on the organisation and tooling, but the following structure is useful.

```text
Title:
[Detection name]

Purpose:
[Why this detection exists]

Behaviour:
[What behaviour the detection identifies]

Threat rationale:
[Why the behaviour may matter]

Data source:
[Telemetry required for the detection]

Logic:
[Detection query or rule logic]

Scope:
[Where the detection applies]

Exclusions:
[Known-good activity excluded from alerting]

Severity:
[Initial severity and rationale]

ATT&CK mapping:
[Relevant tactic, technique or sub-technique]

Expected false positives:
[Likely benign causes]

Triage guidance:
[What the analyst should check first]

Response guidance:
[What actions may be required]

Validation:
[How the detection was tested]

Limitations:
[What the detection cannot see or prove]

Owner:
[Team or role responsible for maintenance]

Review:
[Review cadence and triggers]

Retirement:
[When the rule should be removed or replaced]
```

This structure prevents the most common problem: detection logic that nobody understands six months later. The rule should explain itself.

## Detection Rule Lifecycle

A detection rule should have a lifecycle. A simple lifecycle is:

```text
Idea → Candidate → Validation → Production → Operation → Tuning → Audit → Retirement
```

The idea may come from a hunt, incident, intelligence report, audit, baseline or control gap. The candidate defines the behaviour and expected value. Validation tests whether the rule works technically and operationally. Production creates SOC work. Operation shows how the rule behaves in the real environment. Tuning adjusts scope, severity and exclusions. Audit checks whether the rule, telemetry and triage path still work. Retirement removes rules that no longer produce useful work.

A detection rule without a lifecycle becomes technical debt.

It may continue firing after the original assumption has expired. It may alert on behaviour the organisation no longer cares about. It may depend on fields that are no longer reliable. It may remain in production because nobody knows who owns it or why it exists.

A mature detection programme should be able to create detections, operate them, improve them and remove them.

## Reference Detection Rule: Service Account Interactive Logon from Workstation

The reference below shows how a detection rule may be documented as a threat hunting deliverable.

It is intentionally written as an operational detection package, not only as a query.

### Detection Definition

| Field            | Value                                                                                                         |
| ---------------- | ------------------------------------------------------------------------------------------------------------- |
| Detection        | Service Account Interactive Logon from Workstation                                                            |
| ID               | DET-000001                                                                                                    |
| Purpose          | Detect service accounts performing interactive logons from standard user workstations or unmanaged devices.   |
| Behaviour        | Service account authenticates interactively from a source that is not expected for application or system use. |
| Scope            | Scoped Finance-owned service accounts and standard user workstations.                                         |
| Data source      | Authentication telemetry from identity provider, domain controllers and EDR where available.                  |
| Audience         | Detection engineers, SOC analysts, identity administrators and threat hunters.                                |
| Related baseline | Service Account Interactive Logon Behaviour.                                                                  |
| Related playbook | Suspicious Account Usage Playbook; Identity Compromise Playbook.                                              |
| Related runbooks | Review Authentication Context; Review Account Ownership; Review Source Device; Collect Identity Artefacts.    |
| Owner            | Detection Engineering.                                                                                        |
| Review cadence   | Quarterly or after major identity, application or logging changes.                                            |

### Purpose

This detection identifies cases where a scoped service account performs an interactive logon from a standard user workstation, unmanaged device or other source that falls outside documented service account behaviour.

The behaviour may indicate credential misuse, human use of a shared service account, application misconfiguration, unauthorised administrative activity or early signs of identity compromise. It may also occur during approved maintenance or legacy support workflows, which means the detection should be validated against local baselines before production deployment.

### Behaviour

The detection focuses on authentication behaviour.

```text
Account examples:
- application service accounts
- integration service accounts
- scheduled job accounts
- legacy application accounts
- business application service accounts

Suspicious source examples:
- standard user workstation
- unmanaged device
- VPN client
- previously unseen source host
- cloud portal session
- source outside documented maintenance workflow
```

The detection should focus on service accounts performing interactive logons or human-like authentication patterns from unexpected sources. Depending on local tooling and logging, the rule may also consider logon type, authentication method, device compliance, source IP, destination system, time of day and account ownership.

### Threat Rationale

Service accounts are often trusted and over-permissioned. They may have access to applications, databases, file shares, scheduled tasks, integrations or administrative interfaces. If their credentials are misused, the activity may appear legitimate because the account is already expected to access sensitive systems.

Interactive logon by a service account is therefore useful to monitor when the related baseline shows that the account is expected to authenticate non-interactively from known application servers.

The behaviour is not automatically malicious. Some legacy workflows or maintenance processes may require documented interactive use. However, a service account logging on from a standard user workstation should usually require review.

A useful rationale statement may look like this:

```text
The scoped service accounts were expected to authenticate non-interactively from
known application servers during the baseline period. Interactive logons from
standard user workstations, VPN clients and unmanaged devices were not observed.
Alerts should be reviewed as suspicious unless a documented maintenance workflow
or approved exception explains the activity.
```

### Data Requirements

Required fields:

```text
- Timestamp
- Account name
- Account type
- Source host or source IP
- Destination system or application
- Logon type
- Authentication method
- Identity provider or domain controller source
- Device ownership or management state where available
- Business owner or application owner
```

Useful enrichment:

```text
- Account owner
- Application owner
- Account purpose
- Privilege level
- Group membership
- Recent password changes
- Recent account changes
- Device compliance state
- Source host peer group
- Geolocation where useful
- Related identity alerts
- Related endpoint alerts
- Approved maintenance windows
```

If logon type, source host or account ownership is missing, the detection may lose much of its value. That limitation should be documented before production use.

### Example Logic

The example below is intentionally generic. It shows the shape of the logic, not a universal production rule.

```text
AuthenticationEvents
where AccountType == "ServiceAccount"
where LogonType in ("Interactive", "RemoteInteractive")
where SourceDeviceType == "Workstation"
where AccountName in ScopedServiceAccounts
```

A production rule should be adapted to the local data model, platform and naming conventions.The detection may become stronger when combined with additional context:

```text
Higher-risk traits:
- unmanaged source device
- previously unseen source host
- VPN source
- cloud portal sign-in
- privileged service account
- authentication outside maintenance window
- recent password reset
- recent group membership change
- related endpoint alert on source device
- related identity alert for the same account
```

The rule should not become so complex that analysts cannot understand it. If the logic grows too large, consider separating high-fidelity variants from broader hunting queries.

### Scope

Initial scope:

```text
Included:
- Scoped service accounts
- Standard user workstations
- Interactive and remote interactive logons
- Authentication events with source context

Excluded:
- Human user accounts
- Disabled service accounts
- Break-glass accounts
- Known application servers
- Approved administrative jump hosts
- Documented maintenance workflows
```

Scope should be reviewed after validation. If the detection is useful for some account groups but noisy in others, deployment may need to be staged or scoped by owner, application, account type or peer group.

### Exclusions

Exclusions should be based on documented known-good activity, not analyst frustration.

Possible exclusions:

```text
- approved maintenance workflow from a documented administrative host
- legacy application support process
- vendor support activity during approved windows
- known break-fix procedure with named owner
```

Every exclusion should have:

```text
- reason
- owner
- approval
- review date
- supporting evidence
```

A detection with too many unreviewed exclusions may become blind.

### Severity

Initial severity may be **Medium** for scoped service accounts. Severity may increase when additional risk factors are present:

```text
Increase severity when:
- source device is unmanaged
- account is privileged or has access to sensitive systems
- source host is previously unseen for the account
- authentication occurs outside maintenance window
- VPN or cloud portal sign-in is involved
- account recently had password or group membership changes
- related endpoint or identity alerts exist
- the destination system is business-critical
```

Severity may decrease when the activity matches a documented and approved maintenance process.

Severity should be based on behaviour, context and confidence. It should not be assigned only because a technique has an ATT&CK mapping.

### ATT&CK Mapping

Possible ATT&CK mapping:

| Tactic               | Technique                 |
| -------------------- | ------------------------- |
| Initial Access       | T1078 - Valid Accounts    |
| Persistence          | T1078 - Valid Accounts    |
| Privilege Escalation | T1078 - Valid Accounts    |
| Defence Evasion      | T1078 - Valid Accounts    |
| Discovery            | T1087 - Account Discovery |
| Lateral Movement     | T1021 - Remote Services   |

ATT&CK mapping is useful for context, reporting and coverage analysis. It does not prove detection quality. 

> A weak rule with an ATT&CK mapping is still a weak rule.
> 
> -- Roger Johnsen 

### Expected False Positives

Expected false positives may include:

```text
- approved maintenance workflows
- legacy application support processes
- vendor support activity
- misclassified service accounts
- application troubleshooting
- break-fix procedures
- incomplete account ownership data
```

False positives should be investigated before broad exclusions are added. A repeated false positive may reveal a legitimate business process that should be documented in the baseline.

### Triage Guidance

When the detection fires, the analyst should review:

```text
- account name
- account owner
- account purpose
- source host
- destination system
- logon type
- authentication method
- device management state
- whether the source is expected for the account
- recent password changes
- recent group membership changes
- related identity alerts
- related endpoint alerts
- approved maintenance windows
- known application or support workflows
```

Useful first question:

```text
Why did this service account log on interactively from this source?
```

The analyst should avoid closing the alert only because the account is known. The relevant question is whether this type of authentication is expected for this account, source and business context.

### Response Guidance

Possible response paths:

| Condition                                  | Response                                                                       |
| ------------------------------------------ | ------------------------------------------------------------------------------ |
| Known approved maintenance workflow        | Document evidence and close or tune according to process.                      |
| Misclassified account                      | Update account inventory and review whether the detection scope should change. |
| Unknown source workstation                 | Escalate for identity and endpoint review.                                     |
| Unmanaged source device                    | Treat as suspicious and review identity, endpoint and network context.         |
| Privileged service account involved        | Escalate according to incident escalation SOP.                                 |
| Recent password or group membership change | Review change history and account owner approval.                              |
| Related endpoint or identity alert exists  | Escalate for deeper investigation.                                             |
| Evidence inconclusive                      | Preserve evidence, document uncertainty and continue investigation.            |

The detection should link to the relevant playbooks and runbooks. The rule should not force analysts to invent a response path during triage.

### Validation

Validation should test whether the detection works technically and operationally.

Technical validation:

```text
- Rule runs successfully in the target platform.
- Required fields are available.
- Scoped service accounts are correctly identified.
- Interactive logon type is captured correctly.
- Source host or source IP is available.
- Device context is available where expected.
- Known test activity triggers the rule.
```

Operational validation:

```text
- Alert volume is acceptable.
- False positives are understood.
- Account owners can be identified.
- SOC analysts can triage the alert.
- Triage guidance is clear.
- Escalation path is defined.
- Detection output contains enough context.
```

A detection should not move to production only because the query works. It should move to production when the organisation can handle the alert.

### Limitations

This detection has limitations.

```text
- It may miss misuse that occurs through expected application paths.
- It may miss activity if logon type is unavailable.
- It may miss activity if source host context is incomplete.
- It may generate false positives from undocumented maintenance workflows.
- It may depend on account inventory quality.
- It may not detect non-interactive abuse of the same account.
- It does not prove compromise by itself.
```

Limitations should be documented so that analysts understand what the rule can and cannot tell them.

### Expected Output

A useful alert should contain enough context for first-line triage.

Expected alert fields:

```text
- Detection name
- Timestamp
- Account name
- Account type
- Source host or IP
- Destination system
- Logon type
- Authentication method
- Device management state where available
- Account owner
- Application owner
- Related identity alerts
- Related endpoint alerts
- Severity
- Recommended triage steps
```

If the alert only says “service account logon detected”, the detection is not operationally useful enough.

### Review and Maintenance

This detection should be reviewed when:

```text
- service account ownership changes
- identity provider configuration changes
- domain controller logging changes
- EDR telemetry changes
- application hosting changes
- false positives increase
- new legitimate maintenance workflows are discovered
- related incident or hunt findings suggest changes
- SOC analysts report unclear triage guidance
```

Review should consider whether the detection still has value, whether exclusions remain justified and whether the original baseline still holds.

### Retirement Criteria

Not every detection should live forever. This detection should be considered for retirement or replacement when:

```text
- scoped service accounts are redesigned or removed
- interactive logon is technically blocked for the account population
- a stronger preventive control replaces the detection need
- telemetry no longer supports reliable detection
- the rule produces no useful alerts over multiple review cycles
- a broader identity detection covers the same behaviour with better context
- the SOC no longer has a valid response path
```

Retirement is not failure. Removing a detection that no longer creates useful work is part of maintaining detection quality.

## Detection Quality

A detection rule should be judged by operational usefulness, not only by technical correctness. A technically correct detection may still be poor if it creates noise, lacks context, cannot be triaged or has no clear response path.

A useful detection should answer:

```text
- What behaviour is being detected?
- Why does it matter?
- Where does the data come from?
- What assumptions are being made?
- What false positives are expected?
- What should the SOC do?
- How was the rule validated?
- Who owns the rule?
- When should it be reviewed?
- When should it be retired?
```

If these questions cannot be answered, the detection is not ready for production.

## Common Failure Modes

Detection rules often fail after they are deployed. They may look strong during development, but fail when they meet real alert volume, incomplete telemetry, unclear triage or changing business behaviour.

| Failure mode                 | Why it hurts                                                                  |
| ---------------------------- | ----------------------------------------------------------------------------- |
| Query-only detection         | The SOC receives logic without context or guidance.                           |
| No baseline                  | The team does not know whether the behaviour is rare or normal locally.       |
| No owner                     | Nobody reviews, tunes or retires the rule.                                    |
| No validation                | The team does not know whether the rule works as intended.                    |
| No triage guidance           | Analysts handle alerts inconsistently.                                        |
| No false-positive analysis   | Noise is accepted or suppressed without learning.                             |
| Too broad                    | The rule creates alert fatigue.                                               |
| Too narrow                   | The rule only detects a lab example.                                          |
| Too many exclusions          | The rule becomes blind to meaningful behaviour.                               |
| Stale assumptions            | The environment changes but the rule does not.                                |
| Weak severity rationale      | Alerts are prioritised poorly.                                                |
| No response path             | The alert fires, but nobody knows what should happen next.                    |
| No account ownership         | Analysts cannot determine whether activity is expected or abusive.            |
| Lab-only validation          | The rule works in a test case but fails against real operational data.        |
| Alert without entity context | The SOC sees an alert but lacks account, device, owner or peer-group context. |
| No retirement path           | Old detections continue creating work after they stop producing value.        |

The most common failure is treating detection as a technical artefact only. Detection is operational work.

## Detection Rules and SOC Triage

A detection rule creates work for the SOC. That work should be intentional. Before a rule is placed into production, the team should understand what an analyst is expected to do when the alert fires.

A useful triage package should include:

```text
- what happened
- why it matters
- what to check first
- what evidence to preserve
- what related activity to review
- when to escalate
- when closure is reasonable
- which playbook or runbook applies
```

If the SOC cannot triage the alert, the detection is not ready. This does not mean every detection must be perfect. It means every production detection should create work that has a purpose.

## Detection Rules and Threat Hunting Maturity

A mature threat hunting function does not only find interesting behaviour. It helps the organisation decide what should be monitored, what should be ignored, what should be baselined and what should be improved. Detection rules are one way hunting becomes durable. 

A hunt may start with a question:

```text
Are service accounts being used interactively from workstations or unmanaged devices?
```

The hunt may produce observations:

```text
The scoped Finance service accounts normally authenticate non-interactively
from known application servers.

Interactive logons from standard user workstations were not observed during
the baseline period.

Two maintenance workflows explain rare interactive use from approved
administrative hosts.
```

The detection engineering output may then become:

```text
Alert on service account interactive logon from standard user workstations,
unmanaged devices or previously unseen source hosts, with documented exclusions
for approved maintenance workflows and clear SOC triage guidance.
```

That is a mature path from hunt to detection. The value is not only the rule. The value is the reasoning preserved inside the rule.

## Working Position for This Book

For this book, a detection rule is treated as an operational detection package.

It is not only logic. It is logic plus context, assumptions, expected false positives, validation, triage guidance, response path, ownership, review and retirement.

A detection rule earns its place when it creates useful, reviewable work for the SOC.

That is the standard. Not because every suspicious behaviour needs an alert, but because every alert should have a reason to exist.

## Resources

* [MITRE ATT&CK: Valid Accounts](https://attack.mitre.org/techniques/T1078/)
* [MITRE ATT&CK: Domain Accounts](https://attack.mitre.org/techniques/T1078/002/)
* [MITRE ATT&CK: Cloud Accounts](https://attack.mitre.org/techniques/T1078/004/)
* [MITRE ATT&CK: Account Discovery](https://attack.mitre.org/techniques/T1087/)
* [MITRE ATT&CK: Remote Services](https://attack.mitre.org/techniques/T1021/)
* [Sigma: Generic Signature Format for SIEM Systems](https://sigmahq.io/)
* [Splunk SURGe: PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html)
* [The ThreatHunting Project: Hunting Maturity Model](https://www.threathunting.net/hunting-maturity-model)

## Revision

| Revised Date | Comment                                                                                                                                                                                                 |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-13   | Initial version. Introduced detection rules as threat hunting deliverables and explained how detection logic should be documented, validated and handed over to SOC operations.                         |
| 2026-07-14   | Revised for flow, added detection cost, when not to create a detection rule, detection lifecycle, retirement criteria and replaced the reference rule with service account interactive logon behaviour. |
