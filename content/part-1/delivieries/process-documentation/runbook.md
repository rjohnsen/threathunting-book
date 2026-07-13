---

title: "Runbook"
description: "A practical explanation of security runbooks, how they support repeatable task execution, and how they differ from SOPs and playbooks."
date: 2024-10-19T18:13:15+02:00
lastmod: 2026-07-13
draft: false
hidden: false
weight: 3
tags:
   - runbook
   - process documentation
   - SOC
   - threat hunting
   - incident response
   - task execution
keywords:
   - runbook
   - security runbook
   - SOC runbook
   - incident response runbook
   - threat hunting runbook
   - SOP
   - playbook
   - process documentation
   - task execution
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**A runbook is a repeatable execution guide for a specific operational task. In security operations, many tasks need to be performed the same way regardless of who is on shift. Analysts may need to collect email headers, remove malicious messages from mailboxes, revoke user sessions, isolate an endpoint, export sign-in logs, block a domain, collect endpoint artefacts or run a specific hunt query.**

These tasks are often part of a larger investigation or response. A playbook explains the scenario and the response path. A runbook explains how one task in that path is performed. If the task cannot be performed consistently, the output cannot be trusted.

That is the reason runbooks exist. They reduce execution variation, preserve operational knowledge and make technical work easier to review, repeat, automate and hand over. A runbook should not make the analyst guess where to go, what to collect, what to record or how to prove that the task was completed correctly.

At its simplest:

```text
A runbook explains how to perform a task.
```

It is not responsible for the whole scenario. It is responsible for correct execution.

## What a Runbook Is

A runbook is a task manual.

It describes how to perform a defined operational activity from start to finish. The task may be manual, automated or partly automated, but the runbook should still explain the required input, procedure, expected output, validation checks and failure handling.

A usable runbook may describe:

* required access
* required tools
* required input
* safety considerations
* step-by-step procedure
* commands, queries or tool actions
* evidence to preserve
* output to record
* validation checks
* failure conditions
* escalation points
* related playbooks
* related SOPs

The important word is **defined**.

A runbook should not say:

```text
Investigate the suspicious email.
```

That is too broad. It describes intent, not execution.

A better runbook task would be:

```text
Collect and analyse email headers from a reported suspicious email.
```

That is narrow enough to perform, validate and document.

## What a Runbook Is Not

* A runbook is not a scenario guide. That is usually a playbook.
* A runbook is not a policy or formal operating standard. That is usually an SOP.
* A runbook is not a collection of good advice. It must be executable.

Many weak runbooks are actually vague instructions with a title. They tell the analyst what should happen, but not how to do it. A runbook should remove that uncertainty.

| Weak instruction             | Better runbook task                                    |
| ---------------------------- | ------------------------------------------------------ |
| Investigate suspicious email | Collect and analyse email headers                      |
| Check user activity          | Export and review sign-in activity for a user          |
| Contain endpoint             | Isolate endpoint in EDR                                |
| Review mailbox               | Check mailbox forwarding and inbox rules               |
| Block indicator              | Add domain to approved blocklist mechanism             |
| Look for related activity    | Search for similar messages by sender, subject and URL |

The runbook should be narrow enough that an analyst can complete it and produce a recognisable output.

## Runbook, Playbook and SOP

SOPs, playbooks and runbooks often work together, but they operate at different levels.

| Document type | Main concern           | Example                    |
| ------------- | ---------------------- | -------------------------- |
| SOP           | Required standard      | Evidence handling SOP      |
| Playbook      | Scenario response path | Phishing response playbook |
| Runbook       | Task execution         | Collect email headers      |

A phishing response playbook may reference several runbooks:

* collect email headers
* extract URLs from a reported email
* search for similar messages
* remove malicious email from mailboxes
* reset user password
* revoke user sessions
* review mailbox rules
* check sign-in activity
* block a domain or URL

