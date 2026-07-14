---

title: "Playbook"
description: "A practical explanation of security playbooks, how they guide scenario-based response and investigation, and how they differ from SOPs and runbooks."
date: 2024-10-19T18:13:00+02:00
lastmod: 2026-07-13
draft: false
hidden: false
weight: 2
tags:
- playbook
- process documentation
- SOC
- threat hunting
- incident response
- phishing
keywords:
- playbook
- security playbook
- SOC playbook
- incident response playbook
- phishing playbook
- threat hunting playbook
- SOP
- runbook
- process documentation
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**A playbook is a scenario-based guide that helps a security team respond to a specific type of situation. In security operations, not every situation can be handled by a simple checklist. Some cases require judgement, branching decisions, escalation, evidence collection, containment choices and communication with several stakeholders. A playbook gives the team a structured way to approach a known scenario, such as phishing, ransomware, suspected account compromise, malware infection, data exfiltration or suspicious cloud activity.**

Without playbooks, similar incidents may be handled differently every time. One analyst may preserve the right evidence, another may skip important checks, and a third may escalate too late or too early. A playbook reduces that variation by giving the team a shared response path.

Playbooks also connect detections to response. A detection, alert, user report or threat hunting observation may tell the team that something needs attention, but the playbook explains what should happen next.

A playbook does not need to describe every click, command or tool-specific action. That level of detail often belongs in a runbook. A playbook should instead help the analyst understand the situation, decide what matters, collect the right evidence, involve the right people and move the case forward.

A practical way to say it is:

```text
A playbook explains how to handle a scenario.
```

It does not define every step. It defines how to think and act.

## What a Playbook Is

A playbook is a guide for handling a type of operational situation. It is reusable across similar situations, but flexible enough to adapt when the evidence changes.

A playbook describes the response logic, investigation flow, decision points, escalation criteria, evidence requirements and expected outputs for a scenario. It should help the team understand what kind of situation they are dealing with, what questions need to be answered, which actions may be appropriate and when the case should move from routine handling to incident response.

A playbook may include:

* purpose and scope
* scenario description
* severity guidance
* roles and responsibilities
* initial triage steps
* investigation questions
* evidence to collect
* decision points
* containment options
* escalation criteria
* communication requirements
* related SOPs
* related runbooks
* documentation requirements
* lessons learned and review steps

A playbook should help the team answer:

```text
How should we approach this type of situation?
```

It should not try to replace analyst judgement. A playbook is not a script for thinking. It is a structure that helps the analyst avoid missing important steps while still allowing the investigation to adapt to evidence.

## Playbook, SOP and Runbook

Playbooks are often confused with SOPs and runbooks. They are related, but they do different jobs.

| Document type | Main question                          | Example                     |
| ------------- | -------------------------------------- | --------------------------- |
| SOP           | What is the required standard process? | Incident escalation SOP     |
| Playbook      | How should we handle this scenario?    | Phishing response playbook  |
| Runbook       | Exactly how do I perform this task?    | Remove email from mailboxes |

For example, a phishing playbook may reference the incident escalation SOP, the evidence handling SOP, a runbook for analysing email headers, a runbook for extracting URLs, a runbook for removing messages from mailboxes, a runbook for resetting credentials and a runbook for revoking user sessions.

The playbook gives the scenario structure. The SOP defines the standard process. The runbook gives the exact execution steps.

This is important because documentation becomes difficult to maintain when every document tries to do everything. A playbook that contains every technical step becomes too large to use during an active case. A runbook that tries to explain the entire scenario becomes unclear. An SOP that includes tool-specific instructions may become outdated as soon as the tool changes.

Good documentation keeps each layer clear.

```text
SOP      → the required standard
Playbook → the scenario response path
Runbook  → the exact execution steps
```

## What Makes a Good Playbook

A good playbook should be practical enough to use during real work. It should not be so vague that it only says “investigate phishing”, but it should also not be so detailed that it becomes a long runbook full of tool-specific steps.

