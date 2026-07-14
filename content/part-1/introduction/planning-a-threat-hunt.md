---
title: "Planning a Threat Hunt"
description: "How to turn a hunting idea or hypothesis into a scoped, executable and reviewable threat hunt plan."
date: 2024-10-27T11:10:42+01:00
lastmod: 2026-07-09
draft: false
weight: 9
tags:
    - fundamentals
    - threat hunting
    - planning
    - methodology
keywords:
    - threat hunt planning
    - threat hunting plan
    - threat hunting methodology
    - hunting hypothesis
    - threat hunting scope
    - threat hunting telemetry
    - threat hunting output
    - threat hunting validation
    - AI assisted threat hunting
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**The previous chapter described how to start and grow a threat hunting programme. This chapter moves one level down: how to plan a single hunt.**

A threat hunting programme gives you the capability. A hunt plan turns that capability into a specific investigation.

That distinction matters.

A hunt plan is not a project plan. It is not a long document written to satisfy process requirements. It is the bridge between a hypothesis and an investigation. It explains what the team is trying to find, why it matters, where to look, how to test it, how to validate the result and what useful output may come from the work. A hunt based on “let us just look around” may occasionally find something interesting, but it is difficult to repeat, review or improve. A structured hunt plan does not remove creativity. It gives creativity a shape.

> A threat hunt should be structured enough that someone else can understand the reasoning, but flexible enough that the hunter can follow the evidence.
>
> -- Roger Johnsen

## What a Hunt Plan Is

A hunt plan is a practical working document. It should help the hunter move from an idea to an executable investigation. It should also help someone else understand what was tested afterwards.

At minimum, a hunt plan should answer:

| Question                                     | Why it matters                                                      |
| -------------------------------------------- | ------------------------------------------------------------------- |
| What are we trying to test?                  | Defines the purpose of the hunt.                                    |
| Why does this matter?                        | Connects the hunt to risk, threat intelligence or operational need. |
| What is the hypothesis?                      | Gives the hunt a testable direction.                                |
| What behaviour would support the hypothesis? | Turns the idea into observable activity.                            |
| Which data sources are required?             | Determines whether the hunt is possible.                            |
| What is in scope and out of scope?           | Keeps the hunt controlled.                                          |
| How will we search or analyse?               | Defines the method.                                                 |
| How will we validate findings?               | Protects against weak conclusions.                                  |
| What output should the hunt produce?         | Connects the hunt to improvement.                                   |

A hunt plan should be short enough to use, but clear enough to survive review. If the plan cannot explain what the hunt is testing, the hunt is probably not ready.

## Start With the Question

A hunt usually starts with a question. The question may come from:

* a SOC observation
* a recurring alert pattern
* a recent incident
* threat intelligence
* a vulnerability or exposure
* a detection gap
* a red team or purple team finding
* a new technique described in public reporting
* uncertainty about whether a behaviour is visible in the environment

Examples:

* Can we identify Office applications spawning command interpreters on ordinary user workstations?
* Do we have visibility into suspicious PowerShell activity across managed endpoints?
* Can we detect successful authentication after password spraying behaviour?
* Are administrative tools being used from systems where they are not expected?

The question does not need to be complicated. It needs to be useful. A weak question is:

```text
Are there attackers in the environment?
```

That may be what the organisation ultimately cares about, but it is too broad for a single hunt. A better question is:

```text
Can we identify user workstations where Office applications spawned PowerShell, cmd.exe, wscript.exe or similar interpreters during the last 30 days?
```

That question can be tested.

## Define Scope and Boundaries

Scope is what keeps the hunt from becoming endless. A hunt should define what is included and what is not included. This is not bureaucracy. It is how the team prevents a good question from turning into an uncontrolled investigation. Scope may include:

