---
title: "SOP"
description: "A practical explanation of Standard Operating Procedures in security operations, how they define required ways of working, and how they differ from playbooks and runbooks."
date: 2024-10-19T18:13:21+02:00
lastmod: 2026-07-13
draft: false
hidden: false
weight: 1
tags:
  - SOP
  - standard operating procedure
  - process documentation
  - SOC
  - threat hunting
  - incident response
  - governance
keywords:
  - SOP
  - standard operating procedure
  - security SOP
  - SOC SOP
  - incident response SOP
  - process documentation
  - security operations documentation
  - playbook
  - runbook
  - governance
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**A Standard Operating Procedure, or SOP, defines the required way a recurring process should be performed. In security operations, some activities must be handled consistently regardless of who is on shift, which team is involved or which tool is being used. Incident escalation, evidence handling, case documentation, stakeholder notification, change approval and exception handling are examples of areas where the organisation needs a clear standard.**

An SOP exists to define that standard. If the standard is unclear, the work will vary. An SOP does not need to explain every click or command. That level of detail usually belongs in a runbook. It does not need to describe every decision path for a scenario. That usually belongs in a playbook. An SOP defines the expected way of working, the roles involved, the required controls, the approval points and the minimum documentation standard.

Without SOPs, operational work becomes inconsistent. One analyst may escalate early, another may wait too long, and another may document the case in a way nobody else can review. That creates risk, not because people are careless, but because the standard is unclear.

At its simplest:

```text
An SOP defines the required way of working.
```

It is the standard the team is expected to follow.

## What an SOP Is

An SOP is a formal process document. It explains how a recurring process should be performed at an organisational or team level.

An SOP must be enforceable. If nobody can tell whether the SOP was followed, it is only guidance with a formal title. A good SOP defines the rules of the road. It should make clear what must happen, who is responsible, which conditions trigger the process, what approvals are needed, what must be documented and how exceptions should be handled.

An SOP may include:

* purpose
* scope
* process trigger
* roles and responsibilities
* required inputs
* required outputs
* process requirements
* approval points
* escalation requirements
* documentation requirements
* compliance or policy requirements
* exception handling
* review cadence
* related playbooks
* related runbooks
* change history

The important word is **required**.

An SOP should not merely describe what people often do. It should define the standard that people are expected to follow.

For example, a weak SOP statement would be:

```text
Analysts should document important incidents.
```

That is too vague. It does not define what “important” means, what documentation is required, where it should be recorded or who verifies it.

A stronger SOP statement would be:

```text
All security incidents classified as Medium or higher must have a case record,
assigned owner, incident summary, timeline, actions taken, current status,
escalation decision and closure rationale recorded in the incident management
system.
```

That is closer to an SOP because it defines a requirement.

## What an SOP Is Not

An SOP is not a playbook. A playbook explains how to handle a scenario, such as phishing, ransomware or suspected account compromise.

An SOP is not a runbook. A runbook explains how to perform a specific task, such as revoking sessions, collecting email headers or isolating an endpoint.

An SOP is also not a policy, although it may support one. A policy usually states a higher-level requirement. An SOP translates that requirement into an operational process.

| Document type | Main concern                             | Example                                          |
| ------------- | ---------------------------------------- | ------------------------------------------------ |
| Policy        | What must be true at a governance level? | Security incidents must be managed and reported. |
| SOP           | How must the process be performed?       | Incident escalation SOP                          |
| Playbook      | How should a scenario be handled?        | Ransomware response playbook                     |
| Runbook       | How is a task executed?                  | Isolate host in EDR                              |

A useful separation is:

```text
Policy   → the governing requirement
SOP      → the required operating process
Playbook → the scenario response path
Runbook  → the execution procedure
```

When this separation is lost, documentation becomes hard to use. SOPs become bloated with tool steps, playbooks become full of governance text, and runbooks become unclear because they try to explain why the organisation works the way it does.

## SOP, Playbook and Runbook

SOPs, playbooks and runbooks often support the same operational process, but they should not compete for the same purpose.

In a phishing case, the SOP may define when a case must be created, how evidence must be handled, when escalation is required and what documentation must exist before closure.

