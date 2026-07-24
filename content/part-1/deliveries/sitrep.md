---

title: "SITREP"
description: "A practical introduction to Situation Reports, how they support threat hunting, SOC operations and incident management, and how to structure useful operational updates."
date: 2024-10-13T09:09:44+02:00
lastmod: 2026-07-13
draft: false
hidden: false
weight: 5
tags:
    - sitrep
    - situation report
    - threat hunting
    - SOC
    - incident management
    - communication
keywords:
    - SITREP
    - situation report
    - PROGREP
    - threat hunting communication
    - SOC escalation
    - incident management
    - operational communication
    - handover
    - status report
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**A SITREP, or Situation Report, is a structured operational update. It explains what is happening, what is known, what has been done and what should happen next. In threat hunting, SOC operations and incident management, communication is often just as important as technical analysis. A strong analyst may understand the situation, but if that understanding is not communicated clearly, the wider team may still be blind.**

A SITREP helps solve that problem. It provides a concise operational snapshot for people who need to understand the current situation without reading every log entry, case note, chat message or investigation thread. It helps analysts, managers, incident commanders and stakeholders align on the current state of an investigation or incident.

A good SITREP should answer a few practical questions:

* What is happening?
* What do we know?
* What have we done?
* What is the current impact or risk?
* What happens next?
* Who owns the next action?

The goal is not to produce beautiful prose. The goal is shared situational awareness.

## What a SITREP Is

A SITREP gives a time-bound operational snapshot of an ongoing situation. The term comes from military reporting, but the concept applies directly to cybersecurity operations. In a SOC, during an incident, or during a threat hunt, the team often needs a clear update that separates confirmed facts from assumptions, open questions and next steps.

A SITREP is time-bound. It reflects what is known at a specific point in time, not the final truth. As the situation develops, later SITREPs may confirm, correct or replace earlier assessments.

In active incidents, SITREPs should be updated at a defined cadence or whenever the situation materially changes. The cadence does not have to be complicated, but the team should know when the next update is expected.

A SITREP is useful when:

* the situation is ongoing
* several people need the same update
* management needs a clear status
* work is being handed over between analysts or shifts
* decisions must be made based on current information
* the investigation may later need to be reconstructed
* stakeholders need confidence that the situation is being managed

A SITREP should not be a data dump. It should not include every detail of the investigation. It should provide enough information for the recipient to understand the situation and decide what, if anything, needs to happen next.

## SITREP and PROGREP

A common mistake is to confuse a SITREP with a PROGREP. They are related, but they are not the same:

* A SITREP describes the current situation.
* A PROGREP describes progress towards a goal.

| Report type | Main question                                     | Typical use                                                                   |
| ----------- | ------------------------------------------------- | ----------------------------------------------------------------------------- |
| SITREP      | What is the situation right now?                  | Situational awareness, escalation, handover and decision support.             |
| PROGREP     | What progress has been made towards the objective? | Tracking milestones, remediation, investigation progress and task completion. |

In simple terms:

```text
SITREP first.
PROGREP follows.
```

When management asks for a status update during an incident, they usually need to understand the situation first. They need to know what is happening, what is affected, what the current risk is, and whether the situation is under control.

After that, they may ask about progress. That is where a PROGREP becomes useful. A practical rule of thumb is:

```text
SITREP: We currently have situation X.
PROGREP: We have completed steps A and B, and step C is still in progress.
```

Both reports are useful, but using the wrong one at the wrong time can create confusion. If people do not understand the situation, progress updates may not help.

## Why SITREPs Matter

During an incident or active investigation, communication can easily fragment. One analyst may have endpoint findings. Another may have identity logs. A manager may have stakeholder questions. The incident commander may be trying to coordinate legal, communications, infrastructure, application owners and external parties.

Without a structured update, the same questions repeat:

```text
What is going on?
Is this confirmed?
Who is affected?
What have we done?
What are we doing next?
Who needs to know?
```

A SITREP gives the team a shared reference point. A weak SITREP increases decision-making latency. People ask the same questions repeatedly, decisions are delayed and teams may act from different assumptions. A good SITREP reduces that latency by giving the right people the right operational picture at the right time.

It helps with:

| Need                  | How the SITREP helps                                                            |
| --------------------- | ------------------------------------------------------------------------------- |
| Situational awareness | Summarises the current state of the situation.                                  |
| Decision support      | Gives management or incident command enough information to decide next actions. |
| Handover              | Helps another analyst or shift understand the current status quickly.           |
| Timeline building     | Creates a written record of what was known at a specific time.                  |
| Escalation            | Gives the receiving party a clear summary instead of fragmented observations.   |
| Coordination          | Helps different teams work from the same understanding.                         |
| Accountability        | Clarifies actions taken, next steps and ownership.                              |

