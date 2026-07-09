---
title: "Creating Hypotheses"
description: "How to create threat hunting hypotheses that are specific, testable, relevant and useful enough to drive a real hunt."
date: 2024-11-02T15:09:15+01:00
lastmod: 2026-07-09
draft: false
weight: 9
tags:
- fundamentals
- threat hunting
- hypothesis
- methodology
keywords:
- threat hunting hypothesis
- creating threat hunting hypotheses
- hypothesis-driven hunting
- threat hunting methodology
- MITRE ATT&CK
- threat intelligence
- hunting hypothesis examples
- SMART criteria
- observable behaviour
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**The previous chapters introduced hypotheses as part of threat hunting methodology. This chapter goes deeper. It focuses on how to create hypotheses that are specific, testable, relevant and useful enough to drive an actual hunt.**

Threat hunting relies on more than intuition and experience. Those things matter, but they are not enough. A hunt needs a structured question. It needs a reason. It needs observable behaviour. It needs data that can support or weaken the idea being tested.

That is where the hypothesis comes in.

* A good hypothesis gives the hunt direction. It does not prove anything by itself. It does not guarantee that the team will find malicious activity. It simply gives the hunter a clear statement to test against evidence.
* A weak hypothesis sends the hunter browsing through logs.
* A strong hypothesis tells the hunter what evidence would matter.

> A hypothesis is not a suspicion with better wording. It is a statement you can test against evidence.
>
> -- Roger Johnsen

## What a Threat Hunting Hypothesis Is

A threat hunting hypothesis is a testable statement about behaviour that may indicate malicious activity, security weakness, detection failure, visibility gap or operational risk. It usually combines several elements:

| Element    | Purpose                                                |
| ---------- | ------------------------------------------------------ |
| Assumption | What do we think may be happening?                     |
| Behaviour  | What observable activity would support the assumption? |
| Context    | Why does this matter in our environment?               |
| Data       | Which telemetry can test the idea?                     |
| Scope      | Where and when will we look?                           |
| Output     | What useful result may come from the hunt?             |

A hypothesis does not need to be complicated. It needs to be clear enough to test. 

Weak example:

```text
Attackers may be present in the network.
```

Better example:

```text
If attackers gained initial access through malicious documents, Office applications may spawn PowerShell, cmd.exe, wscript.exe or similar interpreters on user workstations.
```

The second example gives the hunter something to do. It points towards process telemetry, parent-child relationships, command-line arguments, device context and user context. That is what makes it useful. A hypothesis should help the hunter move from a concern to an investigation.

## What a Hypothesis Is Not

It is useful to be clear about what a hypothesis is not:

* A hypothesis is not the same as an alert. An alert says that something matched existing logic. A hypothesis asks whether something may be happening that the existing logic does not fully cover.
* A hypothesis is not the same as an IOC. An IOC can be a useful starting point, but an IOC search is usually narrow. A hypothesis should move towards behaviour and context.
* A hypothesis is not the same as a threat report. A report may inspire the hunt, but the hunter still needs to translate the report into local telemetry and observable behaviour.
* A hypothesis is not the same as a vague concern.

Concern: 

```text
We are worried about ransomware.
```

That may be a valid concern, but it is not yet a hunting hypothesis. A better version could be:

```text
If ransomware operators are preparing for encryption, we may observe unusual archive creation, privilege escalation attempts, lateral movement, backup access or security tool tampering before encryption begins.
```

That version is still broad, but it has become testable. It points towards behaviours that can be scoped, searched and validated.

| Not a hypothesis                  | Why it is weak                                 |
| --------------------------------- | ---------------------------------------------- |
| “There may be attackers here.”    | Too broad to test.                             |
| “Search for this IP address.”     | Indicator lookup, not a hypothesis.            |
| “Look for suspicious PowerShell.” | Too vague without behaviour, scope or context. |
| “APT groups target our sector.”   | Threat awareness, not a local test.            |
| “This alert looks bad.”           | Observation, not a structured assumption.      |

A hypothesis should not only sound plausible. It should make the next analytical step clearer.

## Where Hypotheses Come From

Threat hunting hypotheses can come from many sources. They do not all start with threat intelligence. Some of the best hypotheses come from local observations, weak detections, strange baseline behaviour or questions raised during incident response.

Useful sources include:

| Source              | How it can become a hypothesis                                         |
| ------------------- | ---------------------------------------------------------------------- |
| Threat intelligence | Translate adversary behaviour into local observable activity.          |
| Past incidents      | Hunt for related behaviour, recurrence or missed signals.              |
| SOC observations    | Turn repeated alerts or analyst uncertainty into structured questions. |
| Detection gaps      | Ask what current alerting logic may fail to see.                       |
| Red team results    | Hunt for tested techniques, missed detections or weak controls.        |
| Vulnerabilities     | Ask how exploitation or post-exploitation behaviour would appear.      |
| Business context    | Focus hunting on critical systems, processes or identities.            |
| Baseline anomalies  | Investigate behaviour that does not fit expected activity.             |
| Industry trends     | Convert sector-relevant reporting into local hypotheses.               |
| New telemetry       | Ask what behaviours can now be tested that were previously invisible.  |

The source matters less than the translation. Threat intelligence, for example, is not automatically a hypothesis. A report may describe an actor using PowerShell, scheduled tasks, cloud consent abuse or remote services. The hunter still has to ask:

```text
What would this behaviour look like in our environment?
```

and:

```text
Do we have the data needed to test it?
```

> Threat intelligence gives you ideas. Telemetry decides whether those ideas can be tested.
>
> -- Roger Johnsen

## From Input to Hypothesis

The practical work is turning raw input into a testable hypothesis. A simple flow is:

```text
Input → Local relevance → Observable behaviour → Required data → Hypothesis
```

Example:

| Stage                | Example                                                                                                                                                             |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Input                | Threat report describes phishing leading to PowerShell execution.                                                                                                   |
| Local relevance      | The organisation receives many attachments by email and uses Microsoft 365.                                                                                         |
| Observable behaviour | Office applications spawning PowerShell or script interpreters.                                                                                                     |
| Required data        | EDR process telemetry, command-line logging, email context, device inventory.                                                                                       |
| Hypothesis           | If malicious documents are used for initial execution, Office applications may spawn PowerShell, cmd.exe, wscript.exe or similar interpreters on user workstations. |

This translation step is important. Without it, the team may simply copy ideas from reports into hunts without checking whether they are relevant, observable or testable.

Another example:

| Stage                | Example                                                                                                                                                           |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Input                | SOC sees repeated password spraying alerts.                                                                                                                       |
| Local relevance      | Several users have cloud access and privileged roles.                                                                                                             |
| Observable behaviour | Many failed logins followed by successful authentication from shared infrastructure.                                                                              |
| Required data        | Identity logs, MFA status, user risk, IP reputation, geolocation, device context.                                                                                 |
| Hypothesis           | If password spraying is successful, we may observe a pattern of repeated failed authentication attempts followed by successful login from unusual infrastructure. |

The quality of the hypothesis depends on how well the input is translated into behaviour.

## Making the Hypothesis Testable

A hypothesis should be testable. That means the hunter should be able to define what evidence would support it, what evidence would weaken it and what data is needed.

Ask these questions:

| Question                             | Purpose                                 |
| ------------------------------------ | --------------------------------------- |
| What behaviour are we looking for?   | Prevents vague hunting.                 |
| Where should this behaviour appear?  | Identifies data sources.                |
| Who or what is in scope?             | Defines the population.                 |
| What time period matters?            | Keeps the hunt bounded.                 |
| What would support the hypothesis?   | Defines useful evidence.                |
| What would weaken the hypothesis?    | Reduces confirmation bias.              |
| What benign explanations are likely? | Improves validation.                    |
| What output can this hunt produce?   | Connects the hypothesis to improvement. |

A testable hypothesis often contains three things:

```text
If [condition or threat behaviour], then [observable activity] may appear in [specific scope or data].
```

Example:

```text
If attackers are using valid accounts for lateral movement, then we may observe administrative logons from unusual source systems to servers where those users do not normally authenticate.
```

This is testable because it points towards:

* authentication logs
* source and destination systems
* user roles
* administrative logon types
* baseline behaviour
* time window
* exceptions
* validation paths

A hypothesis does not have to be perfect. It has to be good enough to test.

## Using SMART Without Losing the Point

SMART can help refine hunting hypotheses, but it should not turn the hunt into management theatre.

SMART means:

| Keyword    | Meaning in threat hunting                                                         |
| ---------- | --------------------------------------------------------------------------------- |
| Specific   | The hypothesis identifies a behaviour, technique, asset group or user population. |
| Measurable | The hunt can define what evidence, count, pattern or observation matters.         |
| Achievable | The data and skills required are available.                                       |
| Relevant   | The hypothesis matters to the organisation or threat landscape.                   |
| Time-bound | The hunt has a defined time window or period of relevance.                        |

Weak version:

```text
We will look for suspicious logins.
```

SMART-er version:

```text
During the last 30 days, we will identify successful logins to privileged accounts from unfamiliar countries, rare autonomous systems or unmanaged devices, and compare them against known travel, VPN usage and administrative patterns.
```