The playbook applies those standards to the phishing scenario. The runbooks explain how to perform specific tasks, such as collecting email headers, removing emails from mailboxes, blocking a URL or revoking user sessions.

The relationship may look like this:

```text
SOP:
Defines the required operating standard.

Playbook:
Applies that standard to a scenario.

Runbook:
Executes specific tasks required by the playbook or SOP.
```

This matters because an SOP should be stable enough to guide operations over time. Tool-specific details may change frequently, but the requirement to preserve evidence, assign ownership, document actions and escalate certain conditions should remain more stable.

## What Makes an SOP Useful

A useful SOP is clear, enforceable and operationally realistic. It should define a process that people can actually follow during real work.

An SOP should not be written only for auditors. If the people doing the work cannot understand or apply it, the SOP will fail even if it looks formal.

| Quality              | Why it matters                                                       |
| -------------------- | -------------------------------------------------------------------- |
| Defined scope        | People know when the SOP applies and when it does not.               |
| Defined trigger      | The process starts under known conditions.                           |
| Assigned roles       | People know who is responsible, accountable, consulted and informed. |
| Required outputs     | The process produces something reviewable.                           |
| Approval points      | Sensitive actions are controlled.                                    |
| Escalation rules     | Serious cases do not remain stuck at the wrong level.                |
| Exception handling   | Deviations are visible and justified.                                |
| Linked documentation | Related playbooks, runbooks and policies are easy to find.           |
| Reviewability        | The process can be checked after the fact.                           |
| Maintenance          | The SOP remains aligned with the organisation and tooling.           |

The value of an SOP is not that it makes work bureaucratic. The value is that it defines the minimum standard for work that must be consistent, accountable and reviewable.

## SOP Structure

An SOP should be structured around the process it governs. The exact format depends on the organisation, but a practical security SOP often contains the following sections.

```text
Title:
[Process name]

Purpose:
[Why this SOP exists]

Scope:
[Where the SOP applies and where it does not]

Trigger:
[Conditions that start the process]

Roles and responsibilities:
[Who does what]

Process requirements:
[What must happen]

Required records:
[What must be documented]

Approval points:
[Actions requiring approval]

Escalation:
[When and where to escalate]

Exceptions:
[How deviations are handled]

Related policies:
[Policies this SOP supports]

Related playbooks:
[Scenario guides that rely on this SOP]

Related runbooks:
[Task procedures that support this SOP]

Review:
[Owner, review cadence and change process]
```

This structure should be adapted. The goal is not to make every SOP look identical. The goal is to make the required operating standard clear enough that it can be followed and reviewed.

## Reference SOP: Security Case Documentation and Escalation

The reference below shows an SOP that governs a recurring security operations process: case documentation and escalation.

It is intentionally not a playbook. It does not explain how to handle phishing, ransomware or malware. It defines the minimum process requirements for documenting and escalating security cases.

It is also not a runbook. It does not explain how to use a specific case management tool. Those details should live in tool-specific runbooks.

### SOP Definition

| Field             | Value                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| SOP               | Security Case Documentation and Escalation                                                                                       |
| ID                | SOP-000001                                                                                                                       |
| Purpose           | Define minimum requirements for documenting, owning, escalating and closing security cases.                                      |
| Scope             | SOC cases, threat hunting findings, security alerts and suspected incidents handled by security operations.                      |
| Audience          | SOC analysts, threat hunters, incident responders, SOC leads and security managers.                                              |
| Owner             | Security Operations                                                                                                              |
| Related playbooks | Phishing Response Playbook; Suspected Account Compromise Playbook; Malware Investigation Playbook; Ransomware Response Playbook. |
| Related runbooks  | Create Security Case; Update Case Timeline; Attach Evidence; Escalate to Incident Response; Close Security Case.                 |
| Related policies  | Incident Response Policy; Evidence Handling Policy; Information Classification Policy.                                           |
| Review cadence    | At least annually, or after major incident, tooling or process changes.                                                          |

### Purpose

This SOP defines the minimum standard for documenting and escalating security cases.

