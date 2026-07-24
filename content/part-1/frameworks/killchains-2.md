---
title: "Unified Kill Chain"
description: "A practical explanation of the Unified Kill Chain and how threat hunters can use it to map attacker progression, align observations with MITRE ATT&CK and structure investigations."
date: 2024-07-07T11:39:29+02:00
lastmod: 2026-07-10
draft: false
weight: 3
tags:
   - frameworks
   - threat hunting
   - unified kill chain
   - intrusion analysis
keywords:
   - Unified Kill Chain
   - UKC
   - threat hunting
   - MITRE ATT&CK
   - intrusion lifecycle
   - initial access
   - lateral movement
   - command and control
   - exfiltration
   - impact
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**The Unified Kill Chain (UKC) is a framework for describing cyber intrusions across the full lifecycle of an operation, from preparation and initial compromise to lateral movement, mission completion and impact.**

The model, introduced by Paul Pols, combines ideas from Lockheed Martin’s Cyber Kill Chain and MITRE ATT&CK. The result is a broader framework that helps defenders reason about both attack progression and attacker behaviour.

For threat hunters, that is useful. The Lockheed Martin Kill Chain is good at showing intrusion progression. MITRE ATT&CK is good at describing techniques. The Unified Kill Chain brings those ideas closer together and gives the hunter a structure for asking:

* Where in the operation is the adversary?
* What techniques are likely being used here?
* What should we expect to see next?
* What should have happened before this?
* Which controls, detections or telemetry should help us?

That makes the Unified Kill Chain useful as a hunting framework, an investigative notebook and a way to structure reports.

> I use the Unified Kill Chain as a structured notebook. If I can place the activity in the operation and map the behaviour to ATT&CK, I usually get a clearer picture of what the adversary is trying to achieve.
>
> -- Roger Johnsen

## What the Unified Kill Chain Is

The Unified Kill Chain groups adversary activity into three broad phases:

* **In**
* **Through**
* **Out**

These phases represent a simple way of thinking about intrusion progression:

| Phase   | High-level meaning                                             |
| ------- | -------------------------------------------------------------- |
| In      | Getting in and establishing a foothold                         |
| Through | Moving through the environment and expanding control           |
| Out     | Maintaining access, completing the mission and creating impact |

This structure is useful because it reflects how real intrusions often unfold. Attackers first gain entry, then deepen their position, and finally pursue the objective. At the same time, the model is more detailed than a simple seven-step chain. The individual steps inside the phases can, with some caveats, be aligned with MITRE ATT&CK techniques and behaviours.

That gives the model two practical strengths:

* it helps describe *where* the adversary is in the operation
* it helps describe *how* the adversary is behaving

Not every attack will move neatly through every step. Some steps will be skipped. Others will repeat. Some behaviours may fit more than one step or phase. That does not weaken the model. It simply means the model should be used as a reasoning aid, not a rigid classification scheme.

In practice, intrusion activity is often iterative rather than simply progressive. An attacker may move through parts of the Through phase several times: discover systems, obtain credentials, move laterally, discover more systems and repeat the cycle. The attacker may also return to the In phase by establishing a new access path, or move from Out back to Through if the first objective reveals new opportunities.

That is why the Unified Kill Chain should not be read as a straight line. It is better understood as a structured way to reason about where observed behaviour fits in an operation.

## The In Phase

The **In** phase is about:

* preparation
* initial compromise
* establishing a foothold

In the Unified Kill Chain, this phase commonly includes steps such as:

* reconnaissance
* resource development
* delivery
* social engineering
* exploitation
* persistence
* defence evasion
* command and control

![Unified Kill Chain In Phase](/images/ukc1.png)

This is the phase where the attacker gets into the environment. Examples of activity in this phase may include:

* gathering information about the target
* setting up phishing infrastructure
* delivering a malicious attachment or link
* persuading a user to act
* exploiting a vulnerability
* using stolen credentials
* establishing a persistence mechanism
* beginning outbound communication to attacker-controlled infrastructure

Initial access does not always involve malware or a delivered payload. In identity-centric environments, the attacker may enter through valid credentials, token theft, OAuth abuse, exposed cloud services or excessive permissions. In those cases, the early part of the operation may be visible in identity logs, SaaS audit logs and cloud control-plane activity rather than traditional endpoint malware telemetry.

