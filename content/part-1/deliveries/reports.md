---
title: "Reports"
description: "A practical explanation of reports as threat hunting deliverables, including different report types, audiences, levels of detail, tools, and how reports preserve and communicate operational outcomes."
date: 2026-07-13T00:00:00+02:00
lastmod: 2026-07-14
draft: false
hidden: false
weight: 6
tags:
    - reports
    - threat hunting
    - SOC
    - incident response
    - detection engineering
    - process documentation
    - communication
keywords:
    - reports
    - threat hunting reports
    - hunt report
    - investigation report
    - findings report
    - executive report
    - technical report
    - SOC reporting
    - threat hunting deliverables
    - security reporting
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**A report is a structured way to preserve and communicate the outcome of security work. In threat hunting, reporting is often misunderstood. Some people think of a report as a long PDF written at the end of a major investigation. That can be true, but it is only one version of reporting.**

A report may be a short case summary, a hunt report, a technical appendix, a detection recommendation, a baseline summary, an audit report, an executive briefing, a findings note or a formal incident report.

Some reports are long. Some are short. Some are written for analysts. Some are written for managers. Some are written for detection engineers, incident responders, system owners, compliance teams or external stakeholders.

The format matters less than the function. A report should help the right audience understand what was done, what was found, what it means and what should happen next.

At its simplest:

```text
A report turns security work into documented outcome.
```

A report should support a decision, action, review or shared understanding. If nobody can use the report to understand, decide or act, the report has not done its job.

A simple rule of thumb is:

```text
No decision, no report.
```

That does not mean every report must lead to a major decision. The decision may be to close a case, create a detection candidate, update a baseline, escalate an issue, accept a limitation, assign remediation, start an audit or preserve knowledge for later review. The point is that the report should have a purpose beyond existing as a document.

Without reporting, valuable work may disappear into chat messages, notebooks, queries, screenshots or individual memory. The team may have done good work, but the organisation may not be able to reuse it, review it or act on it.

## What a Report Is

A report is a documented outcome. It explains the result of a hunt, investigation, audit, review, baseline, detection analysis or other security activity. The report may be formal or informal, long or short, technical or non-technical.

A useful report may describe:

* purpose
* trigger
* scope
* audience
* method
* data sources
* observations
* findings
* interpretation
* impact
* confidence
* limitations
* recommendations
* next steps
* owner
* appendices
* revision history

The important word is **useful**. A report does not need to include everything the analyst looked at. It needs to include enough for the audience to understand the outcome and act appropriately.

A short report may be enough when the scope is narrow and the audience only needs a decision. A longer report may be needed when the work is complex, the evidence is detailed, the impact is high or the result must be reviewed later. The length should follow the need.

A report is not defined by its file format. It is defined by whether it preserves and communicates the outcome of the work.

## What a Report Is Not

* A report is not automatically valuable because it is long.
* A report is not automatically weak because it is short.
* A report is not a dumping ground for every query, screenshot, log entry or thought the analyst had during the work.
* A report should not hide uncertainty behind confident language. It should not exaggerate findings. It should not use technical detail as decoration. It should not force every audience to read the same level of detail.

For example, this is weak:

```text
We investigated suspicious activity and found several issues.
```

That does not tell the reader enough.

This is better:

```text
We investigated interactive logon behaviour for selected Finance-owned service
accounts. No confirmed account compromise was identified. However, interactive
logons from standard user workstations were not observed in the scoped baseline
and should be treated as suspicious unless explained by documented maintenance
activity.
```

The second version explains what was investigated, what was found, why it matters and what should happen next. A report should reduce uncertainty for the reader, not create more of it.

## Report, SITREP and Case Notes

Reports, SITREPs and case notes are related, but they do not serve the same purpose.

* A SITREP explains the current situation while work is ongoing.
* Case notes preserve the working record of actions, observations and decisions during the case.
* A report communicates the outcome, conclusion or structured result of the work.

