---
title: "Baselines"
description: "A practical explanation of baselines as threat hunting deliverables, how they help define expected behaviour, and how they support anomaly detection, detection engineering and SOC operations."
date: 2026-07-13T00:00:00+02:00
lastmod: 2026-07-14
draft: false
hidden: false
weight: 8
tags:
    - baselines
    - threat hunting
    - anomaly-driven hunting
    - detection engineering
    - SOC
    - process documentation
keywords:
    - baselines
    - threat hunting baselines
    - security baselines
    - behavioural baselines
    - anomaly-driven hunting
    - detection engineering
    - SOC operations
    - expected behaviour
    - normal behaviour
    - threat hunting deliverables
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**A baseline is a documented understanding of expected behaviour within a defined scope. In threat hunting, baselines are important because many investigations start with a simple question: Is this normal here? That question is harder than it looks. Normal behaviour depends on the environment, the system, the user population, the business process, the time period and the available telemetry. What is normal for one organisation may be suspicious in another. What is normal for an administrator may be strange for a standard user. What is normal during month-end closing may be unusual on a Sunday night.**

Most teams do not really have baselines. They have assumptions. They assume which accounts are used by humans, which servers should talk to the internet, which tools administrators normally run, which cloud activities are expected and which alerts represent normal business activity. Some of those assumptions may be correct. Some may be outdated. Some may only be true for part of the environment.

A baseline helps the team describe what is expected before deciding what is unusual. It does not prove that activity is safe, that the environment is clean or that investigation is no longer needed. It gives the team a reference point.

Without a baseline, anomaly-driven hunting becomes guesswork. Analysts may call something suspicious simply because it looks unfamiliar, or they may ignore something important because they have seen similar activity before without understanding whether it was expected.

At its simplest:

```text
A baseline helps the team understand what normal looks like within a defined scope.
```

That understanding can support hunting, detection engineering, alert triage, incident response and SOC maturity.

## What a Baseline Is

A baseline is an observed and documented reference for expected behaviour. It may describe users, systems, services, accounts, processes, network traffic, authentication patterns, administrative activity, cloud activity, scheduled tasks, endpoint behaviour or any other behaviour that matters to security operations.

A useful baseline is not just a chart or a query result. It should explain the scope, the data source, the observation period, the expected pattern, known exceptions, limitations and how the baseline should be used.

A baseline may include:

* the behaviour being baselined
* the reason the baseline was created
* scope and population
* data source and telemetry requirements
* observation period
* expected behaviour
* known-good examples
* known exceptions
* unusual but accepted patterns
* thresholds or ranges where appropriate
* limitations
* confidence level
* recommended use
* owner and review cadence

The important word is **defined**. A baseline without scope is weak. Saying that “service account activity is normal” is not very useful. Saying that “interactive logons by service accounts were not observed from standard user workstations during the last 30 days” is much more useful.

For example:

```text
Weak baseline:
Service accounts are used in the environment.
```

Better baseline:

```text
During the last 30 days, the scoped service accounts authenticated from three
known application servers using expected non-interactive logon patterns.
Interactive logons from user workstations, VPN clients and unmanaged devices
were not observed in the scoped data.
```

The second statement can support hunting, detection work and SOC triage. The first one cannot.

## What a Baseline Is Not

A baseline is not proof that behaviour is benign. Attackers can operate inside normal patterns. Compromised accounts can perform actions that look similar to legitimate work. Malware can use trusted tools. Living-off-the-land behaviour may blend into existing administrative activity. A compromised service account may also be used in ways that look technically valid because the account already has access to the system being abused.

A baseline is also not a permanent truth. Environments change. Users change roles. New tools are deployed. Business processes move to new platforms. Logging changes. Attackers change their behaviour.

A baseline is a reference point, not a guarantee. A poorly understood baseline can create false confidence. If the team treats “normal” as “safe”, it may miss suspicious activity hiding inside expected behaviour.

A safer way to think about it is:

```text
Baseline does not mean benign.
Baseline means expected within the defined scope and observation period.
```

## Why Baselines Matter

Threat hunting often depends on context. An event may look suspicious in isolation, but normal when compared with the right peer group. Another event may look harmless in isolation, but suspicious because it deviates from how that account, host or business unit normally behaves.

Baselines help the team make those comparisons. They support anomaly-driven hunting, hypothesis-driven hunting, detection engineering, alert triage, false-positive reduction, incident scoping, control validation, telemetry validation, SOC training and environment understanding.

A baseline can help answer questions such as:

```text
Which users normally perform privileged operations?
Which service accounts normally authenticate interactively?
Which servers normally initiate outbound internet connections?
Which applications normally create scheduled tasks?
Which endpoints normally communicate with cloud storage services?
Which accounts normally access administrative portals?
Which parent-child process relationships are expected?
Which systems normally receive administrative logons?
```

These questions are not only useful for threat hunters. They are also useful for SOC analysts and detection engineers.

A detection engineer may need a baseline to understand whether a rule will produce useful signal or constant noise. A SOC analyst may need a baseline to understand whether an alert represents expected business activity. An incident responder may need a baseline to understand whether suspicious behaviour is isolated or widespread.

A baseline gives the team local context.

Baselines can also support telemetry value discussions. If a log source is expensive, noisy or under review for reduction, a baseline may help show whether it supports meaningful hunting, detection or triage. This does not mean every log should be kept forever. It means logging decisions should understand the security value of the data before it is removed.

## Baselines as Threat Hunting Deliverables

A threat hunt should leave something behind. Sometimes that output is a finding. Sometimes it is a detection candidate. Sometimes it is a visibility gap, a SITREP, a playbook update or a no-result conclusion.

Sometimes the output is a baseline.

This matters because not every hunt finds malicious activity. A hunt may instead produce a much better understanding of the environment. That understanding can still be valuable if it is documented and made reusable.

For example, a hunt for suspicious service account usage may not find confirmed malicious activity. However, the hunt may reveal:

* which systems normally use the account
* which logon types are expected
* which source hosts are known and approved
* which authentication patterns are common
* whether interactive logons are rare
* whether cloud portal sign-ins occur
* whether VPN usage is expected
* whether logon telemetry is complete
* whether certain behaviours should become detection candidates

That is useful output. The hunt did not find an adversary, but it created operational knowledge. A baseline becomes a threat hunting deliverable when it is documented in a way that other people can use.

## Baseline, Detection Rule and Audit

Baselines, detection rules and audits are related, but they are not the same deliverable.

| Deliverable    | Main question          | Example                                                                                             |
| -------------- | ---------------------- | --------------------------------------------------------------------------------------------------- |
| Baseline       | What is expected here? | Normal authentication behaviour for service accounts                                                |
| Detection rule | What should alert?     | Service account interactive logon from a workstation                                                |
| Audit          | Does it still work?    | Review whether the detection still fires and the required authentication telemetry is still present |

A baseline may feed a detection rule. If the baseline shows that service accounts are not expected to log on interactively from workstations, that may support a detection candidate.

A detection rule may later create alerts that reveal the baseline was incomplete. Analysts may discover legitimate maintenance activity that was not captured during the original baseline period.

An audit may then check whether the detection still works, whether the baseline is still valid and whether the required telemetry is still available.

The relationship may look like this:

```text
Baseline:
Documents expected behaviour.

Detection rule:
Uses that understanding to identify behaviour worth alerting on.

Audit:
Checks whether the assumption, telemetry, detection and response still hold.
```

The baseline is often the quietest deliverable. It may not look as exciting as a detection rule, but it may be the reason the detection rule works.

## Types of Baselines

Baselines can be built around different kinds of behaviour. The type of baseline should depend on the question the team needs to answer.