The **In** phase is important because it often contains the earliest points where defenders can detect or disrupt the intrusion.

Possible hunting questions:

| Question                                                                 | Possible data                                          |
| ------------------------------------------------------------------------ | ------------------------------------------------------ |
| Are users receiving or interacting with suspicious emails or links?      | Email logs, secure email gateway, proxy, DNS           |
| Are exposed services being probed or exploited?                          | WAF, IDS, firewall, web logs, EDR                      |
| Did delivery lead to execution or suspicious authentication?             | EDR, identity logs, cloud sign-in logs                 |
| Were persistence or defence evasion artefacts created soon after access? | EDR, registry telemetry, task logs, Windows event logs |
| Did a new host or user begin suspicious outbound communication?          | DNS, proxy, firewall, EDR network telemetry            |
| Did a valid identity begin behaving differently after suspicious access? | Identity logs, SaaS audit logs, cloud audit logs, UEBA |

For hunters, the key question is often:

```text
Was this only a suspicious event, or was this the beginning of foothold establishment?
```

## The Through Phase

The **Through** phase is about:

* escalating privileges
* internal reconnaissance
* moving laterally

![Unified Kill Chain Through Phase](/images/ukc2.png)

This is where the attacker moves from having access to having influence. Once the initial foothold is established, the adversary may begin to:

* escalate privileges
* access credentials
* enumerate internal systems
* identify administrators or high-value accounts
* scan internal segments
* move laterally between hosts
* pivot into other identities or systems
* deepen persistence

This phase is especially important in threat hunting because many organisations do not detect the attacker at initial access. They first become visible through strange behaviour inside the environment.

Possible hunting questions:

| Question                                                           | Possible data                                               |
| ------------------------------------------------------------------ | ----------------------------------------------------------- |
| Are users or hosts escalating privileges unusually?                | EDR, identity logs, PAM logs, Windows event logs            |
| Are internal systems being enumerated?                             | EDR, authentication logs, network telemetry, directory logs |
| Is a user or host touching systems outside normal scope?           | EDR, NetFlow, authentication logs, CMDB context             |
| Are credentials being used from unusual hosts?                     | Identity logs, lateral authentication logs, EDR             |
| Are remote administration protocols or tools appearing abnormally? | EDR, network telemetry, PowerShell logs, Windows logs       |

This phase often contains the activity that tells the hunter whether the attacker is trying to go further than the initially compromised asset.

> The Through phase is where many intrusions become clearer. The attacker stops looking like a single event and starts looking like an operation.
>
> -- Roger Johnsen

## The Out Phase

The **Out** phase is about:

* maintaining presence
* completing the mission
* exfiltration and impact

![Unified Kill Chain Out Phase](/images/ukc3.png)

This is the phase where the attacker turns access into outcome. The adversary may:

* maintain persistence
* collect data
* stage data
* exfiltrate information
* deploy ransomware
* sabotage systems
* abuse business processes
* use compromised access for fraud
* impact availability or integrity

Possible hunting questions:

| Question                                                      | Possible data                                               |
| ------------------------------------------------------------- | ----------------------------------------------------------- |
| Is the attacker maintaining access over time?                 | EDR, identity logs, cloud audit logs, persistence telemetry |
| Is sensitive data being collected or staged?                  | File telemetry, DLP, EDR, database logs                     |
| Is data leaving through rare or suspicious channels?          | Proxy, DNS, firewall, cloud storage logs                    |
| Are backups, security tools or recovery paths being targeted? | EDR, admin logs, backup logs, system logs                   |
| Are there signs of destructive activity or business impact?   | EDR, SIEM, audit logs, infrastructure monitoring            |

The **Out** phase is where impact becomes hard to ignore. But from a hunting perspective, the important lesson is not only what the attacker achieved. It is how long the attacker had to get there.

## Why the Model Is Useful for Threat Hunting

The Unified Kill Chain is useful because it helps hunters think about progression, not just isolated alerts. A single suspicious PowerShell process may be interesting, but the real value comes from asking where it fits.

* Is it part of **In** because it follows phishing and execution?
* Is it part of **Through** because it is being used for internal discovery or lateral movement?
* Is it part of **Out** because it is helping stage or exfiltrate data?