The playbook tells the analyst when and why these tasks may be needed. The runbooks explain how to perform them. The SOP defines the organisational requirements that govern the work, such as evidence handling, escalation, approval and communication.

A simple separation is:

```text
SOP      → the required standard
Playbook → the scenario response path
Runbook  → the execution procedure
```

When this separation is lost, documentation becomes difficult to use. A playbook with every click and command becomes too heavy. A runbook that explains the whole incident becomes too broad. An SOP full of tool-specific steps becomes brittle.

## What Makes a Runbook Reliable

A reliable runbook is specific, repeatable and testable. The analyst should know what the task requires, how to perform it, what output to produce and how to verify that the task was completed correctly. This does not remove judgement from the analyst. It removes unnecessary uncertainty from execution.

| Quality        | Why it matters                                                            |
| -------------- | ------------------------------------------------------------------------- |
| Task-specific  | The runbook covers one defined operational task.                          |
| Executable     | The analyst can perform the task without interpreting vague instructions. |
| Repeatable     | Different analysts can produce comparable results.                        |
| Input-aware    | The required starting information is clear.                               |
| Access-aware   | Required permissions and tools are known before the task starts.          |
| Evidence-aware | The runbook explains what should be captured or preserved.                |
| Validated      | The runbook includes checks that confirm completion.                      |
| Failure-aware  | The runbook explains what to do when the expected path fails.             |
| Reviewable     | The output can be checked by another analyst.                             |
| Maintained     | The runbook is updated when tools, permissions or processes change.       |

Runbooks also support quality control. When the expected steps and outputs are defined, another analyst can review whether the work was performed correctly. That matters for peer review, training, handover, incident retrospectives and audits.

For execution work, the standard is not only whether the analyst did something. The standard is whether the analyst did the right thing, recorded the right output and left enough evidence for someone else to understand the result.

## Runbook Structure

A runbook should be structured around execution. The exact format depends on the task and environment, but the structure should make the task easy to perform and easy to verify.

```text
Title:
[Task name]

Task:
[The specific task this runbook performs]

Input:
[What the analyst needs before starting]

Output:
[What the runbook should produce]

Audience:
[Who the runbook is written for]

Prerequisites:
[Access, tools, permissions and conditions required before starting]

Safety considerations:
[Risks, approvals, impact or actions that require care]

Procedure:
[Step-by-step execution instructions]

Validation:
[How to confirm that the task was completed correctly]

Failure handling:
[What to do if the task cannot be completed]

Escalation:
[When and where to escalate]

Documentation:
[What must be recorded in the case or ticket]

Related playbooks:
[Scenario-based playbooks that may call this runbook]

Related SOPs:
[Standard operating procedures that govern the task]

Review:
[When the runbook should be reviewed or updated]
```

The structure should not become bureaucracy. A short, accurate runbook is better than a long document nobody trusts during real work.

## Reference Runbook: Email Header Collection and Analysis

The reference below shows a task-focused runbook that may be used inside a phishing response process.

It is intentionally narrower than a phishing playbook. It does not explain how to handle the full phishing case. It explains one task: collecting and analysing email headers so the analyst can support classification, investigation and response decisions.

### Task Definition

| Field          | Value                                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------ |
| Runbook        | Email Header Collection and Analysis                                                                         |
| ID             | RB-000001                                                                                                    |
| Task           | Collect, review and document email header information from a suspicious email.                               |
| Input          | Reported suspicious email, message ID or reliable message copy.                                              |
| Output         | Header analysis summary with findings, limitations and recommended next action.                              |
| Audience       | SOC analysts and Tier 2 analysts.                                                                            |
| Used by        | Phishing Email Response Playbook; Suspected Account Compromise Playbook; Business Email Compromise Playbook. |
| Related SOPs   | Evidence Handling SOP; Incident Escalation SOP; Case Documentation SOP.                                      |
| Severity       | The task itself has no severity. Severity is determined by the related case or playbook.                     |
| ATT&CK mapping | TA0001 Initial Access; T1566 Phishing; T1566.001 Spearphishing Attachment; T1566.002 Spearphishing Link.     |

