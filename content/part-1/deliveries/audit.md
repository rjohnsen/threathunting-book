---
title: "Audits"
description: "A practical explanation of audits as threat hunting deliverables, how hunters can review assumptions, telemetry, detections and operational controls, and how audits support SOC maturity."
date: 2026-07-13T00:00:00+02:00
lastmod: 2026-07-14
draft: false
hidden: false
weight: 10
tags:
    - audits
    - threat hunting
    - SOC
    - detection engineering
    - baselines
    - detection rules
    - operational review
keywords:
    - audits
    - threat hunting audits
    - SOC audits
    - detection audit
    - telemetry audit
    - detection engineering
    - baselines
    - detection rules
    - operational controls
    - security operations maturity
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**An audit is a structured review of whether something still works as expected. In threat hunting and security operations, audits should not be understood only as compliance exercises. A threat hunter may perform or contribute to audits that check whether telemetry exists, whether detections still work, whether baselines are still valid, whether playbooks are followed, whether response actions are documented, or whether assumptions made during previous hunts still hold.**

This kind of audit is operational. It asks whether the security team can still observe, detect, investigate, respond and improve in the way it believes it can.

A baseline may answer:

```text
What is expected here?
```

A detection rule may answer:

```text
What should alert?
```

An audit asks:

```text
Does it still work?
```

That question matters because security operations decay. Logs disappear. Permissions change. Tools are reconfigured. Detection rules become noisy. Playbooks go stale. Runbooks stop matching the interface. Teams change. Business processes move. Assumptions that were true six months ago may no longer be true.

An audit helps the team find that drift before it matters.

## What an Audit Is

An audit is a structured review against a defined expectation. That expectation may come from a policy, SOP, playbook, runbook, detection rule, baseline, control requirement, incident lesson, regulatory obligation or threat hunting assumption. A useful audit does not simply ask whether a document exists. It asks whether the thing described by the document still works in practice.

An audit may review:

* telemetry availability
* data quality
* detection coverage
* detection rule performance
* alert triage quality
* response documentation
* evidence handling
* baseline validity
* playbook usage
* runbook accuracy
* escalation behaviour
* control effectiveness
* ownership and review cadence
* exception handling
* lessons learned implementation

The important word is **evidence**. An audit should not rely only on opinion. It should produce observations that can be reviewed, challenged and acted on.

For example, a weak audit statement would be:

```text
PowerShell detection looks fine.
```

A stronger audit statement would be:

```text
The PowerShell detection rule fired successfully during controlled validation.
Required parent process and command-line fields were present in 97% of scoped
endpoint events during the review period. Three known exclusions remain valid.
One business unit has incomplete endpoint telemetry and should be remediated
before the detection can be considered reliable for that scope.
```

The second statement can drive action. The first one cannot.

## What an Audit Is Not

An audit is not the same as a hunt. A hunt investigates whether a behaviour, hypothesis or threat may be present or meaningful in the environment. An audit reviews whether an expected control, process, detection, data source or assumption is working as intended.

An audit is also not automatically a compliance activity. Compliance audits may be one type of audit, but threat hunting and SOC audits are often more operational. They are concerned with whether the team can actually perform the work it claims to perform.

An audit is not a blame exercise. If a detection no longer works, a log source is missing or a playbook is stale, the useful question is not:

```text
Who failed?
```

The useful question is:

```text
What changed, what is the impact, and what should be improved?
```

A mature audit should make operational drift visible.

## Audit, Baseline and Detection Rule

Audits complete the loop between baselines, detection rules and operations.

| Deliverable    | Main question          | Example                                                                                |
| -------------- | ---------------------- | -------------------------------------------------------------------------------------- |
| Baseline       | What is expected here? | Normal PowerShell usage by user workstations                                           |
| Detection rule | What should alert?     | Office spawning PowerShell from user workstations                                      |
| Audit          | Does it still work?    | Validate whether the detection still fires and the required telemetry is still present |

A baseline may document expected behaviour. A detection rule may use that baseline to create monitoring. An audit later checks whether the baseline, detection rule, data source and operational response still make sense.

The relationship may look like this:

```text
Baseline:
Documents expected behaviour.

Detection rule:
Operationalises behaviour worth reviewing.

Audit:
Checks whether the assumption, telemetry, detection and response still hold.
```

This is how hunting output becomes part of a managed security capability rather than a one-time investigation.

## Types of Audits

Threat hunters can contribute to different types of operational audits. The audit type should depend on the question being asked, not on a need to produce more paperwork.

| Audit type            | Main question                                                                           |
| --------------------- | --------------------------------------------------------------------------------------- |
| Telemetry audit       | Can we observe the behaviour we care about?                                             |
| Detection audit       | Does the detection still work as intended?                                              |
| Baseline audit        | Is the baseline still valid?                                                            |
| Playbook audit        | Is the scenario guidance still usable and followed?                                     |
| Runbook audit         | Can the task still be executed as documented?                                           |
| SOP audit             | Is the required process being followed and documented?                                  |
| Coverage audit        | Which behaviours, assets or techniques are covered or not covered?                      |
| Control audit         | Is the security control producing the expected security outcome?                        |
| Assumption audit      | Are previous hunting or detection assumptions still true?                               |
| Exception audit       | Are exceptions still justified and documented?                                          |
| Migration audit       | Did detection capability survive a platform, SIEM or telemetry migration?               |
| Telemetry value audit | Which logs and fields are worth keeping, reducing or removing?                          |
| Validation audit      | Can controlled test activity confirm that expected telemetry and detections still work? |

{{% notice tip %}}
The point is to review the things that matter before they fail during an incident.
{{% /notice %}}

## When Not to Audit

Not every question needs an audit. An audit is useful when there is a defined expectation to review. If the expectation is unclear, a hunt, baseline or design review may be better first.

An audit may be the wrong tool when:

* there is no defined control, detection, process or assumption to test
* the team already knows the issue and the work belongs in a backlog
* ownership is missing and findings cannot become action
* the area is low-risk and stable enough that review effort would be better used elsewhere
* the real question is exploratory rather than validation-focused
* the telemetry does not exist and the first task is to establish visibility

For example, if the team does not know what normal PowerShell usage looks like, a baseline is probably more useful than an audit. If the team suspects that a new attacker behaviour may be present, a hunt is probably more useful than an audit. If the team already knows that a detection is broken, another audit may not add much value. The next step is remediation, ownership and later re-audit.

An audit should create clarity. It should not become a way to postpone known improvement work.

## Audit Scope

An audit must have a clear scope. Without scope, the audit cannot be interpreted safely. The scope defines what was reviewed, what was not reviewed and what the result can reasonably say.

A useful audit scope may include:

* audit objective
* systems or populations reviewed
* time period
* data sources
* controls or documents reviewed
* test method
* exclusions
* limitations
* expected output
* owner

For example:

```text
Audit objective:
Validate whether the Office-spawned PowerShell detection is still operationally
effective for standard user workstations.

Scope:
Standard user workstations, last 30 days.

Data sources:
Endpoint process telemetry, detection rule history, SOC alert records and
related baseline documentation.

Exclusions:
Servers, administrator workstations and known software deployment systems.

Limitations:
The audit depends on endpoint telemetry completeness and case documentation
quality.
```

This gives the audit meaning. Without scope, the audit may sound broader than the evidence supports.

An audit does not prove that everything works everywhere. It shows what was reviewed, what evidence was used and whether the reviewed item worked within the defined scope.

## Audit Lifecycle

An operational audit should not stop at the finding. A simple lifecycle is:

```text
Trigger → Scope → Evidence → Finding → Action → Re-audit
```

The trigger explains why the audit is performed. The scope defines what is reviewed. The evidence supports the conclusion. The finding explains what changed, failed or was confirmed. The action moves the issue into ownership, backlog or remediation. The re-audit checks whether the improvement worked.

Without action and re-audit, the audit may only document drift instead of reducing it.

Common audit triggers include:

* scheduled review cadence
* major tool change
* major logging change
* incident lesson learned
* repeated SOC triage issue
* detection performance concern
* new business process
* baseline expiry
* significant exception
* control change
* failed validation
* management or regulatory request
* SIEM or detection platform migration
* telemetry reduction or licence optimisation initiative

The trigger matters because it explains why this audit is worth doing now.