| Document type | Main question                                              | Typical timing                        |
| ------------- | ---------------------------------------------------------- | ------------------------------------- |
| SITREP        | What is happening now?                                     | During active work                    |
| Case notes    | What did we observe and do?                                | Throughout the work                   |
| Report        | What was done, what was found and what should happen next? | After a defined activity or milestone |

A useful distinction is:

```text
SITREP is operational status.
Case notes are the working record.
Report is documented outcome.
```

A report may use information from SITREPs and case notes, but it should not simply paste them together. It should interpret the work and present the result in a form the audience can use.

## Why Reports Matter

Threat hunting should leave something behind. A hunt may find malicious activity. It may find suspicious behaviour. It may find nothing. It may produce a baseline, a detection candidate, a visibility gap, a process improvement or a new question.

If the result is not documented, much of the value may be lost.

Reports help the team preserve knowledge, support decisions, explain findings, communicate risk, hand over work, justify recommendations, support detection engineering, support incident response, support management decisions, support audits and retrospectives, and turn individual analysis into organisational learning.

A report is often the bridge between technical work and organisational action. Reports also expose whether the organisation is willing to act on what security work discovers.

A report that identifies a visibility gap, detection weakness, ownership problem or process failure should not disappear into a folder. If the same issue is found again three months later, the problem was not only technical. The organisation failed to turn the report into action.

That does not mean every report must create urgent remediation work. Some reports preserve knowledge, support closure or document why no further action is needed. But when a report recommends action, the action should have an owner, a decision or a deliberate acceptance of risk.

A report without follow-up can become a polished way to forget the same problem twice. The hunter may understand what was found, but the organisation needs to understand what it means.

## Reports Can Be Many Things

There is no single report format that fits all threat hunting work. Some reports are short and operational. Some are detailed and technical. Some are written for decision-makers. Some are written for people who need to reproduce the work.

The type of report should follow the purpose.

| Report type                 | Purpose                                                                          |
| --------------------------- | -------------------------------------------------------------------------------- |
| Hunt report                 | Document the purpose, scope, method, result and recommendations from a hunt.     |
| Investigation report        | Document evidence, analysis and conclusions from a security investigation.       |
| Findings report             | Summarise confirmed observations that require action.                            |
| No-result report            | Explain what was tested, what was not found and what the result means.           |
| Baseline report             | Document expected behaviour within a defined scope.                              |
| Detection recommendation    | Explain why a behaviour should or should not become a detection.                 |
| Detection validation report | Document whether detection logic works as intended.                              |
| Coverage report             | Show which behaviours, assets, techniques or data sources are covered.           |
| Audit report                | Review whether a detection, process, control or assumption still works.          |
| Executive report            | Communicate risk, impact, decisions and recommended actions to leadership.       |
| Technical appendix          | Preserve queries, evidence, artefacts, fields, screenshots or detailed analysis. |
| Lessons learned report      | Capture what should change after a hunt, incident or review.                     |

The report type should not be chosen because it sounds formal. It should be chosen because it fits the work and the audience.

* A short report may be the correct deliverable.
* A long report may be the correct deliverable.

The wrong report is the one nobody can use.

## Audience Matters

A report should be written for its audience. The same hunt may produce different reporting outputs for different readers. The SOC may need triage guidance. Detection engineering may need logic, assumptions and false positives. Management may need risk, impact and decisions. System owners may need concrete remediation actions.

| Audience                       | What they usually need                                                      |
| ------------------------------ | --------------------------------------------------------------------------- |
| SOC analysts                   | What happened, what to check, how to triage, when to escalate.              |
| Detection engineers            | Behaviour, logic, data source, assumptions, false positives and validation. |
| Incident responders            | Evidence, timeline, scope, impact and containment status.                   |
| System owners                  | Affected systems, required actions, urgency and business impact.            |
| Management                     | Risk, impact, decision points, ownership and progress.                      |
| GRC or compliance              | Control gaps, evidence, exceptions and accountability.                      |
| Threat intelligence            | Local observations, adversary behaviour and intelligence feedback.          |
| Architecture or platform teams | Capability gaps, telemetry gaps and design implications.                    |