The value of a playbook is usually found in its decision logic. It should help the analyst understand what matters, what evidence changes the response, when to contain, when to escalate and when the case can be closed.

A useful playbook usually has the following qualities:

| Quality               | Why it matters                                                                  |
| --------------------- | ------------------------------------------------------------------------------- |
| Scenario-specific     | The playbook is written for a recognisable situation.                           |
| Decision-oriented     | It helps analysts decide what to do next.                                       |
| Evidence-focused      | It explains what information must be collected.                                 |
| Flexible              | It allows analysts to adapt when the case changes.                              |
| Linked to runbooks    | It points to exact task instructions where needed.                              |
| Linked to SOPs        | It follows organisational standards for escalation, evidence and communication. |
| Maintained            | It is updated when tools, threats or processes change.                          |
| Usable under pressure | It is clear enough to use during an active case.                                |

A playbook should reduce confusion, not add paperwork. If the analyst needs to read a long theoretical document before they can make the next operational decision, the playbook is probably too heavy. If the playbook only contains generic advice, it is probably too weak.

## Common Playbook Sections

There is no single perfect playbook format. The structure depends on the organisation, tooling, process maturity and scenario. Still, many useful playbooks contain the same core sections.

```text
Title:
[Scenario name]

Purpose:
[Why this playbook exists]

Scope:
[When this playbook should and should not be used]

Trigger:
[Alerts, reports, intelligence, observations or conditions that activate the playbook]

Severity:
[Initial severity guidance and escalation conditions]

Audience:
[Who the playbook is written for]

Roles:
[Who is responsible for triage, investigation, containment, communication and approval]

Initial triage:
[First checks to confirm whether the scenario applies]

Investigation:
[Questions to answer and evidence to collect]

Decision points:
[Conditions that change the response path]

Containment:
[Possible containment actions and approval requirements]

Eradication and recovery:
[Actions to remove the threat and restore normal operations]

Communication:
[Who must be informed and when]

Documentation:
[What must be recorded in the case or incident system]

Related SOPs:
[Relevant standard operating procedures]

Related runbooks:
[Relevant task execution guides]

Review:
[Lessons learned, tuning, detection improvements and playbook updates]
```

This structure should be adapted. The goal is not to force every playbook into the same format. The goal is to make sure the scenario can be handled consistently.

## Reference Example: Phishing Response Playbook

The example below is written as a reference playbook. It shows how a phishing response playbook may look when it is used as scenario-based guidance in a SOC.

It is intentionally not a tool-specific runbook. It does not describe every click, query or command needed to remove an email, inspect a URL, review sign-in logs or revoke sessions. Those details should live in linked runbooks. The purpose of this playbook is to guide the analyst through the scenario: what to check, what to decide, when to contain, when to escalate and what to document.

### Playbook Summary

```text
Playbook:
Phishing Email Response

ID:
PB-000001

Audience:
SOC analysts, Tier 2 analysts and incident responders

Purpose:
This playbook provides a structured approach for triaging, investigating,
containing and documenting phishing emails reported by users or detected by
security controls.

The playbook should help the analyst determine whether the email is benign,
suspicious, malicious or inconclusive, whether users interacted with it, and
whether containment or escalation is required.

Scope:
This playbook applies to user-reported phishing emails, phishing alerts from
email security tools, suspicious emails identified during threat hunting and
phishing campaigns discovered through threat intelligence.

The playbook covers emails containing suspicious links, attachments, credential
harvesting pages, brand impersonation or malware delivery attempts.

This playbook does not replace the full incident response process. If there is
evidence of credential theft, malware execution, lateral movement, data
exfiltration or business impact, the case should be escalated according to the
incident escalation SOP.

Classification:
Initial Access / Phishing

ATT&CK mapping:
TA0001 - Initial Access
T1566.001 - Spearphishing Attachment
T1566.002 - Spearphishing Link

TLP:
AMBER

PAP:
WHITE

Severity:
Low to High, depending on user interaction, payload execution, account exposure,
affected user role, campaign scale and business impact.
```