## SIEM and Detection Platform Migrations

SIEM migrations are a strong audit trigger. When an organisation moves from one SIEM or detection platform to another, it should not assume that detection capability survives the migration. Queries may need to be rewritten, fields may be named differently, parsing may change, enrichment may be missing and alert workflows may behave differently.

A migration audit can help answer:

```text
- Which detections were migrated?
- Which detections were changed during migration?
- Which detections no longer work?
- Which required fields are missing or renamed?
- Which alerts still reach the SOC?
- Which playbooks or runbooks need to be updated?
- Which detections should be retired instead of migrated?
```

A SIEM migration is therefore not only a technical migration. It is also a detection capability review.

The same applies when moving between EDR, XDR, NDR, cloud security or SOAR platforms. The tooling may change, but the operational question remains the same:

```text
Did the capability survive the change?
```

## Telemetry Reduction and Licence Pressure

Audits are also useful when an organisation needs to reduce logging volume.In many environments, telemetry has a direct cost. SIEM licensing, storage cost, ingestion limits and operational noise may force the organisation to reduce what it logs. That can be reasonable, but it should not be done blindly.

A telemetry reduction audit can help the team understand what can be reduced, what must be preserved and which detection capabilities depend on specific data sources or fields.

Useful questions include:

```text
- Which detections depend on this log source?
- Which fields are required for triage?
- Which fields are required for detection logic?
- Which logs are high-volume but low-value?
- Which logs are low-volume but high-value?
- Which detections will break if this source is removed?
- Which hunts will no longer be possible?
- Which compliance or incident response requirements depend on this data?
```

The goal is not to keep every log forever. The goal is to understand the security consequence of removing telemetry.

A logging reduction that saves licence cost but removes the organisation’s ability to detect or investigate important behaviours may only move the cost somewhere else.

## Evidence Comes First

Audits should be evidence-based. The evidence does not need to be perfect, but the audit should make clear what was reviewed and what supports the conclusion.

Possible audit evidence includes:

```text
- Log availability checks
- Sampled events
- Detection rule execution history
- Alert history
- Case records
- Closed alert samples
- False-positive records
- Baseline documents
- Change records
- Playbook or runbook versions
- Screenshots
- Query results
- Validation tests
- Exception records
- Analyst interviews
```

Interviews and analyst feedback can be useful, but they should not be the only evidence unless the limitation is explicitly documented.

For example:

```text
Finding:
Analysts report that the runbook is difficult to follow.

Evidence:
Three sampled cases used different manual steps for the same task. Two analysts
reported that the screenshots no longer match the current tool interface. The
runbook has not been reviewed since the email platform upgrade.
```

That is much stronger than:

```text
The runbook is probably outdated.
```

The first example connects feedback to evidence. The second only expresses suspicion.

## From Evidence to Action

Audit findings should show the path from evidence to action. A useful progression is:

```text
Evidence → Analysis → Finding → Outcome → Action
```

Evidence shows what was reviewed. Analysis explains what the evidence means. The finding states the issue or confirmation. The outcome classifies the result. The action defines what should change.

This progression helps prevent audit reports from becoming either opinion pieces or raw evidence dumps.

A useful finding should usually include:

```text
- Observation
- Evidence
- Impact
- Recommendation
- Owner
- Priority
```

For example:

```text
Observation:
pwsh.exe is listed in the detection documentation but is not included in the
production detection logic.

Evidence:
The production query was reviewed on 2026-07-13. The rule includes
powershell.exe but not pwsh.exe.

Impact:
PowerShell 7 execution may not trigger the detection.

Recommendation:
Add pwsh.exe to the production detection logic and validate the updated rule.

Owner:
Detection Engineering

Priority:
Medium
```

This is actionable. It explains what was observed, why it matters and who should do something about it.

## Audit Outcomes

An audit should produce a clear outcome. The outcome should be specific enough that the team knows what to do next, but careful enough not to claim more than the evidence supports.

Possible outcomes include:

| Outcome                    | Meaning                                                                        |
| -------------------------- | ------------------------------------------------------------------------------ |
| Effective                  | The reviewed item works as expected within scope.                              |
| Effective with limitations | It works, but with documented gaps or conditions.                              |
| Partially effective        | Some requirements are met, but important gaps exist.                           |
| Ineffective                | It does not work as expected or cannot be trusted.                             |
| Not testable               | The audit could not determine effectiveness due to missing evidence or access. |
| Not applicable             | The reviewed expectation does not apply to the scoped environment.             |

The outcome should not hide uncertainty. If the audit cannot determine whether something works, that is itself a finding.

```text
Not testable because required endpoint telemetry was missing for 38% of scoped
devices.
```

That is a useful result. It tells the organisation where the problem is.

## Audit Metrics

Audit results can also produce useful operational metrics. The goal is not to create vanity metrics. The goal is to understand whether security capability is stable, improving or drifting.

Useful audit metrics may include:

| Metric                    | Why it matters                                                        |
| ------------------------- | --------------------------------------------------------------------- |
| Telemetry completeness    | Shows whether required data is available for the scoped population.   |
| Detection validation rate | Shows whether detections still fire when tested.                      |
| Not testable rate         | Shows where the team cannot verify capability.                        |
| Findings closure rate     | Shows whether audit findings become improvement work.                 |
| Re-audit pass rate        | Shows whether remediation actually worked.                            |
| Exception age             | Shows whether accepted deviations are becoming permanent blind spots. |
| Stale detection count     | Shows how many detections have not been reviewed within cadence.      |
| Stale baseline count      | Shows how many baselines may no longer represent the environment.     |

The most interesting metric is often not failure. The most interesting metric may be:

```text
Not testable.
```

A control, detection or assumption that cannot be tested may be more concerning than one that fails clearly. A clear failure can be fixed. An untestable capability may hide a deeper visibility or ownership problem.

## Controlled Validation and BAS

An audit may include controlled validation. Controlled validation may be performed manually, through internal test activity or by using breach and attack simulation tools. Tools such as BAS platforms or Atomic Red Team can help validate whether specific behaviours produce the expected telemetry, detection output and SOC workflow.

This is especially useful when the team wants to test detection coverage in a repeatable way without waiting for real attacker activity.

Examples include:

```text
- Execute a controlled PowerShell test
- Simulate credential dumping behaviour in an approved lab
- Validate whether scheduled task creation is detected
- Test whether suspicious command-line patterns are logged
- Confirm whether endpoint alerts reach the SOC queue
- Check whether detection severity and triage guidance are correct
```

The test must still be approved, scoped and documented. BAS or Atomic Red Team does not replace audit judgement. It provides controlled evidence that a detection, telemetry source or response workflow behaves as expected.

## Reference Audit: Office-Spawning PowerShell Detection

The reference below shows an operational audit of a detection rule. It is intentionally narrow. It does not audit all PowerShell detection coverage. It reviews whether one specific detection is still reliable enough to support SOC triage and detection engineering decisions.

### Audit Definition

| Field             | Value                                                                                       |
| ----------------- | ------------------------------------------------------------------------------------------- |
| Audit             | Office-Spawning PowerShell Detection Audit                                                  |
| ID                | AUD-000001                                                                                  |
| Objective         | Validate whether the Office-spawned PowerShell detection still works as intended.           |
| Scope             | Standard user workstations.                                                                 |
| Review period     | 2026-06-01 to 2026-06-30.                                                                   |
| Data sources      | Endpoint process telemetry, detection rule history, SOC alert records and related baseline. |
| Related baseline  | PowerShell Usage on User Workstations.                                                      |
| Related detection | Office Spawning PowerShell.                                                                 |
| Related playbooks | Suspicious Script Execution Playbook; Phishing Response Playbook.                           |
| Related runbooks  | Review Process Tree; Review Network Connections; Collect Endpoint Artefacts.                |
| Owner             | Detection Engineering.                                                                      |
| Participants      | Threat Hunting, SOC and Detection Engineering.                                              |

### Short Audit Summary

```text
Outcome:
Effective with limitations.

Key result:
The detection still identifies Office-spawned PowerShell from standard user
workstations, but the production logic misses pwsh.exe and one exclusion is too
broad.

Required actions:
- Add pwsh.exe to detection logic.
- Review and narrow the Legacy Finance exclusion.
- Update the related baseline.
- Improve SOC case documentation for approved exceptions.
```