A report fails when it gives the wrong audience the wrong level of detail. A manager does not need every query. A detection engineer may need the exact query. A SOC analyst may not need the full strategic context, but they need enough detail to triage the alert correctly.

The report should meet the reader where the decision is made.

## Length and Depth

Report length should be intentional. A report can be:

```text
Short:
A concise case conclusion or findings note.

Medium:
A structured hunt report with scope, method, findings and recommendations.

Long:
A full technical report with executive summary, evidence, timeline, analysis,
appendices and detailed recommendations.
```

The length should depend on complexity, impact, audience, evidence volume, uncertainty, need for review, regulatory or legal relevance, and whether the work must be reproduced later.

* A simple no-result hunt may only need a short report if the scope is clear and the conclusion is limited.
* A suspected compromise involving multiple systems may need a detailed report with timeline, evidence, technical analysis and recommendations.
* A detection recommendation may be short, but should still include behaviour, rationale, data source, false positives and validation needs.

The question is not:

```text
How long should the report be?
```

The better question is:

```text
What does the reader need in order to understand, decide or act?
```

That should drive the length.

## When Not to Write a Full Report

Not every outcome needs a full report. A full report is useful when the work is complex, the audience is broad, the impact is significant, the evidence must be reviewed later or the result needs formal ownership.

A full report may be the wrong format when:

* the audience only needs a short decision note
* the finding is already captured clearly in the case record
* urgent remediation would be delayed by report writing
* the technical detail belongs in an appendix or ticket
* the result is narrow enough for a short findings note
* the same information has already been communicated in a SITREP
* the report would exist mainly to look formal

For example, a minor telemetry gap may not need a ten-page report. It may need a clear finding, evidence, owner, priority and remediation ticket.

The report should fit the decision, not the analyst’s desire to document everything.

## Report Lifecycle

A report should have a lifecycle. A simple lifecycle is:

```text
Trigger → Audience → Scope → Evidence → Interpretation → Recommendation → Follow-up
```

The trigger explains why the report exists. The audience defines what level of detail is needed. The scope defines what the report can and cannot say. The evidence supports the conclusion. The interpretation explains what the evidence means. The recommendation defines what should happen next. Follow-up checks whether the report led to action, decision or preserved knowledge.

A report without follow-up may document work without changing anything.

That may be acceptable for some records, but it should be intentional. If the report recommends action, someone should own that action.

## Report Structure

A practical report structure should support clarity.

Not every report needs every section, but the following structure works well for many threat hunting and security operations reports.

```text
Title:
[Report name]

Summary:
[Short explanation of the outcome]

Purpose:
[Why the work was performed]

Trigger:
[What initiated the work]

Scope:
[What was included and excluded]

Audience:
[Who the report is written for]

Method:
[How the work was performed]

Data sources:
[Telemetry, tools, cases or evidence used]

Observations:
[What was seen]

Findings:
[What was concluded from the observations]

Impact:
[Why the findings matter]

Recommendations:
[What should happen next]

Limitations:
[What the report cannot prove or did not cover]

Appendices:
[Queries, evidence, artefacts or detailed analysis]

Revision:
[Date and change history]
```

A short report may only use some of these sections. A long report may expand them significantly.

The structure should serve the reader, not the other way around.

## Choosing the Reporting Tool

A report does not have to be a Word document converted to PDF. The tool should fit the purpose, audience, workflow and required output format. Some reports may be written in a ticketing system. Some may be produced as Markdown, AsciiDoc, HTML, PDF, slide decks, dashboards, notebooks or structured case records. Some may be generated from templates. Some may be partly automated.

The important question is not which tool looks most formal. The important question is:

```text
Can the right audience use the report?
```

A reporting tool should support the way the team works. Useful considerations include:

* readability
* portability
* version control
* review workflow
* export formats
* technical formatting
* code and query highlighting
* tables
* diagrams
* evidence handling
* appendices
* reusable templates
* automation
* long-term storage
* access control
* compatibility with the audience