| Baseline type           | Example question                                                                    |
| ----------------------- | ----------------------------------------------------------------------------------- |
| User baseline           | What is normal behaviour for this user or user group?                               |
| Host baseline           | What is normal behaviour for this endpoint or server group?                         |
| Account baseline        | What is normal behaviour for this service account or privileged account?            |
| Process baseline        | Which processes and parent-child relationships are expected?                        |
| Authentication baseline | Which sign-in patterns, locations and methods are expected?                         |
| Network baseline        | Which systems normally communicate with which destinations?                         |
| Cloud baseline          | Which cloud activities are expected for this tenant, workload or role?              |
| Administrative baseline | Which administrative actions are normal, by whom and from where?                    |
| Time-based baseline     | What is expected during business hours, weekends, month-end or maintenance windows? |
| Telemetry baseline      | Which logs and fields are available, complete and useful?                           |

The most useful baselines are usually scoped tightly. A baseline for “all user behaviour” is usually too broad. A baseline for “interactive logons by service accounts to user workstations during the last 30 days” is much more actionable.

## When Not to Baseline

Not every question needs a baseline. A baseline is useful when the team needs a reference point for expected behaviour. It may be the wrong tool when the real question is urgent investigation, confirmed malicious activity, missing telemetry or a process that changes too quickly to make the baseline meaningful.

A baseline may not be useful when:

* the behaviour is already confirmed malicious
* the environment is changing too quickly
* the telemetry is too incomplete to support comparison
* the population cannot be scoped
* the team already knows the required action
* the question is about immediate containment rather than expected behaviour
* the baseline would be used to delay a decision that already has enough evidence

For example, if malware execution is confirmed on an endpoint, the team does not need a baseline before responding. If the team cannot see authentication source, logon type or account context, the first task may be telemetry improvement rather than an authentication baseline.

A baseline should create useful context. It should not become a way to delay decisions that already have enough evidence.

## Scope Comes First

A baseline must have a clear scope. Without scope, the baseline cannot be interpreted properly. The reader needs to understand what was included, what was excluded and which assumptions were made.

A useful scope statement may define:

* population
* systems
* accounts
* business unit
* time period
* data source
* telemetry fields
* exclusions
* known limitations

For example:

```text
Scope:
This baseline covers interactive logon behaviour for selected service accounts
used by Finance business applications during the period 2026-06-01 to
2026-06-30.

Data source:
Authentication telemetry from identity provider, domain controllers and EDR
where available.

Excluded:
Human user accounts, privileged administrator accounts, disabled service
accounts and application accounts not owned by Finance.

Limitation:
The baseline depends on logon type, source host, account name and authentication
source being available and complete for the scoped period.
```

This gives the baseline meaning. Without that scope, a future analyst may use the baseline in the wrong context and draw the wrong conclusion.

## Observation Period

The observation period matters. A baseline built from one day of data may miss weekly, monthly or seasonal patterns. A baseline built from six months of data may hide important changes inside a large average. A baseline built during an incident may accidentally include attacker behaviour as if it were normal.

The right period depends on the behaviour being studied.

For example:

| Behaviour                  | Possible observation period                                          |
| -------------------------- | -------------------------------------------------------------------- |
| Daily user activity        | 14 to 30 days                                                        |
| Administrative activity    | 30 to 90 days                                                        |
| Month-end finance activity | Several month-end periods                                            |
| Patch management behaviour | Several patch cycles                                                 |
| Rare privileged operations | 90 days or more                                                      |
| Newly deployed service     | Shorter period with explicit review date                             |
| Incident-related behaviour | Avoid using incident period as normal baseline unless clearly marked |

Choosing the wrong period can create bad conclusions. If the period is too short, ordinary but infrequent behaviour may appear suspicious later. If the period is too long, important changes may disappear inside the average. If the period includes attacker dwell time, malicious behaviour may be preserved as if it were expected.

There is no universal rule. The important thing is to document the observation period and explain why it was chosen.

A baseline should not pretend to be timeless.

## Peer Groups Matter

Many baselines are only useful when compared against the right peer group. A domain administrator, finance user, developer, service account and kiosk machine should not be judged by the same behavioural expectations.