The sections below show the evidence behind that summary.

### Objective

The objective is to determine whether the detection still identifies Office applications launching PowerShell from standard user workstations, whether required telemetry is present, whether alert output is useful for SOC triage and whether documented exclusions remain justified.

The audit should answer:

```text
- Does the rule still run successfully?
- Are required fields still available?
- Does the rule still detect the intended behaviour?
- Are exclusions still valid?
- Is alert volume acceptable?
- Can SOC analysts triage the alert?
- Does the related baseline still support the detection rationale?
```

### Expected Behaviour

The detection is expected to alert when Microsoft Office processes launch PowerShell or PowerShell-compatible interpreters on standard user workstations.

Expected parent processes include:

```text
- winword.exe
- excel.exe
- powerpnt.exe
- outlook.exe
- msaccess.exe
```

Expected child processes include:

```text
- powershell.exe
- pwsh.exe
```

The detection depends on endpoint process telemetry containing parent process, child process, user, device and command-line fields.

### Audit Procedure

The procedure should be repeatable enough that another reviewer can understand how the audit was performed.

```text
1. Confirm the current detection rule version.
2. Confirm the detection owner and review history.
3. Review the detection logic and documented scope.
4. Verify that required telemetry fields are present.
5. Run the detection logic over the review period.
6. Review alert history for the same period.
7. Sample closed alerts and review SOC triage quality.
8. Review exclusions and confirm they are still justified.
9. Compare observed activity with the related baseline.
10. Perform controlled validation if possible.
11. Document findings, limitations and recommended actions.
```

This procedure is not a runbook for a specific platform. A local organisation may create a runbook for the exact query, console, export and sampling steps.

### Telemetry Review

The audit should first confirm whether the required data exists. This is important because a detection can only be trusted within the visibility available to it.

Required fields:

```text
- Timestamp
- Device name
- User account
- Parent process name
- Parent process command line
- Child process name
- Child process command line
- Process ID
- Parent process ID
- Device group or business unit
```

Example telemetry finding:

```text
Endpoint process telemetry was available for 94% of scoped workstations during
the review period. Parent process name and child process command line were
present in the sampled events. Parent process command line was missing in 18% of
sampled events. This limitation reduces context but does not prevent the rule
from identifying the primary parent-child relationship.
```

This kind of finding is useful because it separates detection viability from context quality.

### Detection Logic Review

The detection logic should be reviewed against the intended behaviour. The purpose is not only to confirm that the rule runs, but to confirm that it still represents the behaviour it claims to detect.

Review questions:

```text
- Does the logic still match the documented behaviour?
- Does it include the intended Office parent processes?
- Does it include the intended PowerShell child processes?
- Has the data schema changed?
- Are exclusions applied correctly?
- Are time windows and scope filters correct?
- Does the rule produce the expected alert fields?
```

Example logic finding:

```text
The detection logic still matches the documented parent-child relationship.
However, pwsh.exe was not included in the production rule, although it is listed
in the detection documentation. This creates a coverage gap for PowerShell 7.
```

That is a concrete finding with a clear remediation path.

### Alert and Case Review

The audit should review whether the SOC can use the detection output. A detection that fires correctly can still fail operationally if alerts lack context, analysts do not know what to check, or cases are closed without enough evidence.

This may include sampling alerts and closed cases.

Review questions:

```text
- Did alerts contain enough context for triage?
- Did analysts review parent and child processes?
- Did analysts check related email or file activity?
- Were false positives documented consistently?
- Were escalations justified?
- Were closures supported by evidence?
- Did analysts follow the linked playbook or runbooks?
```

Example case review finding:

```text
Five alerts were generated during the review period. Four were closed as known
business automation and one was escalated for endpoint review. In two closed
cases, the analyst documented the business process but did not link the approved
exception record. The closures appear reasonable, but documentation quality is
inconsistent.
```

This is not only a detection finding. It is also a SOC process finding.

### Exclusion Review

Exclusions should be reviewed carefully. A detection exclusion is a controlled blind spot. Some exclusions are necessary, but they should not become permanent without ownership and review.

For each exclusion, confirm:

```text
- What behaviour is excluded
- Why it is excluded
- Who owns the exclusion
- When it was approved
- Whether the business need still exists
- Whether the exclusion is too broad
- Whether the exclusion has an expiry or review date
```

Example exclusion finding:

```text
One exclusion suppresses Office-spawned PowerShell for all devices in the Legacy
Finance application group. The original reason was valid, but the exclusion is
broad and has not been reviewed since the application upgrade. The exclusion
should be narrowed to the specific signed script path or reviewed for removal.
```

This is the type of issue audits are supposed to find. The finding does not say that the original exclusion was wrong. It says the exclusion has become too broad to trust without review.

### Baseline Review

The audit should check whether the related baseline still supports the detection. This matters because detections are often built on assumptions about what is rare, expected or unusual in the local environment.

Questions:

```text
- Is the baseline still within review date?
- Does the scoped population still match the detection scope?
- Has business behaviour changed?
- Are known exceptions still documented?
- Did recent alerts reveal expected behaviour missing from the baseline?
- Should the baseline be updated?
```

Example baseline finding:

```text
The related PowerShell workstation baseline is still within review date, but
recent alerts identified a new approved Finance macro that launches a signed
PowerShell script. The baseline should be updated with this known exception.
```

A detection audit may therefore update the baseline, not only the rule.

### Controlled Validation

Where possible, the detection should be validated with controlled test activity. Controlled validation should confirm that the rule can still observe the intended behaviour and that the alert contains useful context.

Validation should be approved and documented.

```text
Controlled validation:
A test document was used in an approved lab workstation to launch PowerShell
from winword.exe. The detection fired within the expected time window. The alert
included device, user, parent process, child process and command line. Related
network context was not included and should be considered for future enrichment.
```

If controlled validation cannot be performed, document the reason.

```text
Controlled validation was not performed because no approved test workstation was
available during the audit period.
```

That limitation matters. It does not invalidate the audit, but it changes how much confidence the team should place in the result.

### Audit Findings

The audit findings should be specific enough to create action. They should not simply say that the detection should be improved.

Example findings:

| Finding                                               | Impact                                 | Recommendation                                  |
| ----------------------------------------------------- | -------------------------------------- | ----------------------------------------------- |
| `pwsh.exe` missing from production logic              | PowerShell 7 execution may not alert.  | Add `pwsh.exe` to detection logic and validate. |
| Parent process command line missing in some telemetry | Triage context may be reduced.         | Review endpoint telemetry configuration.        |
| Broad Finance exclusion not reviewed                  | Legitimate blind spot may be too wide. | Review and narrow exclusion.                    |
| Cases lack linked exception records                   | Reviewability is reduced.              | Update SOC triage guidance.                     |

Good findings are specific. They create a path into ownership, remediation and later review.

### Audit Outcome

The audit should provide an overall outcome.

Example:

```text
Outcome:
Effective with limitations.

Summary:
The Office-spawned PowerShell detection still identifies the intended
parent-child relationship for standard user workstations. Required telemetry is
mostly available, and controlled validation confirmed that the rule fires.
However, the production logic does not include pwsh.exe, one exclusion is too
broad, and SOC case documentation is inconsistent for known business automation.

Recommended actions:
- Add pwsh.exe to detection logic.
- Review and narrow the Legacy Finance exclusion.
- Update the related baseline with the approved Finance macro.
- Update SOC triage guidance to require linked exception records.
```

This outcome is useful because it tells the team what works, what is limited and what needs attention.

## Audit Reporting

Audit reporting should make the review reproducible. It should show what was tested, what evidence was used, what outcome was reached and what should change.

A practical audit report may include:

```text
Title:
[Audit name]

Objective:
[What the audit reviewed]

Scope:
[What was included and excluded]

Trigger:
[Why the audit was performed]

Period:
[Review period]

Method:
[How the audit was performed]

Evidence:
[What data, cases or tests were reviewed]

Findings:
[Observed issues or confirmations]

Outcome:
[Effective, effective with limitations, partially effective, ineffective,
not testable or not applicable]

Recommendations:
[Actions to take]

Owner:
[Who owns each action]

Limitations:
[What the audit could not determine]

Review date:
[When to review again]
```