Plain-text and markup-based reporting can be useful because the report source is portable. Markdown and AsciiDoc can be stored in Git, reviewed through normal change processes, published to an internal wiki, converted to other formats or reused as part of a knowledge base.

Tools such as Joplin and Obsidian can support this kind of workflow because Markdown is central to how they store and organise notes. They can be useful for collecting observations, drafting sections, linking related notes and preserving investigation context in a searchable form. In some teams, the same notes may be placed directly in Git, shared through a repository or dragged into a wiki as part of the handover.

AsciiDoc and Asciidoctor can be especially useful when the report needs richer structure and more controlled publishing. AsciiDoc supports features such as admonitions, syntax highlighting, tables, diagrams, includes, attributes and reusable templates. With the right toolchain, the same source can be converted into formats such as HTML, PDF, Word, slides, e-books or other publishable outputs.

For example, a reporting workflow may look like this:

```text
Markdown or AsciiDoc source
→ Git repository or knowledge base
→ review and update
→ HTML for internal documentation
→ PDF for formal distribution
→ Word document for stakeholder review
→ slide deck for management briefing
```

That does not mean Markdown, AsciiDoc, Joplin, Obsidian or any other tool is always the right choice. A short operational note may fit better in a case record. A management briefing may work better as slides. A detection handover may belong in a detection repository. An incident record may need to stay in the case management system.

The point is that reporting should be deliberate.

A report should not be forced into Word or PDF simply because that is what people expect a report to look like. The format should preserve the outcome, support review and make the result useful for the people who need to act on it.

## Observation, Interpretation, Impact and Recommendation

Many weak reports fail because they mix observation, interpretation and recommendation together.

A useful model is:

```text
Observation → Interpretation → Impact → Recommendation
```

This keeps the report clear.

For example:

```text
Observation:
A Finance-owned service account authenticated interactively from a standard user
workstation.

Interpretation:
This behaviour was not present in the scoped baseline and does not match the
documented application workflow.

Impact:
The activity may indicate credential misuse, human use of a service account or
undocumented maintenance activity.

Recommendation:
Review account ownership, source device, logon type, recent account changes and
related identity or endpoint alerts.
```

This is much better than:

```text
A service account logon looked suspicious and should be investigated.
```

The structured version explains why the observation matters and what should happen next. This model also helps prevent overstatement. If the interpretation is uncertain, the report can say so.

```text
Observation:
A service account authenticated from a new source host at 02:14.

Interpretation:
The activity does not match the documented baseline. However, the account owner
has not yet confirmed whether emergency maintenance was performed.

Impact:
Credential misuse cannot be confirmed based on currently available evidence,
but the activity should not be closed as expected until ownership and source
context are verified.

Recommendation:
Contact the application owner, review change records, check related identity and
endpoint alerts, and preserve authentication evidence before closure.
```

That is honest and useful.

## Reference Report: Threat Hunt Report

The reference below shows a practical threat hunt report. It is not intended to be the only format. It shows how a hunt outcome may be documented in a clear and reusable way.

### Report Definition

| Field              | Value                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------- |
| Report             | Threat Hunt Report                                                                                         |
| ID                 | REP-000001                                                                                                 |
| Hunt               | Service Account Interactive Logon Behaviour                                                                |
| Audience           | SOC analysts, identity administrators, detection engineers and security management.                        |
| Author             | Threat Hunting Team                                                                                        |
| Period reviewed    | 2026-06-01 to 2026-06-30                                                                                   |
| Data sources       | Authentication telemetry from identity provider, domain controllers and EDR where available.               |
| Related baseline   | Service Account Interactive Logon Behaviour.                                                               |
| Related detections | Service Account Interactive Logon from Workstation; Service Account Sign-In from Unmanaged Device.         |
| Related playbooks  | Suspicious Account Usage Playbook; Identity Compromise Playbook.                                           |
| Related runbooks   | Review Authentication Context; Review Account Ownership; Review Source Device; Collect Identity Artefacts. |

### Executive Summary

This hunt reviewed interactive logon behaviour for selected Finance-owned service accounts during June 2026.