A good SITREP reduces noise. It does not remove uncertainty, but it makes uncertainty visible.

## What a Good SITREP Should Contain

There is no single golden SITREP template. The right structure depends on the situation, organisation, audience and operational tempo. A threat hunt update does not need the same format as a major ransomware incident. A SOC escalation does not need the same detail as an incident command update.

Still, most useful SITREPs include the same core elements.

| Field                  | Purpose                                                                             |
| ---------------------- | ----------------------------------------------------------------------------------- |
| Date and time          | Shows when the update was produced.                                                 |
| Prepared by            | Identifies who wrote or provided the update.                                        |
| Audience               | Clarifies who the update is written for and why the level of detail is appropriate. |
| Situation summary      | Gives a short explanation of what is happening.                                     |
| Status                 | Describes whether the situation is new, ongoing, contained, escalating or resolved. |
| Scope or affected area | Identifies affected systems, users, locations, business areas or data.              |
| Key observations       | Lists the most important facts or signals.                                          |
| Actions taken          | Summarises what has already been done.                                              |
| Current assessment     | Explains current risk, confidence or operational impact.                            |
| Open questions         | Shows what is still unknown.                                                        |
| Next steps             | Describes what happens next.                                                        |
| Owner                  | Clarifies who is responsible for the next action.                                   |
| Cadence                | States when the next update is expected, if the situation is active.                |

The SITREP should be clear about what is confirmed and what is suspected.

A weak SITREP may say:

```text
Customer data has been stolen.
```

A stronger SITREP may say:

```text
We have confirmed unauthorised access to the CRM database.
We have not yet confirmed whether customer data was exfiltrated.
Forensic review is ongoing.
```

The second version is more useful because it separates fact from uncertainty.

## Writing Style

A SITREP should be short, structured and plain. Avoid dramatic language, speculation and unnecessary technical depth. The recipient may not be a specialist, and even specialists need clarity during high-pressure situations.

Good SITREPs use:

* short sentences
* clear timestamps
* named owners
* explicit uncertainty
* concrete actions
* consistent structure
* plain operational language

Avoid vague phrases such as:

```text
We are looking into it.
Some suspicious activity was observed.
The issue may be related to several things.
Actions are being taken.
```

Prefer concrete wording:

```text
SOC is reviewing sign-in logs for the affected accounts.
EDR telemetry shows suspicious PowerShell execution on one workstation.
We have not yet confirmed lateral movement.
Infrastructure has isolated the affected host from the network.
```

The goal is not to impress the reader. The goal is to make the situation understandable.

## When to Use a SITREP

A SITREP is useful whenever the situation is important enough that others need a structured update.

Examples include:

* critical or high-priority SOC alerts
* ongoing threat hunts
* suspected compromise
* confirmed incidents
* ransomware or extortion cases
* data exposure or data exfiltration concerns
* suspicious identity activity
* major vulnerability exploitation
* red team or purple team activity
* shift handovers
* management briefings
* external stakeholder updates

Not every event needs a formal SITREP. A minor false positive does not need a full report. A short case note may be enough. But if the situation may require coordination, escalation or decision-making, a SITREP is usually worth writing.

## Examples

{{% notice info %}}
The examples below are not universal templates. They are realistic examples showing how a SITREP may look in different operational situations. Use the structure, tone and level of detail as inspiration, and adapt it to your own organisation.
{{% /notice %}}

The point of these examples is not to prescribe one perfect format. The point is to show the kind of operational clarity a SITREP should provide.

## Threat Hunting SITREP

A threat hunting SITREP is useful when a hunt is ongoing and others need to understand the current focus, early observations, current status and next steps.

In threat hunting, the SITREP can also help define the hunt before execution begins. The hunter can document the trigger, scope, objective and data sources before starting, and then update the status as findings develop.

Example:

```text
SITREP: Threat Hunt - Phishing Follow-Up

Date/Time:
2026-07-10 10:00

Audience:
SOC lead, hunt team and incident commander

Prepared by:
Team Corgi

Status:
Ongoing

Situation summary:
A phishing campaign was reported against selected users in the organisation.
The current hunt is assessing whether affected users clicked suspicious URLs,
submitted credentials or executed payloads from the phishing emails.

Trigger:
Internal phishing alerts and external intelligence about similar campaigns
targeting organisations in the same sector.

Scope:
- Users who received the phishing email
- Email telemetry from the last 72 hours
- Proxy logs for clicked URLs
- Endpoint telemetry for affected workstations
- Identity sign-in logs for affected accounts

Key observations:
- Several users received emails containing suspicious URLs.
- Three users clicked links associated with the campaign.
- No confirmed payload execution has been observed so far.
- One user entered credentials into a suspicious login page.
- No confirmed lateral movement has been observed.

Actions taken:
- Extracted URLs from reported emails.
- Checked proxy logs for user clicks.
- Reviewed endpoint telemetry for affected workstations.
- Started sign-in review for users who clicked links.
- Requested password reset and session revocation for one affected user.

Current assessment:
Suspicious user interaction is confirmed.
Compromise is not confirmed at this time.
The highest concern is possible credential theft for one user.

Open questions:
- Did the suspicious login page capture credentials?
- Were any sessions reused from unmanaged devices?
- Was any mailbox rule created after the user interaction?
- Did any attachment execute on the endpoint?

Next steps:
- Complete identity sign-in review for affected users.
- Review mailbox rules and OAuth consent activity.
- Continue endpoint review for suspicious process execution.
- Escalate if post-authentication activity is confirmed.

Owner:
SOC analyst on duty

Cadence:
Next update in two hours or earlier if compromise is confirmed.
```