The purpose is to make sure that security work is traceable, reviewable and safe to hand over. A case should not depend on the memory of the analyst who handled it. Another analyst, SOC lead or incident responder should be able to understand what happened, what was observed, what decisions were made and what remains unresolved.

The SOP applies whether the case starts from an alert, user report, threat hunting observation, vulnerability signal, threat intelligence lead or manual investigation.

### Scope

This SOP applies to security cases handled by the SOC, threat hunting team or incident response function.

It applies to:

* security alerts requiring analyst review
* user-reported security events
* phishing reports
* suspected account compromise
* suspected endpoint compromise
* threat hunting findings
* suspicious activity requiring follow-up
* cases escalated from IT or business teams
* cases that may become incidents

This SOP does not define the full response path for every scenario. Scenario-specific guidance belongs in playbooks. Tool-specific execution steps belong in runbooks.

### Process Trigger

A security case must be created or updated when there is a security-relevant event that requires investigation, tracking, escalation decision or handover.

Typical triggers include:

```text
- Security alert requiring analyst assessment
- User report of suspicious activity
- Threat hunting observation requiring follow-up
- Suspicious authentication or endpoint activity
- Confirmed or suspected phishing email
- Malware detection requiring validation
- External notification about possible compromise
- Vulnerability exploitation signal
- Request from incident response, management, legal or compliance
```

If the analyst is uncertain whether a case should be created, the default should be to create a lightweight case record and document the reason for closing or downgrading it later.

### Roles and Responsibilities

| Role               | Responsibilities                                                                                                              |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| SOC Analyst        | Create and update cases, perform initial triage, document findings, recommend next action and escalate when criteria are met. |
| Threat Hunter      | Document hunt findings, evidence, scope, assumptions, limitations and recommended follow-up.                                  |
| Senior Analyst     | Review complex cases, validate escalation decisions and support interpretation of uncertain evidence.                         |
| Incident Responder | Take ownership when the case becomes an incident or requires formal incident response.                                        |
| SOC Lead           | Ensure process compliance, review escalations, resolve ownership issues and approve exceptions where required.                |
| Service Owner      | Support investigation, containment or recovery actions for systems under their responsibility.                                |
| Security Manager   | Own the SOP, ensure periodic review and resolve process gaps or recurring failures.                                           |

The exact role names may vary by organisation. The SOP should use role names that match the local operating model.

### Minimum Case Record

Every security case covered by this SOP must contain enough information to support review, handover and decision-making.

Minimum required fields:

```text
- Case title
- Case owner
- Date and time opened
- Source of case
- Affected user, system, asset or service if known
- Initial description
- Current status
- Severity or priority
- Evidence collected
- Actions taken
- Decisions made
- Escalation status
- Limitations or uncertainty
- Next action
- Closure rationale when closed
```

The case record does not need to be long. It needs to be clear. A short and accurate case record is better than a long case record that hides the operational facts.

### Evidence Requirements

Evidence must be recorded or linked in a way that allows another analyst to understand the basis for the decision.

Depending on the case, evidence may include:

```text
- Alert details
- Log entries
- Email headers
- Message IDs
- File hashes
- URLs
- Domains
- IP addresses
- Screenshots
- Query results
- Endpoint telemetry
- Authentication events
- User statements
- Timeline entries
- Analyst notes
```

Evidence should be preserved according to the evidence handling policy. If the original evidence cannot be preserved, the limitation must be documented.

The analyst should separate facts from interpretation. For example:

```text
Observed:
User clicked the link at 09:42.

Not yet confirmed:
Whether credentials were submitted.
```

This is better than writing:

```text
User compromised.
```

unless compromise has actually been confirmed.

### Severity and Priority

Every case must have an initial severity or priority assigned. The classification may change as new evidence becomes available.

Severity should consider:

* affected user or asset
* business impact
* confidence in malicious activity
* evidence of user interaction
* evidence of execution
* evidence of credential exposure
* evidence of lateral movement
* sensitivity of data or system involved
* scope of affected users or systems
* regulatory, legal or privacy implications

The SOP should not require perfect certainty before severity is updated. If the available evidence suggests higher risk, the case should be raised and reviewed rather than left at a low level until certainty is achieved.