The hunt did not identify confirmed account compromise. However, it confirmed that the scoped service accounts normally authenticate non-interactively from known application servers. Interactive logons from standard user workstations, VPN clients and unmanaged devices were not observed during the review period.

The result supports a detection candidate for service account interactive logons from unexpected sources and highlights the need to maintain service account ownership and authentication telemetry quality.

No immediate incident response action is required based on this hunt. Recommended follow-up actions are detection validation, baseline publication, service account ownership review and telemetry completeness review.

### Purpose

The purpose of the hunt was to understand whether selected Finance-owned service accounts were being used interactively from unexpected sources and whether local usage patterns could support detection engineering and SOC triage.

The hunt focused on behaviours commonly associated with credential misuse, shared account use, undocumented maintenance activity, application misconfiguration or early signs of identity compromise.

### Scope

Included:

```text
- Finance-owned service accounts used by business applications
- Authentication events from identity provider and domain controllers
- Source host and destination context where available
- Logon type where available
- Interactive and non-interactive authentication patterns
- Known maintenance activity
```

Excluded:

```text
- Human user accounts
- Privileged administrator accounts
- Disabled service accounts
- Application accounts owned by other business units
- Break-glass accounts
- Third-party SaaS accounts without sufficient telemetry
```

### Method

The hunt reviewed authentication telemetry for selected service accounts during the selected period.

The analysis grouped activity by:

```text
- account
- source host
- destination system
- logon type
- authentication method
- device context
- business owner
- application owner
- time of authentication
- known maintenance activity
```

The hunt looked specifically for:

```text
- interactive logon by service accounts
- remote interactive logon by service accounts
- authentication from standard user workstations
- authentication from unmanaged devices
- VPN sign-in by service accounts
- cloud portal sign-in by service accounts
- authentication from previously unseen source hosts
- activity outside known processing or maintenance windows
```

### Observations

Observed:

```text
- Most authentication activity was non-interactive.
- Authentication activity was primarily linked to known application servers.
- Repeated activity matched documented business application workflows.
- Two maintenance workflows explained rare interactive use from approved
  administrative hosts.
- No interactive logons from standard user workstations were observed.
- No VPN sign-ins by scoped service accounts were observed.
- No authentication from unmanaged devices was observed.
```

Limitations:

```text
- Logon type was not available for all authentication sources.
- Some SaaS authentication events lacked source device context.
- The 30-day period may not capture rare quarterly maintenance workflows.
- The hunt does not prove that account misuse is absent.
```

### Findings

| Finding                                                                                         | Assessment                                                             |
| ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Scoped service accounts normally authenticate non-interactively from known application servers. | Useful baseline observation.                                           |
| Interactive logons from standard user workstations were not observed.                           | Supports detection candidate for unexpected interactive use.           |
| VPN sign-ins and unmanaged device authentications were not observed.                            | Supports detection candidates for higher-risk authentication patterns. |
| Logon type is incomplete for some authentication sources.                                       | Visibility gap requiring follow-up.                                    |
| Service account ownership is incomplete for some accounts.                                      | Governance and triage gap requiring follow-up.                         |

### Impact

The hunt did not identify confirmed account compromise, but it improved understanding of local service account authentication behaviour.

The result can support:

```text
- anomaly-driven hunting
- SOC triage of service account alerts
- detection engineering for unexpected service account use
- baseline documentation
- service account ownership review
- identity telemetry improvement
```

The visibility gap around incomplete logon type and ownership data may reduce confidence in future service account hunts and detections if not remediated.

### Recommendations

| Recommendation                                                             | Owner                  | Priority |
| -------------------------------------------------------------------------- | ---------------------- | -------- |
| Publish service account interactive logon baseline.                        | Threat Hunting         | Medium   |
| Validate detection for service account interactive logon from workstation. | Detection Engineering  | Medium   |
| Validate detection for service account sign-in from unmanaged device.      | Detection Engineering  | Medium   |
| Review accounts without confirmed business owner.                          | Identity Team          | Medium   |
| Review authentication telemetry gaps.                                      | Identity Platform Team | Medium   |
| Add SOC triage guidance for unexpected service account usage.              | SOC Lead               | Low      |