### Trigger

This playbook may be used when a phishing-related case is created from a user report, an email security alert, threat intelligence, suspicious URL detection, malware attachment detection or a hunt that identifies phishing-related activity.

The playbook may also be used when multiple users report similar emails, when suspicious sign-in activity follows email delivery, or when a known campaign appears to target the organisation.

```text
Typical triggers:
- User reports suspicious email to SOC
- Email security tool generates phishing alert
- Suspicious URL is detected in delivered email
- Malware attachment is detected or sandboxed
- Threat intelligence reports active phishing campaign
- Multiple users report similar emails
- Suspicious sign-in activity follows email delivery
- Threat hunt identifies phishing-related activity
```

### Initial Triage

The first triage decision is whether the email is likely benign, suspicious, malicious or unknown. The analyst should quickly establish who received the email, what the email attempted to make the user do, whether any user interacted with it and whether similar messages exist elsewhere in the environment.

```text
Initial triage questions:
- Who reported the email?
- Who received the email?
- When was it delivered?
- Was the sender internal, external or spoofed?
- Does the message contain links?
- Does the message contain attachments?
- Does the message ask for credentials, payment, approval or file access?
- Did any user click a link?
- Did any user open an attachment?
- Did any user submit credentials?
- Are similar emails present in other mailboxes?
```

The analyst should collect enough evidence to classify the email and decide whether containment or escalation is required. Evidence may include the original email, headers, sender address, reply-to address, subject line, delivery timestamp, recipient list, URLs, attachments, screenshots, user interaction data and related alerts.

The playbook should point to runbooks for the exact collection steps.

```text
Related runbooks:
- Collect email headers
- Extract URLs from reported email
- Search for similar messages
- Check user clicks in proxy logs
- Check attachment detonation results
- Review user sign-in activity
```

### Analysis

The analysis phase determines what the email attempted to do and whether users were exposed. The goal is to understand intent, impact and user exposure, not merely to decide whether the email “looks suspicious”.

The analyst should assess sender reputation, domain similarity, SPF/DKIM/DMARC results, URL destinations, redirects, attachment behaviour, sandbox results, credential harvesting indicators, brand impersonation, social engineering theme, delivery scope, user interaction and related endpoint or identity activity.

```text
Possible classification outcomes:

Benign:
The email appears legitimate or harmless.

Suspicious:
The email has concerning traits, but there is not enough evidence to confirm
malicious intent.

Malicious phishing:
The email is confirmed malicious and requires containment or response.

Inconclusive:
The available evidence is insufficient to classify the email with confidence.
```

The analyst should avoid overstating the result. A good playbook should encourage precise language, especially when the evidence is incomplete.

For example, this statement separates confirmed facts from uncertainty:

```text
The email contains a suspicious link to a credential harvesting page.
One user clicked the link.
Credential submission has not yet been confirmed.
```

That is better than:

```text
The user was compromised.
```

The first statement supports operational decision-making. The second may be true, but it is not supported unless account compromise has actually been confirmed.

### Decision Points

Decision points are the core of the playbook. They define what should happen when evidence changes, when risk increases, when containment is needed or when the case should be escalated.

| Decision point                                            | Response path                                                                                     |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Email classified as benign                                | Document classification and close the case.                                                       |
| Email suspicious but not confirmed malicious              | Preserve evidence, document uncertainty and monitor for related activity.                         |
| Email confirmed malicious with no user interaction        | Remove email from mailboxes, block indicators and document the case.                              |
| User clicked link without confirmed credential submission | Review sign-in activity, consider session revocation if risk is elevated and monitor the account. |
| Credential submission suspected or confirmed              | Reset password, revoke sessions, review account activity and escalate if needed.                  |
| Attachment opened or payload executed                     | Escalate to endpoint investigation and incident response.                                         |
| Similar emails delivered to many users                    | Treat as campaign activity and coordinate containment.                                            |
| Internal account sent phishing email                      | Investigate possible internal account compromise.                                                 |
| Sensitive user or privileged account affected             | Escalate according to the incident escalation SOP.                                                |
| Evidence inconclusive                                     | Document uncertainty, preserve evidence and define next steps.                                    |