### Required Input

Do not start the runbook until the minimum input is available or the limitation has been documented.

```text
Minimum input:
- Reported email or message ID
- Recipient mailbox
- Reporter name or case reference
- Approximate delivery time
- Existing alert or ticket reference
- Related playbook or investigation context
```

Screenshots, forwarded messages or copied text may be useful, but they are weaker than the original message. Headers and attachments may be incomplete or modified. If the original email is unavailable, record that limitation before continuing.

### Required Access

| Requirement        | Description                                                                     |
| ------------------ | ------------------------------------------------------------------------------- |
| Message access     | Access to the reported message, message ID or reliable copy.                    |
| Mailbox access     | Access to the mailbox, email investigation portal or phishing reporting system. |
| Gateway access     | Access to email security gateway or message trace data, where available.        |
| Case access        | Access to the incident, alert or ticket record.                                 |
| Evidence location  | Approved location for storing exported messages or header output.               |
| Reputation sources | Approved tools for checking sender IPs, domains and related artefacts.          |

If the analyst does not have the required access, do not improvise with weaker evidence unless the limitation is clearly recorded.

### Safety Rules

Header collection should not require the analyst to click links, open attachments or interact with external content.

```text
Safety rules:
- Do not click links in the suspicious email.
- Do not open attachments directly on a normal workstation.
- Do not forward the message outside approved channels.
- Preserve the original email where possible.
- Record where the email was collected from.
- Record if the message was forwarded, exported or reconstructed.
```

If links or attachments must be analysed, use the appropriate analysis runbooks and approved environments. If the case may become an incident, evidence handling must follow the relevant SOP.

## Procedure

The procedure below describes a repeatable approach. Tool-specific instructions should be adapted for the organisation’s email platform, email security gateway and investigation tooling.

### Step 1: Locate the Message

Find the reported message in the relevant system. This may be the user mailbox, phishing reporting mailbox, email security gateway, security portal or case management attachment.

Record:

```text
- Source system
- Mailbox or portal used
- Message ID if available
- Case or ticket reference
- Analyst performing the task
- Time of collection
```

If the message cannot be found, check whether it was deleted, quarantined, automatically remediated, moved by a mailbox rule or reported as an attachment rather than as the original message.

### Step 2: Preserve the Original Message

Preserve the original message before making changes. The exact method depends on the platform, but the goal is to retain a reliable copy that can be reviewed later.

Possible preservation methods:

```text
- Export original message
- Save message as .eml or .msg
- Preserve message in case management system
- Retain gateway copy
- Attach original message to incident record
- Store in approved evidence location
```

Record:

```text
- Preservation method
- Evidence location
- File name or reference
- Hash value if calculated
- Any limitations or modifications
```

If only a forwarded copy is available, document that the headers may not represent the original delivery path.

### Step 3: Extract Full Headers

Extract the full message headers from the original message or investigation portal. Do not rely only on visible sender information in the email client.

Capture at minimum:

```text
- From
- Reply-To
- Return-Path
- Sender
- Message-ID
- Date
- Subject
- Received headers
- Authentication-Results
- SPF result
- DKIM result
- DMARC result
- Source IP address if available
- Sending domain
- Envelope sender
- Mail client or sending infrastructure if visible
```

If some fields are missing, document the limitation.

### Step 4: Review Sender Identity

Compare visible sender information with technical sender information. Phishing often relies on a mismatch between what the user sees and what the message headers reveal.

Review:

```text
- Visible display name
- Visible sender address
- Reply-To address
- Return-Path
- Sending domain
- Source IP address
- Message-ID domain
- Similarity to trusted domains
- Unusual or newly registered domains
- Internal versus external origin
```

Answer:

```text
- Does the display name impersonate a known person or brand?
- Does the visible sender match the return-path?
- Does the reply-to address differ from the sender?
- Does the sending domain resemble a trusted domain?
- Does the message claim to be internal while originating externally?
```