### Conclusion

No confirmed account compromise was identified within the scoped population and review period.

The hunt produced useful baseline observations and detection engineering candidates. The result should be used to improve SOC triage context, validate detection opportunities, review service account ownership and address authentication telemetry gaps.

### Appendices

Appendices may include:

```text
- Hunt queries
- Field definitions
- Detection candidate logic
- Baseline data
- Sample events
- False-positive notes
- Service account ownership notes
- Telemetry coverage summary
- ATT&CK mapping
```

Appendices are useful when technical detail must be preserved without overloading the main report.

## Short Report Example

Not every report needs to be long.

A short report may be appropriate when the work is narrow, the audience is operational and the conclusion is straightforward.

Example:

```text
Report:
No-result hunt summary: Service account interactive logons from workstations

Summary:
No interactive logons from standard user workstations were observed for the
scoped Finance-owned service accounts during the period 2026-06-01 to
2026-06-30.

Scope:
Selected Finance-owned service accounts. Human user accounts, privileged
administrator accounts, disabled service accounts and application accounts
owned by other business units were excluded.

Data source:
Authentication telemetry from identity provider and domain controllers.

Limitations:
Logon type was incomplete for some authentication sources. Some SaaS
authentication events lacked source device context. The result does not prove
that service account misuse never occurs in the environment.

Outcome:
No evidence supporting the hypothesis was found within the scoped data.

Recommended action:
Use this result to support a detection candidate for service account interactive
logons from standard user workstations and review authentication telemetry
completeness.
```

This is a report. It is short, but it still explains the scope, data source, result, limitations and recommendation.

## Long Report Example

A longer report may be required when the work has high impact, complex evidence or multiple audiences.

A long report may include:

```text
- Executive summary
- Background
- Objective
- Scope
- Timeline
- Methodology
- Data sources
- Findings
- Evidence
- Impact assessment
- Affected assets or users
- Detection opportunities
- Response actions
- Recommendations
- Limitations
- Appendices
- Revision history
```

Long reports should still be readable.

A long report fails when it becomes a warehouse for raw data. The main body should explain the outcome. Appendices can preserve supporting detail.

## Findings and Recommendations

Findings should be written so they can drive action. A useful finding should include enough context for the reader to understand what was observed, why it matters and what should happen next.

A practical structure is:

```text
Finding:
[What was found]

Evidence:
[What supports the finding]

Impact:
[Why it matters]

Recommendation:
[What should happen next]

Owner:
[Who should act]

Priority:
[How urgent or important it is]
```

Example:

```text
Finding:
Logon type is incomplete for part of the scoped authentication telemetry.

Evidence:
During the service account hunt, logon type was missing from 12% of sampled
authentication events from one authentication source.

Impact:
Incomplete logon type reduces confidence in service account interactive logon
hunting and detection logic.

Recommendation:
Identity Platform Team should review authentication logging configuration and
confirm whether logon type can be captured consistently.

Owner:
Identity Platform Team

Priority:
Medium
```

This is actionable. It tells someone what to fix and why it matters.

## Technical Appendices

Technical detail should be preserved, but it should not always be placed in the main body.

Appendices are useful for:

* queries
* sample events
* field mappings
* hashes
* indicators
* timelines
* screenshots
* detection logic
* baseline data
* enrichment results
* validation evidence
* tool output

A technical appendix allows the main report to stay readable while still preserving evidence.

A common failure is to make the report unreadable by placing every technical detail in the main narrative. That usually helps nobody. The executive reader gets lost, and the technical reader still has to search for the exact detail they need.

Use the main report to explain the outcome. Use appendices to preserve supporting detail.

## Confidence and Limitations

Reports should be honest about confidence and limitations. A report should not claim more than the evidence supports. Useful limitation statements include:

```text
This result is limited to the scoped accounts and review period.

The conclusion depends on authentication telemetry being available and complete.

The hunt did not include service accounts owned by other business units.

No interactive logons from standard user workstations were observed, but logon
type was incomplete for some authentication sources.

The result does not prove that the behaviour never occurs. It means it was not
observed in the scoped data.
```