The table does not replace judgement. It gives the analyst a set of response paths that can be selected and adapted based on evidence.

### Containment

Containment depends on the evidence. The analyst should not apply every possible containment action automatically. The response should match the classification, user interaction, affected accounts, business impact and confidence level.

```text
Possible containment actions:
- Remove email from affected mailboxes
- Block sender, domain, URL or file hash
- Submit URL or file to security tooling
- Reset affected user password
- Revoke active sessions
- Disable malicious inbox rules
- Isolate affected endpoint
- Block OAuth application or revoke consent
- Notify affected users
- Escalate to incident response
```

Containment should follow the organisation’s approval and escalation requirements. The playbook should link to the relevant SOPs and runbooks instead of duplicating every technical step.

```text
Related SOPs:
- Incident escalation
- Evidence handling
- User notification
- External reporting

Related runbooks:
- Remove email from mailboxes
- Block domain or URL
- Reset user password
- Revoke user sessions
- Isolate endpoint in EDR
- Review mailbox rules
- Review OAuth consent grants
```

### Escalation Criteria

The case should be escalated when the risk exceeds normal phishing triage. Escalation is not only about technical severity. It may also depend on the user affected, business impact, legal exposure, privacy concerns or the scale of the campaign.

```text
Escalate to incident response or incident command if:
- Credential submission is confirmed or strongly suspected
- A privileged or sensitive account is affected
- Payload execution is observed
- Malware is detected on an endpoint
- Lateral movement is suspected
- Data access or exfiltration is suspected
- Internal account compromise is suspected
- Many users received or interacted with the email
- Business impact is reported
- Legal, privacy or regulatory concerns may apply
```

Escalation should follow the incident escalation SOP. If the analyst is uncertain whether the case meets the escalation threshold, the uncertainty should be documented and discussed with the appropriate SOC lead, incident manager or escalation point.

### Communication

Communication should be factual, short and appropriate to the audience. The analyst should avoid creating panic, overstating compromise or sending technical details to users who only need clear instructions.

Example user guidance:

```text
Do not click any links or open attachments from the reported email.
If you already interacted with the email, contact the SOC immediately and do
not delete the message.
```

A SOC-facing update may contain more operational detail:

```text
Phishing email confirmed malicious.
Three users received the email.
One user clicked the link.
Credential submission has not yet been confirmed.
Mailbox purge and URL blocking are in progress.
Sign-in activity for the affected user is under review.
```

If the case becomes an incident, communication should move into the organisation’s incident communication process. The playbook should not replace a SITREP, incident bridge or incident management process.

### Documentation

The case record should explain how the email was reported or detected, who received it, whether any users interacted with it, how it was classified, what evidence was collected, which indicators were extracted, what actions were taken, which decisions were made and what uncertainty remains.

A useful case conclusion may look like this:

```text
The reported email was confirmed as credential phishing.
Three users received the email.
One user clicked the link.
Credential submission was not confirmed.
The email was removed from all affected mailboxes.
The destination domain was blocked.
Sign-in activity for the affected user is under review.
No incident escalation at this time unless suspicious account activity is confirmed.
```

This conclusion is useful because it explains the classification, scope, user interaction, containment actions, remaining uncertainty and escalation position. It does not claim more certainty than the evidence supports.

### Closure Criteria

The case may be closed when the email has been classified, relevant evidence has been preserved, affected users have been identified, user interaction has been reviewed, containment actions have been completed or deliberately rejected, escalation criteria have been evaluated and remaining uncertainty has been documented.

Closure does not mean that every possible question has been answered. Closure means the case has reached a justified operational decision.