| Scope element   | Example                                                        |
| --------------- | -------------------------------------------------------------- |
| Time window     | Last 7 days, last 30 days, last 90 days                        |
| Asset type      | Workstations, servers, domain controllers, cloud workloads     |
| User population | All users, privileged users, finance users, external accounts  |
| Data source     | EDR process telemetry, identity logs, DNS logs, proxy logs     |
| Environment     | Production, corporate IT, cloud tenant, specific business unit |
| Technique       | PowerShell execution, credential dumping, lateral movement     |
| Exclusion       | Known admin jump hosts, lab machines, test tenants             |

The hunt should also define boundaries. For example:

```text
This hunt looks for suspicious Office-spawned command interpreters on managed Windows workstations. It does not attempt full malware analysis, user forensics or incident response. Confirmed suspicious activity will be escalated.
```

That kind of boundary is useful. It protects the hunt from becoming everything at once.

## Turn the Hypothesis Into Observable Behaviour

A hypothesis is useful only when it can be tested against data. A common mistake is to write a hypothesis that sounds good but does not translate into observable behaviour.

Weak hypothesis:

```text
Attackers may be using malware in the environment.
```

Better hypothesis:

```text
If malicious documents are used for initial execution, Office applications may spawn PowerShell, cmd.exe, wscript.exe or similar interpreters on user workstations.
```

The second hypothesis gives the hunter something to look for. The next step is to identify observable behaviours:

| Hypothesis element           | Observable behaviour                                                       |
| ---------------------------- | -------------------------------------------------------------------------- |
| Malicious document execution | Office process starts script interpreter or shell                          |
| PowerShell abuse             | Encoded command, hidden window, download cradle, suspicious parent process |
| Credential access            | LSASS access, credential dumping tool behaviour, unusual handle access     |
| Lateral movement             | Remote service creation, admin share access, unusual remote execution      |
| Data staging                 | Large archive creation, unusual compression, staging paths                 |
| Exfiltration                 | Unusual outbound volume, rare destination, abnormal protocol use           |

This is where the hunt becomes practical.

> A hypothesis is not useful because it sounds intelligent. It is useful when it can be tested against observable behaviour.
>
> -- Roger Johnsen

## Identify Required Data Sources

A hunt plan should identify the data needed to test the hypothesis. This is where many hunts fail. The idea may be good, but the required telemetry may not exist, may not be retained long enough, may not be parsed correctly, or may not contain the fields needed to answer the question.

For each data source, ask:

| Question                         | Why it matters                                               |
| -------------------------------- | ------------------------------------------------------------ |
| Does the data exist?             | Determines whether the hunt is possible.                     |
| Is the data searchable?          | Determines whether the hunt can be executed efficiently.     |
| Is the time window sufficient?   | Determines whether historical behaviour can be tested.       |
| Are the required fields present? | Determines whether the query can answer the question.        |
| Is the data reliable?            | Determines whether conclusions can be trusted.               |
| Is enrichment available?         | Helps connect entities, users, devices and business context. |

Example for an Office-spawned PowerShell hunt:

| Data source            | Purpose                                                        |
| ---------------------- | -------------------------------------------------------------- |
| EDR process telemetry  | Identify parent-child process relationships                    |
| Command-line telemetry | Inspect PowerShell arguments and execution patterns            |
| Device inventory       | Separate managed workstations from servers and special systems |
| User context           | Identify user, role and expected behaviour                     |
| Network telemetry      | Check whether execution led to outbound connections            |
| DNS or proxy logs      | Identify rare or suspicious destinations                       |

If a required data source is missing, the hunt can still be useful. It may become a visibility-gap hunt. That should be documented clearly.

```text
The hypothesis could not be fully tested because unmanaged devices do not provide endpoint process telemetry.
```

That is not a failure. That is an output.

## Decide the Method

The hunt plan should describe how the team will test the hypothesis. This does not need to be overly detailed, but it should be clear enough that another analyst can understand the approach.

Common hunting methods include:

| Method                     | Use when                                                            |
| -------------------------- | ------------------------------------------------------------------- |
| Behavioural search         | Looking for activity patterns associated with attacker tradecraft   |
| Anomaly detection          | Looking for deviations from expected behaviour                      |
| Baseline comparison        | Comparing observed behaviour against known normal activity          |
| IOC pivoting               | Using indicators as a starting point, not the entire hunt           |
| Entity-based investigation | Following users, devices, IPs, files or identities through the data |
| Temporal analysis          | Looking at sequence, timing and causality                           |
| Relationship analysis      | Connecting events across systems or data sources                    |

A good plan may combine several methods.

For example:

```text
Search for Office applications spawning command interpreters, baseline frequency across workstations, exclude known packaging hosts, review command-line arguments, and pivot to network connections within five minutes of execution.
```

That tells the team what to do. It also makes the hunt reviewable.

## Prepare Queries, Enrichment and Notes

A hunt plan should prepare the practical material needed for execution. This may include:

* initial queries
* field mappings
* known exclusions
* enrichment sources
* lookup tables
* asset context
* user context
* known administrative tools
* notes from previous hunts
* expected false positives
* triage questions

Do not wait until the hunt is finished to think about documentation. Good notes during the hunt should capture:

| Note type      | Example                                                                    |
| -------------- | -------------------------------------------------------------------------- |
| Assumption     | Office applications rarely spawn command interpreters in this environment. |
| Query decision | Excluded known software packaging hosts after validation.                  |
| Pivot          | Checked DNS and proxy logs for outbound activity after process execution.  |
| Limitation     | Some unmanaged endpoints lack process telemetry.                           |
| Validation     | Confirmed activity on one host was legitimate packaging work.              |
| Open question  | Need to confirm whether macro-enabled documents are logged separately.     |

This is where many hunts become fragile. The analyst may remember why something was done, but the organisation will not.

> The reasoning is part of the hunt. If you do not document it, you lose part of the evidence.
>
> -- Roger Johnsen

## Execute With Discipline

Execution should follow the plan, but not blindly. The plan gives direction. The data may still force the hunter to adjust.

During execution, the hunter should:

* run the planned queries
* inspect the results
* validate obvious false positives
* pivot when evidence justifies it
* record query changes
* document assumptions
* note gaps or unexpected behaviour
* avoid turning every interesting result into a new hunt

That last point matters. Threat hunting requires curiosity, but curiosity without discipline becomes wandering. If a new lead appears, the team should decide whether it belongs inside the current hunt or should become a separate hypothesis.

Example:

```text
The hunt for Office-spawned PowerShell reveals unusual rundll32 behaviour. If this is not directly connected to the hypothesis, document it as a follow-up hunt instead of derailing the current one.
```

A disciplined hunt does not ignore interesting findings. It controls them.

## Validate Before Concluding

A hunt should not jump from observation to conclusion. Finding a suspicious pattern is not the same as proving malicious activity.

Validation may include:

| Validation question                               | Purpose                             |
| ------------------------------------------------- | ----------------------------------- |
| Is the activity expected for this user or system? | Adds entity context.                |
| Is this common in the environment?                | Adds baseline context.              |
| Is there a legitimate administrative explanation? | Reduces false positives.            |
| Did related activity occur before or after?       | Adds temporal context.              |
| Are multiple data sources consistent?             | Strengthens confidence.             |
| Could the hypothesis be wrong?                    | Protects against confirmation bias. |

The hunter should be especially careful with language.

Weak conclusion:

```text
This host was compromised.
```

Better conclusion:

```text
This host showed rare Office-spawned PowerShell execution followed by outbound connections to an uncommon domain. The activity is inconsistent with observed baseline behaviour and should be escalated for incident response validation.
```

That is stronger because it separates observation, context and recommended action.

> The hunt does not end when something looks suspicious. It ends when the team has tested what the observation can and cannot support.
>
> -- Roger Johnsen

## Decide the Output