The report should separate facts from interpretation and avoid overstating the conclusion beyond the evidence. It should also make follow-up visible. If audit findings do not become owned actions, the audit may only document problems that the team will rediscover later.

## Audits as Threat Hunting Deliverables

An audit becomes a threat hunting deliverable when it uses hunting knowledge to test whether the organisation can still observe, detect or respond to important behaviours.

For example, a threat hunter may audit whether:

```text
- a detection created from a previous hunt still works
- a baseline is still valid
- a telemetry source still contains required fields
- a playbook reflects lessons learned from recent investigations
- a runbook still matches the current tool interface
- a control produces the expected evidence
- an assumption used in previous hunts still holds
```

This is valuable because threat hunting often creates assumptions.

Examples:

```text
We can see PowerShell command lines.
Office-spawned PowerShell is rare.
Privileged users do not authenticate from unmanaged devices.
Mailbox forwarding is logged.
Endpoint isolation is available for all managed workstations.
```

Those assumptions should not be trusted forever. Audits check whether they still hold.

## Audits and Operational Maturity

A low-maturity team discovers broken assumptions during incidents. A more mature team looks for broken assumptions before incidents.

That is one of the reasons audits matter. An audit may reveal that the team cannot observe a behaviour, that a detection no longer works, that cases are closed without evidence, that a runbook is stale or that a playbook does not match how analysts actually work.

These are not embarrassing findings. They are opportunities to fix operational drift.

A mature SOC should be able to say:

```text
We checked this.
Here is what still works.
Here is what does not.
Here is what we are improving.
```

That is a stronger position than assuming the operation works because the documentation says it should.

## Common Failure Modes

Audits often fail when they become too broad, too shallow or too disconnected from operational reality.

| Failure mode                                  | Why it hurts                                                                                                                      |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| No clear scope                                | The audit result cannot be interpreted safely.                                                                                    |
| Only document review                          | The audit checks whether documents exist, not whether work happens.                                                               |
| No evidence                                   | Findings become opinion.                                                                                                          |
| Too broad                                     | The audit produces general statements without actionable findings.                                                                |
| No owner                                      | Recommendations do not become improvements.                                                                                       |
| No follow-up                                  | The same gaps are rediscovered later.                                                                                             |
| No operational input                          | Analysts are not involved, so the audit misses real workflow issues.                                                              |
| Overstated conclusion                         | The report claims more than the evidence supports.                                                                                |
| No link to baselines or detections            | The audit does not improve monitoring capability.                                                                                 |
| Treated as blame                              | People hide problems instead of exposing drift.                                                                                   |
| Cost-driven telemetry reduction without audit | Logging is reduced to save cost, but detection and investigation capability are weakened without anyone understanding the impact. |
| Migration without validation                  | Detection content is moved to a new platform, but nobody verifies whether the behaviour still alerts or supports SOC triage.      |

The most useful audits are specific, evidence-based and connected to improvement work. An audit should make reality visible.

## Working Position for This Book

For this book, an audit is treated as an operational review that checks whether an assumption, detection, baseline, process, runbook, playbook, telemetry source or control still holds within a defined scope.

A useful audit is not a paperwork exercise. It is a way to expose drift, validate capability and create improvement work.

An audit earns its place when it helps the team answer a practical question:

```text
Can we still trust this?
```

That is the standard. Not because every process needs constant review, but because security operations change whether the documentation changes or not.

## Resources

* [NIST SP 800-53 Rev. 5: Security and Privacy Controls](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
* [NIST SP 800-61 Rev. 2: Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
* [MITRE ATT&CK: PowerShell](https://attack.mitre.org/techniques/T1059/001/)
* [MITRE ATT&CK: Command and Scripting Interpreter](https://attack.mitre.org/techniques/T1059/)
* [Sigma: Generic Signature Format for SIEM Systems](https://sigmahq.io/)
* [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
* [Splunk SURGe: PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html)
* [The ThreatHunting Project: Hunting Maturity Model](https://www.threathunting.net/hunting-maturity-model)

## Revision

| Revised Date | Comment                                                                                                                                                                                            |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-13   | Initial version. Introduced audits as threat hunting deliverables and explained how audits can review telemetry, detections, baselines, assumptions and operational controls.                      |