A mismatch does not automatically prove malicious intent, but it may increase suspicion and should be considered with the rest of the evidence.

### Step 5: Review Authentication Results

Review SPF, DKIM and DMARC results. These results help determine whether the message passed expected email authentication checks, but they should not be treated as a complete verdict.

Check:

```text
- Did SPF pass, fail, softfail or return neutral?
- Did DKIM pass or fail?
- Did DMARC pass or fail?
- Which domain was authenticated?
- Does the authenticated domain match the visible sender domain?
- Are there alignment issues?
```

A message may pass SPF or DKIM and still be malicious, especially if sent from a compromised legitimate service or a lookalike domain. A failed authentication result may support suspicion, but it should be interpreted in context.

### Step 6: Review Routing and Received Headers

Review the `Received` headers to understand how the message moved between mail systems. This may help identify the source infrastructure, unexpected relays or signs of spoofing.

Look for:

```text
- First untrusted sending host
- Unusual relay path
- Mismatch between claimed sender and sending infrastructure
- Suspicious source IP address
- Unexpected geography if available
- Use of bulk mail providers
- Use of compromised or abused legitimate infrastructure
```

Answer:

```text
- What appears to be the first external sender?
- Does the route match the claimed sender?
- Are there unexpected relays?
- Does the source infrastructure belong to a known provider?
- Is the source IP or domain known in threat intelligence sources?
```

Received headers can be complex and may vary between platforms. If the analyst is uncertain, document the result as uncertain rather than forcing a conclusion.

### Step 7: Enrich Header Artefacts

Enrich relevant artefacts using approved tools. This may include reputation checks, passive DNS, WHOIS, threat intelligence platforms, email security logs or internal telemetry.

Artefacts for enrichment:

```text
- Sending IP address
- Sending domain
- Reply-to domain
- Return-path domain
- Message-ID domain
- URLs extracted from the message
- Attachment hashes if already available from approved tooling
```

Do not rely on a single reputation result. Reputation sources may disagree, may lack context or may not have seen the artefact before.

### Step 8: Record Findings

Record the findings in the case or ticket. Separate facts, interpretation and uncertainty.

Example:

```text
Header analysis summary:
- Message was reported by one user on 2026-07-13.
- Visible sender impersonated the internal finance department.
- Return-path used an external domain not associated with the organisation.
- Reply-to address differed from visible sender.
- SPF passed for the external sending domain.
- DMARC did not align with the visible sender domain.
- Received headers indicate delivery from external mail infrastructure.
- Header analysis supports suspicious classification.
- Full classification requires URL and user interaction analysis.
```

This summary is useful because it explains what was observed, what the evidence supports and what still needs to be tested.

### Step 9: Determine Next Action

Header analysis rarely closes the full case by itself. Determine which follow-up tasks are needed based on the findings.

| Condition                                                                    | Next action                                                       |
| ---------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Header artefacts are assessed as benign and no other suspicious traits exist | Document result and return to playbook decision point.            |
| Sender identity appears spoofed                                              | Continue phishing analysis and review user interaction.           |
| Reply-to address differs from sender                                         | Review message body, links and social engineering theme.          |
| Authentication fails or alignment is suspicious                              | Continue investigation and consider broader search.               |
| Sending domain or IP has poor reputation                                     | Enrich indicators and search for related messages.                |
| Evidence suggests credential phishing                                        | Follow phishing playbook containment and escalation criteria.     |
| Evidence is inconclusive                                                     | Document uncertainty and continue with related analysis runbooks. |

Do not decide the entire case based only on headers unless the playbook and available evidence support that decision.

## Validation

Validation is mandatory. The task is not complete until the expected output exists and the analyst can show how it was produced.

```text
Validation checklist:
- Original message or best available copy located
- Evidence preservation method documented
- Full headers extracted
- Sender identity reviewed
- Authentication results reviewed
- Routing reviewed
- Relevant artefacts enriched
- Findings recorded in the case
- Limitations documented
- Recommended next action recorded
```