The same behaviour may belong to different phases depending on context. PowerShell used immediately after a phishing attachment may belong to the In phase. PowerShell used to enumerate systems or move laterally may belong to the Through phase. PowerShell used to stage data before exfiltration may belong to the Out phase.

The behaviour alone is not enough. The surrounding context decides where it fits. That shift matters. The model helps the hunter move from:

```text
This looks suspicious.
```

to:

```text
This looks suspicious, and it appears to belong to this part of the adversary’s operation.
```

That makes the investigation more structured and makes the output more useful for SOC, IR, DFIR and detection engineering.

## Using the Unified Kill Chain With MITRE ATT&CK

One of the most practical strengths of the Unified Kill Chain is how naturally it works with MITRE ATT&CK. ATT&CK gives the hunter the technical vocabulary to describe what happened. The Unified Kill Chain helps the hunter describe where that behaviour fits in the wider operation.

| Framework          | Main value                                  |
| ------------------ | ------------------------------------------- |
| MITRE ATT&CK       | Describes behaviour and techniques          |
| Unified Kill Chain | Describes progression through the operation |

For example:

| Observation                     | ATT&CK view               | UKC view      |
| ------------------------------- | ------------------------- | ------------- |
| Malicious link sent to user     | Phishing / Initial Access | In            |
| PowerShell launched from Office | Execution                 | In            |
| New scheduled task              | Persistence               | In or Through |
| AD enumeration                  | Discovery                 | Through       |
| PsExec to another server        | Lateral Movement          | Through       |
| Large archive sent externally   | Exfiltration              | Out           |
| Ransomware execution            | Impact                    | Out           |

This is why I find the model useful during hunts. If I can map what I observe to ATT&CK and then place it in the operation, I get a clearer picture of what the adversary has done and what they may do next.

## Using the Unified Kill Chain With the Diamond Model

The Unified Kill Chain works very well together with the Diamond Model. The Diamond Model helps answer:

* Who is involved?
* What infrastructure is being used?
* What capability is being used?
* Who or what is the victim?

The Unified Kill Chain helps answer:

* Where in the operation are we?
* What likely came before this?
* What may come next?

Together, they give the hunter both **relationship structure** and **operational progression**. That combination is useful when writing notes, structuring reports or preparing handover to another team.

## Practical Example: Financial Institution

Consider a simplified intrusion against a financial institution.

### In

| Goal                    | Example activity                                                                                                   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Preparation             | The attackers research finance employees and external services.                                                    |
| Initial compromise      | A phishing email delivers a malicious link or attachment, or a valid account is accessed using stolen credentials. |
| Foothold                | The victim executes content, submits credentials or the attacker begins using the account for continued access.    |
| Persistence and evasion | The attacker establishes a foothold and attempts to avoid detection.                                               |

### Through

| Goal                    | Example activity                                                         |
| ----------------------- | ------------------------------------------------------------------------ |
| Escalate privileges     | The attackers obtain elevated rights.                                    |
| Internal reconnaissance | They enumerate systems, shares, identities and administrator paths.      |
| Move laterally          | They access additional hosts, identities and services to expand control. |

### Out

| Goal                    | Example activity                                                |
| ----------------------- | --------------------------------------------------------------- |
| Maintain presence       | The attackers ensure continued access.                          |
| Complete mission        | They access or stage sensitive data.                            |
| Exfiltration and impact | Data is stolen, or ransomware is deployed to create disruption. |

The value of the model is not only that it describes the attack. It helps the hunter ask what evidence should exist at each phase.

For example:

| Phase   | Hunting question                                                                                                                   |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| In      | Which users received similar phishing messages, which identities showed suspicious access, and what happened after initial access? |
| Through | Which hosts, identities or services were accessed beyond the original victim?                                                      |
| Out     | Was data staged, exfiltrated or impacted after the attacker expanded access?                                                       |

## Detection and Response Use by Phase

The Unified Kill Chain can also be used to organise defensive thinking.

| Phase   | Detection focus                                            | Response focus                                                       |
| ------- | ---------------------------------------------------------- | -------------------------------------------------------------------- |
| In      | Reconnaissance, phishing, delivery, exploitation, foothold | Filtering, hardening, user awareness, rapid containment              |
| Through | Privilege escalation, discovery, lateral movement          | Segmentation, credential hygiene, access control, scope expansion    |
| Out     | Persistence, exfiltration, impact                          | Data protection, backup protection, IR escalation, business recovery |