This is better because it defines:

* time window
* account population
* observable behaviour
* data needed
* validation context

SMART should sharpen the hunt, not bury it in paperwork.

> SMART is useful when it makes the hypothesis clearer. It is useless when it turns hunting into form-filling.
>
> -- Roger Johnsen

## Weak and Strong Hypotheses

A strong hypothesis is not necessarily longer. It is more testable.

| Weak hypothesis                | Stronger hypothesis                                                                                                                                                                        |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| There may be phishing.         | If phishing leads to credential theft, we may observe failed and successful logins from unusual infrastructure shortly after suspicious email delivery.                                    |
| Attackers may use PowerShell.  | If attackers use PowerShell for execution, we may observe PowerShell launched by unusual parent processes with encoded commands, download behaviour or hidden execution.                   |
| There may be lateral movement. | If attackers move laterally using remote services, we may observe administrative logons, remote service creation or remote command execution from systems that are not normal admin hosts. |
| Someone may be stealing data.  | If data is being staged for exfiltration, we may observe unusual archive creation, large file movement or outbound transfers to rare destinations outside business hours.                  |
| There may be persistence.      | If attackers establish persistence using scheduled tasks, we may observe newly created tasks with unusual names, paths, users or execution commands on endpoints.                          |

The stronger versions do not claim that an attack is happening. They describe what evidence may support the idea. That is the point. A strong hypothesis should help the hunter decide:

* where to look
* what to look for
* what context matters
* what would be normal
* what would be suspicious
* what to do with the result

A weak hypothesis often produces open-ended searching. A strong hypothesis produces directed investigation.

## Using MITRE ATT&CK as Structure

MITRE ATT&CK is useful for hypothesis creation because it provides a structured vocabulary for adversary behaviour. It can help the hunter ask:

* Which tactic are we concerned about?
* Which technique may be relevant?
* What behaviour would that technique create?
* Which data sources could show it?
* Which detections do we already have?
* What might our existing detections miss?

ATT&CK should be used as structure, not as a substitute for thinking. A technique page may describe adversary behaviour, but it does not automatically tell you what is relevant in your environment. The hunter still needs to translate the technique into local systems, logs and context.

Example:

| ATT&CK area                                   | Hunting translation                                                                                      |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Initial Access / Phishing                     | Which email, identity and endpoint behaviours would suggest successful phishing?                         |
| Execution / Command and Scripting Interpreter | Which parent-child process relationships and command-line patterns are unusual?                          |
| Persistence / Scheduled Task or Job           | Which newly created tasks are rare, suspicious or inconsistent with normal administration?               |
| Credential Access / OS Credential Dumping     | Which processes accessed credential material, and is that expected?                                      |
| Lateral Movement / Remote Services            | Which users authenticated remotely to which systems, and is that normal?                                 |
| Exfiltration                                  | Which large transfers, rare destinations or unusual protocols may indicate data leaving the environment? |

The useful movement is:

```text
ATT&CK technique → local behaviour → data source → hypothesis → hunt plan
```

Not:

```text
ATT&CK technique → generic search → conclusion
```

## Example Hypotheses

Below are examples of threat hunting hypotheses inspired by common ATT&CK areas. They are intentionally written as hypotheses, not conclusions.

### Initial Access: Phishing

| Field            | Example                                                                                                                                                                           |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hypothesis       | If phishing emails are used to steal credentials, we may observe suspicious email delivery followed by failed and successful authentication attempts from unusual infrastructure. |
| Possible data    | Email logs, identity logs, MFA events, device context, IP reputation.                                                                                                             |
| What to validate | Whether the login pattern fits the user, device, location, MFA history and known travel or VPN use.                                                                               |

### Execution: PowerShell

| Field            | Example                                                                                                                                                                           |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hypothesis       | If attackers use PowerShell for execution, we may observe PowerShell launched by unusual parent processes, encoded commands, download behaviour or hidden execution on endpoints. |
| Possible data    | EDR process telemetry, command-line logs, PowerShell logs, network telemetry.                                                                                                     |
| What to validate | Parent process, user context, script content, destination, frequency and known administrative activity.                                                                           |

### Persistence: Scheduled Tasks

| Field            | Example                                                                                                                                                        |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hypothesis       | If attackers establish persistence using scheduled tasks, we may observe newly created tasks with unusual names, paths, users, triggers or execution commands. |
| Possible data    | Windows event logs, EDR telemetry, task scheduler logs, file system telemetry.                                                                                 |
| What to validate | Task creator, command path, trigger, signed binaries, endpoint role and administrative change history.                                                         |