This kind of language protects the quality of the report. It also protects the organisation from false certainty.

## Reporting Failures and False Confidence

A weak report can create false confidence. For example, a weak no-result report may say:

```text
No suspicious service account activity was found.
```

That is not enough. A stronger report would say:

```text
No interactive service account logons from standard user workstations were
observed for the scoped Finance-owned service accounts during the review period.
The result depends on authentication telemetry from the identity provider and
domain controllers. SaaS authentication sources were not fully covered, and
logon type was incomplete for some events.
```

The first statement may create false confidence. The second explains the result, scope and limitation.

This matters because reports often travel further than the analyst expects. A careful limitation in the report may prevent a later reader from turning a scoped finding into a broad claim.

## Common Failure Modes

Reports often fail when they are written as a memory dump instead of a decision aid.

| Failure mode                  | Why it hurts                                                      |
| ----------------------------- | ----------------------------------------------------------------- |
| No clear audience             | The report gives the wrong level of detail.                       |
| No scope                      | The conclusion may be interpreted too broadly.                    |
| No limitations                | Readers may overtrust the result.                                 |
| Too much raw data             | The outcome becomes hard to understand.                           |
| Too little evidence           | Findings cannot be reviewed.                                      |
| No recommendations            | The report does not drive action.                                 |
| Mixed facts and assumptions   | Readers cannot tell what is known.                                |
| Overstated conclusions        | The report creates false confidence.                              |
| No owner for actions          | Recommendations do not become improvements.                       |
| No appendix                   | Technical detail is lost or clutters the main report.             |
| One format for every audience | The report fails to serve anyone well.                            |
| Report with no decision       | The report exists, but nobody can use it to decide, act or learn. |

The most common mistake is confusing volume with quality. A report is not better because it is longer. A report is better when the right reader can use it.

## Reports and Threat Hunting Maturity

A mature threat hunting function does not only perform hunts. It communicates outcomes. That matters because the value of a hunt is not limited to what the hunter personally understands. The value appears when the organisation can use the result.

Reports help turn hunting into:

```text
- decisions
- detections
- baselines
- audits
- process improvements
- incident response actions
- architecture improvements
- telemetry improvements
- training material
- management awareness
```

A hunting team that does not report well may still do good analysis, but the organisation will struggle to benefit from it. A hunting team that reports well creates memory.

## Working Position for This Book

For this book, a report is treated as a documented outcome.

It may be short or long, formal or informal, technical or executive. The format should follow the purpose, audience and impact of the work.

A useful report explains what was done, what was found, what it means, what is uncertain and what should happen next.

A report earns its place when the right reader can use it to understand, decide or act.

That is the standard. Not because every hunt needs a long report, but because security work should not disappear when the analyst closes the notebook.

## Resources

* [NIST SP 800-61 Rev. 2: Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
* [CISA: Incident Response](https://www.cisa.gov/resources-tools/resources/incident-response)
* [MITRE ATT&CK: Valid Accounts](https://attack.mitre.org/techniques/T1078/)
* [MITRE ATT&CK: Domain Accounts](https://attack.mitre.org/techniques/T1078/002/)
* [MITRE ATT&CK: Cloud Accounts](https://attack.mitre.org/techniques/T1078/004/)
* [MITRE ATT&CK: Account Discovery](https://attack.mitre.org/techniques/T1087/)
* [Splunk SURGe: PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html)
* [The ThreatHunting Project: Hunting Maturity Model](https://www.threathunting.net/hunting-maturity-model)

## Revision

| Revised Date | Comment                                                                                                                                                                                                             |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-13   | Initial version. Introduced reports as threat hunting deliverables and explained how reports may vary in length, audience, purpose and technical depth.                                                             |
| 2026-07-14   | Revised for flow, added report lifecycle, when not to write a full report, reporting tool selection, reporting failure examples and replaced the reference report with service account interactive logon behaviour. |