Notice how this SITREP does not list every query or every log entry. It gives the reader the current operational picture: what started the hunt, what has been checked, what is known, what remains uncertain and what happens next.

## SOC Escalation SITREP

A SOC escalation SITREP is useful when an alert or case must be escalated to a SOC manager, incident commander or another responsible function. The purpose is to provide enough information to make a decision quickly.

Example:

```text
SITREP: SOC Escalation - Privileged Account Brute Force

Criticality:
Priority 1

Date/Time:
2026-07-10 10:00

Audience:
SOC manager and incident commander

Analyst:
Albus Corgi

Case:
INC-2026-00123

Status:
Escalated

Situation summary:
A privileged account was targeted by repeated failed login attempts from an
external IP address. One successful login was observed after the failed attempts.

Affected users or systems:
- One privileged user account
- Two externally reachable authentication endpoints

Key observations:
- Multiple failed login attempts were observed from 192.0.2.45.
- One successful login occurred from the same source after repeated failures.
- The successful login used a privileged account.
- The source IP is not associated with known corporate locations.
- Post-authentication activity is still under review.

Actions taken:
- Source IP blocked at perimeter controls.
- Password reset initiated for the affected account.
- Active sessions for the affected account revoked.
- Sign-in logs and audit activity are under review.

Current assessment:
Possible privileged account compromise (medium confidence).
The successful login is confirmed, but malicious use after login has not yet
been confirmed.

Escalation reason:
Privileged account involved.
Successful login observed after brute-force pattern.
Potential impact is high if the account was misused.

Open questions:
- Was MFA satisfied, bypassed or not required?
- Was the successful session used to access sensitive systems?
- Did the account perform any administrative action after login?
- Are other privileged accounts showing similar activity?

Next steps:
- Complete review of post-authentication activity.
- Confirm MFA status for the affected sign-in.
- Check related accounts for similar activity.
- Prepare incident escalation if malicious activity is confirmed.

Owner:
SOC lead

Cadence:
Next update within one hour or immediately if administrative misuse is confirmed.
```

This example shows the difference between alert detail and escalation detail. The manager does not need every failed login event. They need to understand why the case matters, what has been confirmed, what is still unknown and what decision may be needed.

## Incident Management SITREP

An incident management SITREP is useful during confirmed or suspected incidents where several teams or stakeholders need a shared operational update. This type of SITREP should be clear enough for incident command, technical teams and management.

Example:

```text
SITREP: Suspected CRM Data Exposure

Date/Time:
2026-07-10 08:30

Audience:
Incident command, legal, privacy and senior management

Incident Commander:
Albus Corgi

Prepared by:
SOC

Status:
Ongoing investigation

Severity:
High

Situation summary:
Unauthorised access to the CRM database has been confirmed.
Possible customer data exfiltration is under investigation.

Affected systems or data:
- CRM database
- CRM API endpoints
- Customer records may be affected

Confirmed facts:
- A compromised service account was used to access the CRM database.
- Access originated from an external IP address not associated with the company.
- The account accessed customer record tables.
- API access from the account has been disabled.
- The affected service account has been disabled.

Unconfirmed or under investigation:
- Whether customer records were exfiltrated.
- How the service account was compromised.
- Whether other service accounts were accessed.
- Whether the external IP is part of wider attacker infrastructure.

Actions taken:
- Disabled the compromised service account.
- Restricted CRM API access.
- Started forensic review of database and API logs.
- Notified legal and privacy stakeholders.
- Preserved relevant logs for investigation.

Current impact:
CRM access is limited for selected users while containment is ongoing.
No confirmed customer notification requirement at this time.
Regulatory notification assessment is pending.

Decisions needed:
- Whether to activate external incident response support.
- Whether to prepare a formal regulatory notification assessment.
- Whether to extend containment to related API integrations.

Next steps:
- Complete forensic review of CRM access logs.
- Validate whether bulk export or abnormal query volume occurred.
- Review other service accounts with CRM access.
- Prepare next SITREP by 12:00 or earlier if exfiltration is confirmed.

Owner:
Incident commander

Cadence:
Next scheduled SITREP at 12:00, unless exfiltration is confirmed earlier.
```