### Credential Access: Credential Dumping

| Field            | Example                                                                                                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hypothesis       | If attackers attempt credential dumping, we may observe suspicious access to LSASS, credential material, browser credential stores or security account databases. |
| Possible data    | EDR telemetry, process access events, command-line logs, file access logs, security logs.                                                                         |
| What to validate | Process lineage, tool behaviour, user privileges, endpoint role and known security tooling.                                                                       |

### Lateral Movement: Remote Services

| Field            | Example                                                                                                                                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hypothesis       | If attackers use remote services for lateral movement, we may observe administrative logons, remote service creation or remote command execution from systems that are not normal administrative hosts. |
| Possible data    | Authentication logs, EDR telemetry, Windows service events, network telemetry, asset inventory.                                                                                                         |
| What to validate | Source host, destination host, user role, logon type, service name, process lineage and normal admin patterns.                                                                                          |

### Exfiltration: Unusual Data Movement

| Field            | Example                                                                                                                                                                          |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hypothesis       | If attackers stage or exfiltrate data, we may observe unusual archive creation, large file movement or outbound transfers to rare destinations outside normal business patterns. |
| Possible data    | File telemetry, proxy logs, DNS logs, network flow, DLP alerts, cloud storage logs.                                                                                              |
| What to validate | Business process, user role, file type, destination, time of day, volume and previous baseline.                                                                                  |

These examples are not meant to be copied blindly. They are templates for thinking. Each organisation must adapt the hypothesis to its own systems, telemetry, business context and threat landscape.

## What Usually Goes Wrong

Several problems appear repeatedly when teams create hunting hypotheses.

| Problem                                                    | Why it hurts                                                              |
| ---------------------------------------------------------- | ------------------------------------------------------------------------- |
| The hypothesis is too broad                                | The hunt becomes endless and difficult to conclude.                       |
| The hypothesis is just an IOC search                       | The team may miss behaviour that does not match known indicators.         |
| The hypothesis cannot be tested                            | The required data does not exist or cannot answer the question.           |
| The hypothesis ignores local context                       | The hunt may be technically interesting but operationally irrelevant.     |
| The hypothesis assumes malicious intent too early          | The team jumps from observation to conclusion.                            |
| The hypothesis lacks scope                                 | The hunt expands beyond control.                                          |
| The hypothesis has no output path                          | The result does not improve detection, response, visibility or knowledge. |
| The hypothesis is copied from a report without translation | The hunt does not fit the organisation’s telemetry or environment.        |

Most weak hypotheses fail because they do not connect the idea to observable behaviour. The fix is usually simple:

```text
Make the behaviour visible.
Make the scope clear.
Make the data explicit.
Make the conclusion testable.
```

## Working Position for This Book

A threat hunting hypothesis is a practical tool. It should not be written to sound impressive. It should be written to guide investigation.

A good hypothesis connects:

```text
Threat idea.
Local relevance.
Observable behaviour.
Data source.
Scope.
Validation.
Output.
```

That connection is what makes hypothesis-driven hunting useful. The hypothesis does not need to prove that an attacker is present. It needs to help the hunter test whether a behaviour exists, whether it matters and what should happen next.

> A good hypothesis gives the hunt direction. A great hypothesis also tells you what evidence would change your mind.
>
> -- Roger Johnsen

## Resources

* [MITRE ATT&CK](https://attack.mitre.org/)
* [MITRE ATT&CK Overview](https://attack.mitre.org/overview/)
* [SOC Prime: Threat Hunting Hypothesis Examples](https://socprime.com/blog/threat-hunting-hypothesis-examples/)
* [Cyborg Security: 50 Threat Hunting Hypothesis Examples](https://www.cyborgsecurity.com/blog/50-threat-hunting-hypothesis-examples/)
* [Splunk: PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-hypothesis-driven-threat-hunting.html)
* [Cyborg Security: Art of the Hunt – Building a Threat Hunting Hypothesis List](https://www.cyborgsecurity.com/blog/art-of-the-hunt-building-a-threat-hunting-hypothesis-list/)
* [SANS Institute Whitepapers on Threat Hunting](https://www.sans.org/white-papers/)
* [George T. Doran: There’s a S.M.A.R.T. Way to Write Management’s Goals and Objectives](https://www.jstor.org/stable/40604294)

## Revision

| Revised Date | Comment                                                                                                      |
| ------------ | ------------------------------------------------------------------------------------------------------------ |
| 2026-07-09   | Major rewrite. Reframed the article as a dedicated deep-dive on creating testable threat hunting hypotheses. |
| 2024-11-02   | Added page                                                                                                   |