Peer groups help make the comparison fair. Useful peer groups may include:

* standard users
* privileged users
* developers
* helpdesk users
* service accounts
* finance users
* executives
* servers
* workstations
* domain controllers
* cloud administrators
* business-critical applications
* geographic regions
* business units

For example, interactive logon may be normal for a human user but suspicious for a service account. External cloud storage access may be normal for marketing but unusual for a production server. Privileged operations may be expected for domain administrators but suspicious for standard users.

A baseline should make the peer group explicit.

```text
The question is not only whether the behaviour is rare.
The question is whether it is rare for this peer group.
```

That is often where the value is.

## Baseline Lifecycle

A baseline should not be treated as a one-time artefact. A simple lifecycle is:

```text
Question → Scope → Observation → Interpretation → Use → Review
```

The question defines what the team wants to understand. The scope defines where the baseline applies. Observation captures what was seen. Interpretation explains what the observations mean. Use defines how the baseline should support hunting, triage or detection engineering. Review checks whether the baseline still represents the environment.

Without review, a baseline can become stale. A stale baseline may be worse than no baseline because it gives the team confidence in outdated assumptions.

A baseline may be created from a hunt, used by a detection engineer, referenced by SOC analysts, challenged by new alerts and later reviewed through an audit. That lifecycle is what turns a local observation into managed operational knowledge.

## Building a Baseline

A baseline should be built from a clear question. The question defines the behaviour, scope and data needed. A practical baseline workflow may look like this:

```text
1. Define the behaviour.
2. Define the scope.
3. Identify the required telemetry.
4. Collect observations from a defined period.
5. Group the activity by useful peer groups.
6. Identify common patterns.
7. Identify known exceptions.
8. Document limitations.
9. Define how the baseline should be used.
10. Set a review point.
```

For example:

```text
Question:
What does expected interactive logon behaviour look like for service accounts?

Scope:
Selected Finance-owned service accounts, last 30 days.

Telemetry:
Authentication telemetry with account, source host, destination, logon type,
authentication method and device context where available.

Grouping:
Account, source host, destination system, logon type, business owner,
authentication method and time of authentication.

Output:
Baseline of expected service account authentication patterns, known maintenance
activity, rare interactive logons and detection candidates.
```

This is not complicated, but it does require discipline. The baseline should not be a pile of query results. It should be an interpreted and documented reference.

## Reference Baseline: Service Account Interactive Logon Behaviour

The example below shows how a baseline may be documented as a threat hunting deliverable. It is intentionally scoped. It does not attempt to describe all service account behaviour in the organisation. It describes one behaviour in one population over one period.

### Baseline Definition

| Field              | Value                                                                                        |
| ------------------ | -------------------------------------------------------------------------------------------- |
| Baseline           | Service Account Interactive Logon Behaviour                                                  |
| ID                 | BL-000001                                                                                    |
| Purpose            | Document expected interactive logon behaviour for selected service accounts.                 |
| Scope              | Finance-owned service accounts used by business applications.                                |
| Observation period | 2026-06-01 to 2026-06-30.                                                                    |
| Data source        | Authentication telemetry from identity provider, domain controllers and EDR where available. |
| Audience           | Threat hunters, SOC analysts, identity administrators and detection engineers.               |
| Related hunts      | Suspicious Service Account Usage Hunt; Privileged Authentication Review.                     |
| Related detections | Service Account Interactive Logon; Service Account Sign-In from Unmanaged Device.            |
| Owner              | Threat Hunting Team.                                                                         |
| Review cadence     | Quarterly or after major identity, application or platform changes.                          |

### Short Baseline Summary

```text
Baseline:
Service Account Interactive Logon Behaviour

Key result:
The scoped service accounts were expected to authenticate non-interactively from
known application servers. Interactive logons were rare during the observation
period and limited to two documented maintenance workflows.

Not observed:
- interactive logon from standard user workstations
- VPN sign-in by service accounts
- cloud portal sign-in by service accounts
- authentication from unmanaged devices
- service account activity outside known application or maintenance workflows

Recommended use:
Use this baseline to support anomaly-driven hunts, SOC triage and detection
candidates for service account usage that falls outside documented operational
patterns.
```