This is useful because it lets the organisation ask:

* Where would we detect this?
* Where would we contain this?
* Where are we blind?
* Which teams need to act at this phase?

That turns the framework into something operational rather than descriptive. During an active hunt, this mapping can also show which parts of the operation have not yet been investigated. If the team has evidence from the In phase, the hunter can ask whether Through and Out activity followed. If the team has evidence from the Out phase, the hunter can work backwards and ask how the attacker reached that point. This helps avoid narrow investigations where the team only validates the first visible alert.

## How It Extends the Lockheed Martin Kill Chain

The Unified Kill Chain is often presented as an improvement over Lockheed Martin’s Cyber Kill Chain. That is broadly fair, but it is better to be precise. The Lockheed Martin Kill Chain is still useful. It is simpler and easier to teach. It remains a good high-level model for thinking about intrusion progression.

The Unified Kill Chain extends that idea in several useful ways:

| Area              | Lockheed Martin Kill Chain  | Unified Kill Chain                                                     |
| ----------------- | --------------------------- | ---------------------------------------------------------------------- |
| Scope             | High-level intrusion stages | Broader lifecycle across entry, movement and objectives                |
| Detail            | Simpler and more compact    | More detailed and better aligned with modern post-compromise behaviour |
| ATT&CK alignment  | Limited                     | Stronger conceptual alignment                                          |
| Internal movement | Less emphasised             | More explicit                                                          |
| Hunting value     | Good for broad reasoning    | Better for detailed investigations and reporting                       |

So I would not say the Unified Kill Chain makes the older Kill Chain obsolete. I would say it is often more useful when the investigation goes beyond initial access and into internal movement, attacker progression and objective completion.

## Strengths of the Unified Kill Chain

The Unified Kill Chain is useful because it helps defenders:

* reason about the full lifecycle of an operation
* connect ATT&CK mapping to intrusion progression
* think beyond initial compromise
* structure hunts and investigations
* organise notes and reports
* identify detection and visibility gaps
* explain attacker progression to others

It is especially useful when the attacker has already moved beyond the first stage of compromise and the team needs a clearer model for internal progression.

## Limitations of the Unified Kill Chain

The Unified Kill Chain is useful, but it also has limits.

| Limitation            | Why it matters                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------- |
| Complexity            | It is more detailed than simpler models and may be harder to teach quickly.                 |
| Mapping ambiguity     | Some observations can fit more than one step or phase.                                      |
| Modern identity abuse | Some identity-centric and SaaS attacks do not fit neatly into a traditional intrusion flow. |
| Framework dependency  | Poor ATT&CK mapping can weaken how the model is used.                                       |
| Over-structuring risk | Analysts may spend too much time classifying instead of investigating.                      |

As with the other models in this book, the framework should help the analyst reason. It should not pressure the analyst into forcing the evidence into neat boxes.

> A framework is useful when it improves the investigation. If it becomes a paperwork exercise, it has stopped helping.
>
> -- Roger Johnsen

## Working Position for This Book

For this book, I see the Unified Kill Chain as a very practical framework for threat hunting. It helps answer two useful questions at the same time:

* What behaviour are we seeing?
* Where in the adversary’s operation does it fit?

That is why I often use it together with MITRE ATT&CK and the Diamond Model. The Unified Kill Chain gives me a way to structure attacker progression. MITRE ATT&CK gives me the behavioural vocabulary. The Diamond Model gives me the relationship structure. Together, they make a strong notebook for hunting, reporting and further investigation.

## Resources

* [Unified Kill Chain Documentation](https://www.unifiedkillchain.com/assets/The-Unified-Kill-Chain.pdf)
* [Unified Kill Chain](https://www.unifiedkillchain.com/)
* [MITRE ATT&CK](https://attack.mitre.org/)
* [Lockheed Martin Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html)

## Revision

| Revised Date | Comment                                                                                                                                        |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide for using the Unified Kill Chain during threat hunting, ATT&CK mapping and reporting. |
| 2024-10-06   | Improved formatting and wording                                                                                                                |
| 2024-07-07   | Added page                                                                                                                                     |