A hunt plan should define what useful output may come from the hunt. Not every hunt ends with an incident. That is normal.

Possible outputs include:

| Output                       | Description                                       |
| ---------------------------- | ------------------------------------------------- |
| Confirmed finding            | Activity requiring escalation or response         |
| Detection idea               | Logic that can become a detection                 |
| Detection improvement        | Better tuning, enrichment or triage guidance      |
| Visibility gap               | Missing telemetry, retention, parsing or coverage |
| Baseline                     | Better understanding of normal behaviour          |
| Hunt package                 | Reusable hypothesis, queries and notes            |
| Threat intelligence feedback | Local observations that refine intelligence       |
| Follow-up hypothesis         | A new question that emerged during the hunt       |

This should be considered during planning, not only after execution. The hunt is more useful when the team knows what kind of improvement it is trying to produce.

```text
If we find suspicious activity, we escalate it.
If we find no suspicious activity, we still document baseline and visibility gaps.
If we find missing data, we create a telemetry improvement request.
If we find repeatable logic, we hand it to detection engineering.
```

That is how a hunt becomes operationally valuable.

## Where AI Can Assist

AI can support threat hunting, but it should not own the reasoning.

Used well, AI can help with:

| AI use                   | Useful for                                                           |
| ------------------------ | -------------------------------------------------------------------- |
| Hypothesis brainstorming | Generating candidate questions from threat intelligence or incidents |
| Query drafting           | Producing first-pass KQL, SQL, SPL or Sigma-style logic              |
| Explanation              | Explaining unfamiliar artefacts, fields or techniques                |
| Summarisation            | Turning rough notes into readable summaries                          |
| Alternative explanations | Suggesting benign causes that should be checked                      |
| Report structure         | Helping organise findings, limitations and recommendations           |

But every AI-assisted output needs review. AI can hallucinate fields, invent platform behaviour, overstate confidence, produce syntactically plausible but logically weak queries, or turn a weak assumption into a confident conclusion.

A useful rule is:

```text
AI may help draft the plan. The hunter must own the plan.
```

That means the hunter must verify:

* whether the data exists
* whether the query tests the hypothesis
* whether field names are correct
* whether the logic makes sense
* whether the conclusion is supported
* whether alternative explanations were considered

AI is useful when it helps the hunter think. It is dangerous when it replaces the hunter’s thinking.

## Example Hunt Plan

Below is a compact example of a hunt plan.

| Field            | Example                                                                                                                                                               |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hunt name        | Office-spawned PowerShell on user workstations                                                                                                                        |
| Question         | Can we identify Office applications spawning PowerShell or similar interpreters on managed workstations?                                                              |
| Hypothesis       | If malicious documents are used for initial execution, Office applications may spawn PowerShell, cmd.exe, wscript.exe or similar interpreters.                        |
| Why it matters   | This behaviour may indicate initial execution, macro abuse, script execution or malicious document activity.                                                          |
| Scope            | Managed Windows workstations, last 30 days.                                                                                                                           |
| Out of scope     | Servers, known packaging hosts, unmanaged devices without EDR telemetry.                                                                                              |
| Required data    | EDR process telemetry, command-line telemetry, device inventory, user context, DNS or proxy logs.                                                                     |
| Method           | Search for Office parent processes spawning interpreters, baseline frequency, exclude known administrative systems, inspect command lines, pivot to network activity. |
| Validation       | Check user role, device role, known admin activity, frequency, related process tree and network connections.                                                          |
| Possible outputs | Confirmed suspicious activity, detection idea, triage guidance, baseline, visibility gap.                                                                             |

Example execution notes:

| Observation                                                                 | Interpretation                   |
| --------------------------------------------------------------------------- | -------------------------------- |
| Behaviour is rare across managed workstations                               | Useful baseline                  |
| Several results tied to known packaging activity                            | Expected administrative activity |
| One ordinary user workstation shows encoded PowerShell after Word execution | Requires deeper validation       |
| Unmanaged devices cannot be tested                                          | Visibility gap                   |
| DNS lookup to rare external domain occurs after execution                   | Pivot point for investigation    |