```text
Closure checklist:
- Email classified
- Evidence preserved
- Affected users identified
- User interaction reviewed
- Containment actions completed or deliberately rejected
- Escalation criteria evaluated
- Remaining uncertainty documented
- Follow-up actions assigned an owner
```

If follow-up work remains, the case should not simply disappear into a closed ticket. Follow-up actions should have an owner, a due date where appropriate and a clear link back to the original case.

### Review and Improvement

After the case is closed, the team should consider whether the playbook or related documentation should be improved. A playbook should not be static. It should improve when incidents, alerts, hunts and analyst experience reveal better ways of working.

```text
Review questions:
- Did the playbook help the analyst move through the case?
- Were any steps unclear?
- Were required runbooks missing?
- Did the escalation criteria work?
- Did the detection logic identify the campaign early enough?
- Were users given clear instructions?
- Was any evidence difficult to collect?
- Should this case create a new detection, hunt or awareness update?
```

Playbooks can also generate detection and hunting requirements based on observed patterns. If the same scenario repeatedly depends on manual investigation, unclear evidence or missing telemetry, the playbook should feed improvement work back into detection engineering and threat hunting.

```text
Possible improvement outputs:
- Updated playbook
- Updated runbook
- New detection logic
- Tuned email security rule
- New hunt idea
- User awareness example
- Improved escalation criteria
- Updated blocklist or enrichment source
```

This feedback loop is important. A playbook should not only describe how the team responds today. It should also help the team discover where response, detection, telemetry or analyst guidance needs to improve.

## Common Failure Modes

Playbooks often fail in predictable ways.

| Failure mode       | Why it hurts                                                               |
| ------------------ | -------------------------------------------------------------------------- |
| Too generic        | The playbook does not help the analyst make decisions.                     |
| Too detailed       | It becomes a runbook and is hard to use during a live case.                |
| No decision points | Analysts do not know when to escalate or change response path.             |
| No linked runbooks | The playbook says what to do but not how to do it.                         |
| No linked SOPs     | The playbook drifts away from organisational requirements.                 |
| No ownership       | Nobody updates the playbook when tools or threats change.                  |
| No review loop     | Lessons from incidents and hunts never improve the playbook.               |
| Too rigid          | Analysts follow the document even when evidence suggests a different path. |

A playbook should guide the analyst. It should not trap the analyst.

The most dangerous playbooks are often the ones that look complete but fail during real work. A playbook may have many pages, diagrams and checklists, but still be weak if it does not help the analyst decide what to do next. Another playbook may be shorter but much more useful because it clearly defines scope, evidence, decision points and escalation criteria.

## Working Position for This Book

For this book, a playbook is treated as scenario-based operational guidance. It helps the team handle a recognisable situation without pretending that every case will unfold the same way.

A good playbook connects observations to decisions. It helps the analyst move from alert, report or hunt finding to structured response. It should preserve lessons from previous cases and make them usable for future analysts.

The practical standard is simple:

```text
Can an analyst use this playbook to understand the scenario, make the right decisions and find the right task instructions?
```

If the answer is yes, the playbook is doing its job.

## Resources

* [CISA: Traffic Light Protocol Definitions and Usage](https://www.cisa.gov/news-events/news/traffic-light-protocol-tlp-definitions-and-usage)
* [PAP: Permissible Actions Protocol](https://cert.ssi.gouv.fr/csirt/sharing-policy/)
* [MITRE ATT&CK: Phishing](https://attack.mitre.org/techniques/T1566/)
* [MITRE ATT&CK: Initial Access](https://attack.mitre.org/tactics/TA0001/)
* [ATC RE&CT Framework](https://atc-project.github.io/atc-react/)
* [The RE&CT Framework](https://github.com/atc-project/atc-react)

## Revision

| Revised Date | Comment                                                                                                                                                   |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-13   | Major rewrite. Reframed the page as an explanation of playbooks as scenario-based guidance, with a complete phishing response playbook reference example. |
| 2024-10-26   | Page added                                                                                                                                                |