The sections below show the scope, observations and limitations behind that summary.

### Purpose

This baseline documents expected interactive logon behaviour for selected Finance-owned service accounts.

The baseline supports anomaly-driven hunting, detection engineering, SOC triage and identity review by describing which authentication patterns are expected, which are rare and which should require further review.

### Scope

This baseline includes:

```text
- Finance-owned service accounts used by business applications
- Authentication events from identity provider and domain controllers
- Source host and destination context where available
- Logon type where available
- Interactive and non-interactive authentication patterns
- Known maintenance activity
```

This baseline excludes:

```text
- Human user accounts
- Privileged administrator accounts
- Disabled service accounts
- Application accounts owned by other business units
- Break-glass accounts
- Third-party SaaS accounts without sufficient telemetry
```

### Data Requirements

Required telemetry:

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

Limitations:

```text
- Logon type may not be available for all authentication sources.
- Some SaaS authentication events may lack source device context.
- NAT, proxy or VPN infrastructure may reduce source attribution quality.
- The baseline does not prove that observed activity is benign.
- The observation period may not capture rare maintenance workflows.
```

### Observed Patterns

During the observation period, most authentication activity for the scoped service accounts was associated with scheduled application jobs, database connections, integration services and known maintenance activity.

Common observed patterns:

```text
- Non-interactive authentication from known application servers
- Repeated authentication to expected business application backends
- Authentication during scheduled processing windows
- Activity tied to documented application ownership
- Limited maintenance activity from approved administrative hosts
```

Rare or notable patterns:

```text
- Interactive logon by a service account
- Authentication from standard user workstations
- Authentication from unmanaged devices
- VPN sign-in by a service account
- Cloud portal sign-in by a service account
- Service account use outside documented application workflows
- Service account activity outside known processing or maintenance windows
```

### Known Exceptions

Known exceptions should be recorded so that future analysts understand why certain activity may appear unusual but accepted.

| Exception                            | Explanation                                                                                         |
| ------------------------------------ | --------------------------------------------------------------------------------------------------- |
| Monthly Finance maintenance workflow | Two service accounts may be used during approved maintenance from a documented administrative host. |
| Vendor-supported application job     | One legacy application uses a service account interactively during a scheduled support process.     |
| Database integration service         | Repeated authentication from a known application server is expected during business processing.     |

Known exceptions should not become permanent blind spots. They should be reviewed periodically, owned by a responsible system owner and documented clearly enough that the SOC can validate them during triage.

### Baseline Summary

A useful baseline summary may look like this:

```text
The scoped Finance service accounts are normally used for non-interactive
application activity from known servers. Interactive logons were rare during
the baseline period and were limited to documented maintenance workflows.

No VPN sign-ins, cloud portal sign-ins, unmanaged device authentications or
interactive logons from standard user workstations were observed for the scoped
accounts during the observation period.

This baseline supports detection and hunting for service account activity that
falls outside documented application behaviour, especially interactive logons,
new source hosts, unmanaged devices, VPN usage and cloud portal access.
```

This summary is useful because it explains what was observed, what was not observed, and how the baseline should be used.

### Recommended Use

This baseline may support:

```text
- Anomaly-driven hunts for unusual service account behaviour
- Detection engineering for interactive service account logons
- SOC triage of identity and authentication alerts
- False-positive analysis for service account detections
- Review of service account ownership and documentation
- Review of identity telemetry completeness
```

It should not be used to automatically classify all observed service account activity as benign or malicious.

### Detection Candidates

The baseline may generate detection candidates such as:

| Candidate                                            | Reason                                                                                    |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Service account interactive logon from workstation   | Not observed during baseline period and inconsistent with expected service account usage. |
| Service account VPN sign-in                          | Not observed in scoped population and may indicate human use or credential misuse.        |
| Service account cloud portal sign-in                 | Unusual for application service accounts and may indicate interactive abuse.              |
| Service account authentication from unmanaged device | Inconsistent with expected application-hosted usage.                                      |
| Service account activity outside maintenance window  | May indicate misuse if not tied to documented processing.                                 |
| Service account authenticating from new source host  | May indicate application change, misconfiguration or credential misuse.                   |
| Service account used with human-like access pattern  | May indicate shared credential use or account abuse.                                      |

These candidates should be validated before production use.

A baseline can suggest detection logic, but it does not replace detection engineering. In practice, the strongest detections may combine baseline context with multiple signals, such as new source host, interactive logon type, unmanaged device context, unusual time and privileged access.

### Follow-Up Questions

A baseline should often create better questions.

Useful follow-up questions may include:

```text
- Are the same patterns true for service accounts in other business units?
- Are all scoped service accounts owned and documented?
- Are interactive logons technically required for any of these accounts?
- Can service accounts be restricted from workstation logon?
- Is authentication telemetry complete enough for reliable detection?
- Are known maintenance workflows documented and approved?
- Should service account VPN sign-in become a detection rule?
- Should service account cloud portal sign-in be blocked or alerted on?
```

These questions help move the baseline into hunting, detection, identity governance and operational improvement.

### Review

This baseline should be reviewed quarterly or when one of the following occurs:

```text
- New application hosting is introduced
- Identity provider configuration changes
- Domain controller logging changes
- Service account ownership changes
- Finance application workflows change
- Detection rules using this baseline produce excessive false positives
- A related incident or hunt reveals new expected behaviour
- The scoped account population changes significantly
```

A stale baseline can mislead analysts. If the environment changes and the baseline does not, the team may either over-alert on normal changes or miss suspicious activity hidden inside new behaviour.

## Baseline Quality

A baseline is useful only if it is scoped, documented and honest about limitations. A weak baseline usually says:

```text
This is normal.
```

A stronger baseline says:

```text
This was observed as expected behaviour in this population, during this period,
using these data sources, with these limitations.
```

A useful baseline should make clear:

* what was observed
* where it was observed
* when it was observed
* which data source was used
* which population was included
* which population was excluded
* which exceptions are known
* which limitations exist
* how the baseline should be used
* when it should be reviewed

A baseline does not need to be long. It needs to be clear enough that another analyst can understand what it means and what it does not mean.

## Common Failure Modes

Baselines often fail when they are treated as static truth. They may look useful when first created, but become misleading if the environment changes, if the scope is unclear or if analysts use them without understanding their limitations.

| Failure mode                                   | Why it hurts                                                           |
| ---------------------------------------------- | ---------------------------------------------------------------------- |
| No clear scope                                 | The baseline cannot be interpreted safely.                             |
| Poor peer grouping                             | Normal behaviour for one group is applied to another group.            |
| Short observation period                       | Weekly, monthly or seasonal patterns may be missed.                    |
| Long observation period without interpretation | Important changes may disappear inside broad averages.                 |
| Incident period treated as normal              | Attacker behaviour may be preserved as expected behaviour.             |
| Stale baseline                                 | The environment changes but the baseline does not.                     |
| Missing limitations                            | Analysts may overtrust the baseline.                                   |
| Baseline treated as benign list                | Suspicious behaviour may be ignored because it appears common.         |
| No ownership                                   | Nobody updates or reviews the baseline.                                |
| No link to detections                          | Detection engineering cannot use the baseline effectively.             |
| No link to SOC triage                          | Analysts do not know how to apply the baseline during alerts.          |
| No documentation                               | The baseline remains a query result instead of a reusable deliverable. |

The most dangerous baseline is the one that is trusted without context. A baseline should support judgement, not replace it.

## Baselines and Detection Engineering

Baselines are highly useful for detection engineering. A detection rule should not be written only from an attacker technique. It should also consider how that behaviour appears locally. Some behaviours are rare and high-signal in one environment but noisy in another.