This example is careful with wording. It does not say that customer data was stolen. It says unauthorised access is confirmed, customer data may be affected and exfiltration is under investigation. That distinction matters.

## Incident Call-In Example

The first person receiving an incident call has an important role. If the receiver is unprepared, important information may be missed. That can slow down response, create confusion and force the team to call back for details that should have been captured immediately.

A call-in structure should therefore be easy to find and simple to use. It should be available in the SOC wiki, ticketing system, hotline procedure or printed as a fallback.

The purpose is not to perform the full investigation during the call. The purpose is to capture the right information so the issue can be triaged and escalated correctly.

Example:

```text
SITREP: Initial Call-In - Suspected Phishing and Credential Submission

Date/Time received:
2026-07-10 09:15

Audience:
SOC analyst receiving the case and next analyst handling triage

Call received by:
SOC hotline

Caller:
Business user from Finance

Contact details:
Phone number and email recorded in ticket INC-2026-00456

Reported issue:
Caller reports receiving a suspicious email that appeared to be from Microsoft.
The caller clicked the link and entered username and password on a login page.

Location:
Microsoft 365 / corporate workstation

Affected users or systems:
- One user account
- One corporate workstation
- Possible Microsoft 365 session exposure

When it started:
Email received at approximately 08:50.
Link clicked at approximately 09:05.
SOC contacted at 09:15.

What changed:
After entering credentials, the page showed an error message.
The user became suspicious and contacted SOC.
No business impact reported by caller.

Actions already taken:
- User disconnected from VPN.
- User stopped using the workstation.
- SOC ticket opened.
- Password reset requested.
- Session revocation requested.

Evidence available:
- Suspicious email still present in mailbox.
- URL copied into ticket.
- Screenshot of login page provided by user.

Current assessment:
Possible credential phishing.
Compromise is not confirmed.
Credential exposure is plausible because the user entered credentials into a
suspicious login page.

Immediate next steps:
- Reset password and revoke active sessions.
- Review sign-in logs for the affected account.
- Inspect email headers and URL.
- Check whether other users received the same message.
- Review endpoint activity if payload delivery is suspected.

Owner:
SOC analyst on duty

Cadence:
Update the case after password reset, session revocation and sign-in review are complete.
```

This is not a full incident report. It is the first operational snapshot created from the call. Its value is that the next analyst does not have to start from scratch.

## Practical Advice

A SITREP should be useful under pressure. That means the structure must be available before the incident starts. Do not wait until a major incident to decide what a status update should contain.

A few practical recommendations:

* Keep examples and structures in a place analysts can find quickly.
* Use the same field names across SOC, incident and hunting updates where possible.
* Separate confirmed facts from assumptions.
* Include timestamps.
* State the intended audience.
* Define the update cadence during active incidents.
* Include owners for next steps.
* Keep the language plain.
* Update the SITREP when the situation changes.
* Store SITREPs with the case or incident record.
* Use the SITREP as a handover artefact between shifts or teams.

A good SITREP should reduce the number of repeated questions. If everyone still asks what is happening, who is affected and what happens next, the SITREP is probably not doing its job.

## Working Position for This Book

For this book, a SITREP is treated as an operational communication tool. It is not only documentation. It is a way to create shared understanding during uncertainty.

A good SITREP should help people understand the situation, make decisions, coordinate actions and preserve a timeline of what was known at a given point in time.

The practical standard is simple:

```text
Can someone read this and understand the current situation well enough to make or support the next decision?
```

If the answer is yes, the SITREP is doing its job.

## Resources

* [Merriam-Webster: SITREP Definition](https://www.merriam-webster.com/dictionary/sitrep)
* [Ready.gov: Crisis Communications Plan](https://www.ready.gov/business/implementation/crisis)
* [CARE Emergency Toolkit: Situation Reports](https://www.careemergencytoolkit.org/meal/42-information-management/4-situation-reports-sitreps/)
* [ProjectManagement.com: SITREP – A Necessary Tool in Project Management](https://www.projectmanagement.com/deliverables/5314/SITREP)
* [Persimmon Group: Situation Report Template](https://thepersimmongroup.com/situation-report-sitrep-template/)

## Revision

| Revised Date | Comment                                                                                                                                                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide to SITREPs with realistic examples, audience awareness and update cadence for threat hunting, SOC escalation, incident management and incident call-ins. |
| 2024-10-13   | Page added                                                                                                                                                                                                        |