If any of these steps cannot be completed, record the reason and decide whether escalation or another data source is required.

## Expected Output

The expected output is a short, evidence-based summary that can be used by the analyst, SOC lead or incident responder to understand what the header analysis showed.

A good output should include:

```text
- Source of the message
- Sender and reply-to assessment
- Authentication results
- Routing observations
- Enriched artefacts
- Confidence level
- Limitations
- Recommended next step
```

Example output:

```text
Email header analysis completed.

The message was reported by one user and preserved from the phishing reporting
mailbox. The visible sender impersonated the internal finance department, but
the return-path and reply-to address used an external domain. SPF passed for the
external sending domain, but DMARC alignment with the visible sender domain was
not present. Received headers indicate delivery from external mail
infrastructure.

Header analysis supports suspicious classification. URL analysis and user
interaction review are required before final classification and containment
decision.
```

## Failure Handling

Runbooks must handle failure because operational work rarely follows the happy path. Missing access, incomplete evidence, deleted messages and unclear logs should not cause the analyst to improvise silently.

| Failure condition                  | Handling                                                     |
| ---------------------------------- | ------------------------------------------------------------ |
| Original email unavailable         | Document limitation and analyse best available copy.         |
| Headers incomplete                 | Document missing fields and use gateway logs if available.   |
| Message already purged             | Review quarantine, gateway logs or case attachments.         |
| User forwarded message incorrectly | Request original report or collect from mailbox if possible. |
| Authentication results missing     | Use platform logs or gateway metadata if available.          |
| Analyst cannot interpret routing   | Escalate to senior analyst or email platform owner.          |

A failed or incomplete runbook execution is still useful when the limitation is documented. It may reveal a process gap, tooling issue or evidence preservation problem.

## Escalation

Escalate when the task cannot be completed, when the findings suggest higher risk, or when required evidence is unavailable.

Escalate to the SOC lead, senior analyst, incident responder or email platform owner if:

```text
- The original email cannot be preserved
- Multiple users received the same suspicious email
- A privileged or sensitive user is involved
- Sender information suggests internal account compromise
- Authentication and routing indicate likely spoofing of a trusted domain
- Header analysis suggests a broader phishing campaign
- Evidence collection requires elevated access
- The analyst is uncertain how to interpret critical header fields
```

Escalation should follow the relevant SOP and playbook.

## Documentation

The case record should contain enough detail that another analyst can understand what was collected, what was observed, what was uncertain and what should happen next.

Record:

```text
- Runbook name and version
- Analyst name or identifier
- Date and time of execution
- Message source
- Message ID if available
- Sender, reply-to and return-path
- Authentication results
- Routing observations
- Artefacts enriched
- Findings
- Limitations
- Recommended next action
- Related playbook decision point
```

Documentation should be factual and concise. It should not overstate compromise or classification beyond what the evidence supports.

## Quality Control

Runbooks create a baseline for execution. That also makes them useful for quality control.

A reviewer can compare the case record against the runbook and ask whether the required input was present, whether the correct steps were followed, whether limitations were documented and whether the output supports the next operational decision.

A useful quality control review may ask:

```text
- Was the correct runbook used?
- Were the prerequisites met?
- Were required fields collected?
- Were limitations documented?
- Was the output clear enough to support the playbook decision?
- Did the analyst overstate or understate the findings?
- Were escalation criteria considered?
- Are follow-up actions clear?
```

This is especially important for training, handover, incident review and auditability. A runbook is not only a guide for the person doing the task. It is also a reference for the person reviewing the task.

## Review and Maintenance

A runbook should be reviewed when the task, tooling, permissions, evidence requirements or related playbooks change. It should also be reviewed when analysts repeatedly fail to complete the task or when real cases show that the output is not good enough.

Review questions:

```text
- Can analysts follow the runbook without guessing?
- Are the required tools and permissions still correct?
- Are the required fields still available in the current platform?
- Are examples clear enough for junior analysts?
- Are escalation conditions correct?
- Does the output support the related playbook?
- Are there steps that should be automated?
- Are there recurring failures that indicate a tooling or process gap?
```

Possible improvement outputs:

```text
- Updated runbook
- Improved analyst examples
- New screenshots or tool guidance
- Automation for header extraction
- Detection or enrichment improvements
- Updated evidence handling guidance
- Updated phishing playbook links
```

A runbook that is not maintained becomes operational folklore with a document title. It may still look official, but analysts will stop trusting it once it no longer matches the work.

## Automation and Runbooks

Runbooks are often good candidates for automation. If a task is frequent, repetitive, well understood and low-risk, part or all of it may be automated.

Examples include:

* extracting message headers
* parsing URLs
* enriching domains and IP addresses
* searching for similar messages
* collecting endpoint metadata
* exporting sign-in logs
* revoking sessions after approval
* creating case notes
* opening related tickets

Automation does not remove the need for runbooks. In many cases, the runbook becomes the specification for automation. It explains what the automation should do, what input it needs, what output it should produce, what errors it must handle and when a human should take over.

A useful rule is:

```text
Manual runbook first.
Automate only when the task is understood.
Keep the runbook as the control and review point.
```

Automating an unclear process does not create maturity. It only makes poor execution faster.

## Common Failure Modes

Runbooks usually fail at the point of execution.

They may look complete in a documentation portal, but fail when an analyst tries to use them during real work.

| Failure mode                          | Why it hurts                                                         |
| ------------------------------------- | -------------------------------------------------------------------- |
| Too broad                             | The runbook becomes a playbook and loses task clarity.               |
| Too vague                             | Analysts still have to guess how to perform the task.                |
| Too tool-specific without maintenance | The runbook breaks when the tool changes.                            |
| No prerequisites                      | Analysts start the task without the required access or input.        |
| No validation step                    | The analyst cannot confirm whether the task was completed correctly. |
| No failure handling                   | Analysts do not know what to do when the expected path fails.        |
| No ownership                          | Nobody updates the runbook when processes or tools change.           |
| No link to playbooks                  | Analysts do not know when or why to use the runbook.                 |
| No evidence guidance                  | Important artefacts may not be preserved.                            |

The most common problem is not that runbooks are missing. The most common problem is that runbooks are written once and then trusted for too long.

A stale runbook can be worse than no runbook because it gives analysts confidence in steps that no longer work.

## Working Position for This Book

For this book, a runbook is treated as an execution artefact. It should be narrow, practical and verifiable. It should define the task, the input, the procedure, the expected output and the validation checks. It should also explain what to do when the task cannot be completed as expected.

A runbook earns its place in operations when it helps a trained analyst perform the task correctly, prove that it was completed and leave behind a record another analyst can understand.

That is the standard. Not because every task is complex, but because operational work must be repeatable, reviewable and safe to hand over.

## Resources

* [What is a Runbook? - PagerDuty](https://www.pagerduty.com/resources/learn/what-is-a-runbook/)
* [Runbook vs. Playbook, SOP, User Guide - Scribe](https://scribehow.com/library/runbook-vs-playbook)
* [How To Create A Runbook For SOC: Practical Guide](https://www.neumetric.com/journal/how-to-create-a-runbook-for-soc-practical-guide/)
* [ATC RE&CT Framework](https://atc-project.github.io/atc-react/)
* [The RE&CT Framework](https://github.com/atc-project/atc-react)
* [MITRE ATT&CK: Phishing](https://attack.mitre.org/techniques/T1566/)

## Revision

| Revised Date | Comment                                                                                                                                                             |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-13   | Major rewrite. Reframed the page as an explanation of runbooks as task-focused execution artefacts, with an email header collection and analysis reference runbook. |
| 2024-10-26   | Page added                                                                                                                                                          |