A baseline can help detection engineers understand:

- Whether the behaviour exists locally
- Which users, hosts or services normally perform it
- Which fields are available for detection logic
- Which exclusions may be justified
- Which false positives are expected
- Which severity level may be appropriate
- Which triage guidance the SOC will need

For example, a detection for service account interactive logon may be strong in an environment where service accounts are expected to authenticate only from application servers using non-interactive patterns. The same detection may need more context in an environment where legacy maintenance workflows still require documented interactive use.

The baseline does not decide the detection by itself. It provides evidence for detection design.

A useful detection candidate should usually include:

- Behaviour to detect
- Baseline observation
- Data source
- Expected false positives
- Known exclusions
- Severity rationale
- Triage guidance
- Validation method

This is where hunting becomes operational capability.

## Baselines and SOC Triage

SOC analysts need context to make good decisions. A baseline can help an analyst understand whether an alert is unusual for the affected user, host, account or peer group. It can also help the analyst avoid closing alerts too quickly based on generic assumptions.

For example:

```text
Alert:
Service account performed interactive logon to a standard user workstation.

Baseline context:
The account is normally used by a scheduled Finance application job from two
known application servers. Interactive logons from user workstations were not
observed during the 30-day baseline period.

Triage impact:
Treat as suspicious until proven otherwise. Review source device, logon type,
authentication method, recent password changes, related privileged activity,
maintenance windows, application owner notes and whether the account was used
outside its documented workflow.
```

That is more useful than simply saying:

```text
Service accounts are common.
```

The baseline gives the analyst a local reference point.

## Baselines and No-Result Hunts

No-result hunts and baselines are closely related. A no-result hunt may produce a baseline when it documents that a behaviour was not observed in a defined scope. That does not prove the behaviour never occurs. It means the behaviour was not seen in the scoped data during the observation period.

For example:

```text
No interactive service account logons from standard user workstations were
observed for the scoped Finance-owned service accounts during the 30-day
baseline period.
```

That statement can support future hunting and detection work, as long as the scope and limitations are preserved.

The value is not only that nothing was found. The value is that the team now knows what was tested and what was observed.

## Baselines and Threat Hunting Maturity

Baselines are a sign that a hunting function is learning from the environment. A low-maturity hunting function may repeatedly ask the same questions:

```text
Is this normal?
Who usually does this?
Do we expect this behaviour here?
Has this happened before?
```

A more mature function starts preserving the answers. That does not mean every behaviour needs a formal baseline. It means that important operational knowledge should not live only in one analyst’s memory or a temporary notebook.

When a hunt produces reusable understanding, that understanding should be captured. A baseline is one way to do that.

## Working Position for This Book

For this book, a baseline is treated as a documented reference for expected behaviour within a defined scope.

It is not proof that behaviour is benign. It is not a permanent truth. It is not a replacement for investigation.

A useful baseline tells the reader what was observed, where it was observed, when it was observed, which data supported the conclusion and how the baseline should be used.

A baseline earns its place when it helps hunters, SOC analysts and detection engineers make better decisions.

That is the standard. Not because normal behaviour is always safe, but because abnormal behaviour only becomes meaningful when the team understands what expected behaviour looks like.

## Resources

* [MITRE ATT&CK: Valid Accounts](https://attack.mitre.org/techniques/T1078/)
* [MITRE ATT&CK: Account Discovery](https://attack.mitre.org/techniques/T1087/)
* [MITRE ATT&CK: Remote Services](https://attack.mitre.org/techniques/T1021/)
* [MITRE ATT&CK: Anomaly Detection](https://attack.mitre.org/datasources/)
* [The ThreatHunting Project: Hunting Maturity Model](https://www.threathunting.net/hunting-maturity-model)
* [Splunk SURGe: PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html)

## Revision

| Revised Date | Comment                                                                                                                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-13   | Initial version. Introduced baselines as threat hunting deliverables and explained how they support anomaly-driven hunting, detection engineering and SOC triage.                                    |