### Escalation Requirements

A case must be escalated when it exceeds routine triage or when the analyst does not have the authority, access or confidence required to continue safely.

Escalate to a senior analyst, SOC lead or incident responder when:

```text
- Privileged or sensitive account is involved
- Malware execution is confirmed or strongly suspected
- Credential theft is confirmed or strongly suspected
- Lateral movement is suspected
- Data access or exfiltration is suspected
- Multiple users, hosts or business units are affected
- Business impact is reported
- Legal, privacy or regulatory concerns may apply
- The case requires containment with business impact
- The analyst cannot interpret critical evidence
- The case may require formal incident declaration
```

Escalation does not mean the analyst failed. Escalation means the case has reached a threshold where additional authority, expertise or coordination is required.

### Handover Requirements

A case must be ready for handover when ownership changes, when a shift ends or when the case is escalated.

A handover must include:

```text
- Current situation
- Confirmed facts
- Working assessment
- Actions already taken
- Evidence collected
- Open questions
- Risks or concerns
- Next recommended action
- Relevant links and attachments
```

A case should not be handed over with only a chat message or verbal explanation. The case record must be updated so that the next person can continue the work without reconstructing the investigation from memory.

### Approval Points

Some actions may require approval because they can affect users, systems, business operations or evidence integrity.

Approval may be required before:

```text
- Isolating a production endpoint
- Disabling a user account
- Blocking a business-critical domain or IP
- Removing email from many mailboxes
- Resetting credentials for privileged users
- Taking systems offline
- Notifying external parties
- Sharing indicators outside the organisation
- Declaring or downgrading an incident
- Closing a case with unresolved uncertainty
```

The local organisation must define who can approve these actions. The SOP should not leave approval authority ambiguous.

### Exceptions

There may be situations where the standard process cannot be followed. For example, a tool may be unavailable, the original evidence may be missing, a business-critical system may prevent normal containment, or an urgent decision may be required before all documentation is complete.

Exceptions are allowed only when they are documented.

If an SOP is not followed, the deviation must be visible.

An exception record should include:

```text
- What part of the SOP could not be followed
- Why the exception was necessary
- Who approved or accepted the exception
- What risk was introduced
- What compensating action was taken
- Whether follow-up is required
```

Undocumented exceptions are process failures. Documented exceptions are operational decisions that can be reviewed.

### Closure Requirements

A case may be closed only when the case record supports the closure decision.

Before closure, the analyst must confirm that:

```text
- The case has a clear classification or closure category
- Required evidence has been recorded or linked
- Actions taken are documented
- Escalation criteria were considered
- Open questions are resolved or documented as accepted uncertainty
- Follow-up actions have owners
- The closure rationale is clear
```

Closure does not mean that every possible question has been answered. Closure means the case has reached a documented and justified operational decision.

Example closure statement:

```text
The reported email was assessed as suspicious but not confirmed malicious.
No user interaction was identified.
The message was removed from the reporting mailbox after evidence preservation.
No similar messages were found in the email gateway search.
No escalation required.
Case closed with low residual risk.
```

### Required Outputs

This SOP requires the following outputs when applicable:

| Output                        | Required when                                                    |
| ----------------------------- | ---------------------------------------------------------------- |
| Case record                   | Every case covered by this SOP.                                  |
| Evidence links or attachments | Evidence was collected or used for a decision.                   |
| Timeline                      | Case involves multiple actions, escalation or incident response. |
| Escalation note               | Case was escalated, downgraded or reviewed by senior staff.      |
| Closure rationale             | Every closed case.                                               |
| Follow-up action              | Work remains after case closure or transfer.                     |
| Exception record              | Standard process could not be followed.                          |

These outputs make the work reviewable. If the required output is missing, the process cannot be verified.

### Quality Control

The SOC lead, senior analyst or assigned reviewer may review cases to ensure this SOP is being followed.

A quality control review may ask:

```text
- Was the case created when required?
- Was ownership clear?
- Was severity assigned and updated when needed?
- Was evidence preserved or linked?
- Were facts separated from assumptions?
- Were escalation criteria considered?
- Were approval points followed?
- Was handover sufficient?
- Was closure justified?
- Were exceptions documented?
```

Quality control should not be treated only as fault-finding. It is also how the team improves training, documentation, playbooks, runbooks and tooling.

### Review and Maintenance

This SOP should be reviewed at least annually and after major changes to the operating model, case management system, escalation process, incident response policy or regulatory requirements.

It should also be reviewed after incidents or retrospectives reveal that cases were not documented, escalated, handed over or closed consistently.

Review questions:

```text
- Does the SOP still match how the SOC operates?
- Are role names and responsibilities still correct?
- Are escalation thresholds clear?
- Are approval points realistic?
- Are required outputs sufficient?
- Are analysts following the SOP?
- Are exceptions being documented?
- Do related playbooks and runbooks still align with the SOP?
```

Possible improvement outputs:

```text
- Updated SOP
- Updated playbooks
- Updated runbooks
- Revised escalation criteria
- Improved case templates
- Additional analyst training
- Tooling or workflow changes
- Automation requirements
```

An SOP that is not maintained becomes decorative governance. It may still look official, but it no longer protects the operation.

## SOPs and Operational Maturity

SOPs are often misunderstood as paperwork. In a mature SOC, they are part of the operating model.

A good SOP makes expectations explicit. It defines what must happen, who owns the process, when escalation is required, what evidence must be preserved and what output must exist when the work is done.

This matters because security operations are not only about technical skill. They are also about coordination, accountability and repeatability.

A team may have skilled analysts and still operate inconsistently if the required process is unclear. A team may have strong tools and still struggle if cases are not documented, escalated or handed over properly.

The SOP provides the minimum standard that makes the work governable.

## Common Failure Modes

SOPs usually fail when they are disconnected from real work.

They may be formally approved, stored in the right place and still ignored because they are too vague, too long, too theoretical or impossible to follow during operations.

| Failure mode                 | Why it hurts                                                    |
| ---------------------------- | --------------------------------------------------------------- |
| Too vague                    | People cannot tell what is actually required.                   |
| Too detailed                 | The SOP becomes a runbook and breaks when tools change.         |
| No owner                     | Nobody maintains the SOP when the organisation changes.         |
| No trigger                   | People do not know when the process starts.                     |
| No defined roles             | Responsibility becomes ambiguous.                               |
| No required outputs          | The process cannot be reviewed.                                 |
| No exception handling        | Deviations happen silently.                                     |
| No link to playbooks         | Scenario response drifts away from the standard process.        |
| No link to runbooks          | Required tasks are defined but not executable.                  |
| Not used in training         | Analysts learn informal habits instead of the official process. |
| Not reviewed after incidents | Lessons learned never update the operating standard.            |

The most dangerous SOPs are the ones that look official but do not influence behaviour. They create the appearance of control without improving the operation.

## Working Position for This Book

For this book, an SOP is treated as an operating standard.

It defines the required way a recurring process should be performed. It should be clear enough to follow, stable enough to govern the work and practical enough to survive contact with real operations.

A useful SOP does not try to explain every scenario or every technical step. It defines the standard that playbooks and runbooks must align with.

An SOP earns its place when it makes security work more consistent, accountable and reviewable.

That is the standard. Not because security teams need more paperwork, but because important operational processes should not depend on memory, habit or individual interpretation.

## Resources

* [NIST SP 800-61 Rev. 2: Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
* [CISA: Incident Response](https://www.cisa.gov/resources-tools/resources/incident-response)
* [MITRE ATT&CK: Phishing](https://attack.mitre.org/techniques/T1566/)
* [MITRE ATT&CK: Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001/)
* [ATC RE&CT Framework](https://atc-project.github.io/atc-react/)
* [The RE&CT Framework](https://github.com/atc-project/atc-react)

## Revision

| Revised Date | Comment                                                                                                                                                             |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-13   | Major rewrite. Reframed the page as an explanation of SOPs as enforceable operating standards, with a reference SOP for security case documentation and escalation. |
| 2024-10-26   | Page added                                                                                                                                                          |