Possible conclusion:

```text
Office-spawned command interpreters are rare on managed workstations. Most observed activity was tied to known packaging work, but one user workstation showed Word spawning encoded PowerShell followed by DNS activity to a rare external domain. This should be escalated for incident response validation. The hunt also identified a visibility gap for unmanaged devices.
```

Possible outputs:

| Output              | Result                                                                                  |
| ------------------- | --------------------------------------------------------------------------------------- |
| Incident escalation | One workstation requires validation by incident response                                |
| Detection idea      | Alert on Office applications spawning interpreters outside known administrative systems |
| Triage guidance     | Review parent process, user role, command line, child processes and network activity    |
| Visibility gap      | Unmanaged devices lack sufficient process telemetry                                     |
| Baseline            | Office-spawned interpreters are rare in the managed workstation population              |

This is what a small but useful hunt can look like. It starts with one question, one hypothesis and a limited scope. It ends with evidence, context and operational outputs.

## What Usually Goes Wrong

Several problems repeat when teams plan threat hunts.

| Problem                           | Why it hurts                                                        |
| --------------------------------- | ------------------------------------------------------------------- |
| The hunt starts too broad         | The team cannot tell when the hunt is finished.                     |
| The hypothesis is not testable    | The team cannot map the idea to observable behaviour.               |
| The data is assumed, not verified | The hunt fails during execution or produces weak conclusions.       |
| IOCs dominate the hunt            | The team misses behaviour that does not match known indicators.     |
| Scope is unclear                  | The hunt expands until it becomes unmanageable.                     |
| Documentation starts too late     | Reasoning, pivots and assumptions are lost.                         |
| Validation is weak                | Suspicious observations are treated as conclusions.                 |
| Output is undefined               | The hunt produces interesting notes but no operational improvement. |
| AI output is trusted too quickly  | Plausible suggestions become unverified assumptions.                |

Most of these problems are avoidable. A good hunt plan does not need to be large. It needs to make the reasoning visible.

## Working Position for This Book

A threat hunt plan is the bridge between a hypothesis and an investigation. It should not be written to impress anyone. It should be written so the hunt can be executed, reviewed, challenged and turned into useful security work.

A practical hunt plan should define:

```text
Question.
Hypothesis.
Scope.
Observable behaviour.
Required data.
Method.
Validation.
Output.
```

That is enough to begin. The hunt may still change during execution. That is expected. But when the plan is clear, the team can tell the difference between following the evidence and simply wandering through logs.

> A good hunt plan does not make the hunt rigid. It makes the reasoning visible.
>
> -- Roger Johnsen

## Resources

* [MITRE ATT&CK](https://attack.mitre.org/)
* [MITRE ATT&CK Overview](https://attack.mitre.org/overview/)
* [CIS Threat Hunting: The Basics](https://www.cisecurity.org/white-papers/threat-hunting-the-basics/)
* [CIS Threat Intelligence](https://www.cisecurity.org/controls/threat-intelligence/)
* [Red Canary Threat Hunting Methodology](https://redcanary.com/threat-hunting-methodology/)
* [NIST Computer Security Incident Handling Guide](https://www.nist.gov/publications/computer-security-incident-handling-guide)
* [SANS Data Collection Best Practices](https://www.sans.org/white-papers/38366/)
* [Open Threat Hunting Framework](https://www.openthreat.hunting)

## Revision

| Revised Date | Comment                                                                                                                                                   |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-09   | Major rewrite. Reframed the article from a generic step-by-step checklist into a practical guide for turning a hunting idea into an executable hunt plan. |
| 2025-02-22   | Improved formatting and revised wording                                                                                                                   |
| 2024-10-27   | Added page                                                                                                                                                |
