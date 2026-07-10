---
title: "OODA Loop"
description: "A practical explanation of the OODA Loop and how threat hunters can use it to structure observation, orientation, decision-making and action during investigations."
date: 2024-06-22T10:16:57+02:00
lastmod: 2026-07-10
draft: false
weight: 5
tags:
   - frameworks
   - threat hunting
   - OODA loop
   - decision making
keywords:
   - OODA Loop
   - Observe Orient Decide Act
   - threat hunting
   - SOC
   - decision making
   - incident response
   - cyber defence
   - security operations
   - John Boyd
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**The OODA Loop is a decision-making framework built around four activities: Observe, Orient, Decide and Act.**

The model is commonly associated with United States Air Force Colonel John Boyd and was originally developed in a military context. It is often used to explain how people and organisations make decisions in competitive, uncertain and fast-moving environments.

Personally, I think the OODA Loop is one of those models that is far more useful than its original context suggests. It can be used in military strategy, business, sports, security operations, incident response, detection engineering, threat hunting and ordinary daily life. Any time you have to understand a situation, make sense of incomplete information, choose what to do and then adapt based on the result, you are already close to OODA thinking.

That is why the model works well in security. A SOC analyst does not simply receive an alert and press a button. A threat hunter does not simply run a query and declare an answer. A detection engineer does not simply write a rule because a technique exists in ATT&CK. All of these roles depend on the same basic movement:

```text
Observe what is happening.
Orient around context.
Decide what to do next.
Act in a way that creates new information or improves the situation.
Repeat.
```

This is the real value of the OODA Loop. It reminds us that good security work is not a straight line from signal to conclusion. It is an iterative process where each action should improve the next observation.

> The OODA Loop is useful because it reminds the analyst that investigation is not a straight line. Every action should create new observations.
>
> -- Roger Johnsen

## What the OODA Loop Is

The OODA Loop is often shown as a cycle.

![OODA loop](/images/ooda.png)

Courtesy of [online.visual-paradigm.com](https://online.visual-paradigm.com/knowledge/decision-analysis/what-is-ooda-loop/1000)

The four parts are:

| Step    | Meaning                                             |
| ------- | --------------------------------------------------- |
| Observe | Gather information from the environment.            |
| Orient  | Interpret the information in context.               |
| Decide  | Choose what to do next.                             |
| Act     | Carry out the decision and create new observations. |

The model looks simple, but the real value is not in memorising the four words. The value is in understanding the movement between them.

Observation without orientation is just data collection. Orientation without decision becomes endless analysis. Decision without action produces no effect. Action without renewed observation becomes blind movement.

The loop matters because every action changes the situation in some way. A query produces new results. A containment action changes attacker behaviour. A conversation with a system owner adds business context. A false positive teaches something about normal behaviour. Each of those outcomes feeds back into the next loop.

That makes OODA useful for threat hunting, incident response, detection engineering and everyday security operations.

## Observe

Observe means gathering information from the environment. In threat hunting and SOC work, observations may come from many places:

* alerts
* process telemetry
* command lines
* authentication logs
* DNS queries
* proxy logs
* firewall logs
* cloud audit logs
* email logs
* endpoint telemetry
* user reports
* threat intelligence
* asset and identity context

But observation is not the same as understanding. This is an important distinction. A suspicious command line is an observation. A rare domain is an observation. A failed login pattern is an observation. An EDR alert is an observation. None of those things explain themselves.

The analyst still has to understand what was observed, where it came from, how reliable it is and what else may be needed before any conclusion makes sense.

Useful questions in the Observe phase include:

| Question                                       | Why it matters                                |
| ---------------------------------------------- | --------------------------------------------- |
| What exactly did we observe?                   | Prevents vague investigation notes.           |
| Which data source produced it?                 | Helps assess reliability and completeness.    |
| Is the observation complete?                   | Avoids conclusions based on partial evidence. |
| Is this rare, suspicious, expected or unknown? | Starts the orientation process.               |
| What else should we observe before deciding?   | Prevents premature action.                    |

A weak observation is often too vague:

```text
Suspicious PowerShell.
```

A better observation is more specific:

```text
WINWORD.EXE spawned powershell.exe with encoded arguments on a finance workstation shortly after the user opened an attachment.
```

The second version gives the analyst something to work with. It includes process context, command-line context, host context, user context and timing. That makes the next step possible.

## Orient

Orient is the most important part of the loop. This is where the analyst turns observation into context. It is also where experience, environment knowledge, threat intelligence and scepticism matter. In security work, orientation may include:

* understanding the user
* understanding the host
* understanding the business process
* checking asset criticality
* comparing against baseline behaviour
* reviewing related telemetry
* considering threat intelligence
* testing alternative explanations
* mapping behaviour to ATT&CK or another framework
* deciding whether the evidence actually supports suspicion

This is where many investigations succeed or fail. If the analyst orients badly, the rest of the loop becomes weak. Benign activity may become a false incident. Malicious activity may be dismissed because it does not match the expected pattern. A noisy alert may be closed too quickly, or a weak signal may be over-escalated without enough evidence.

For example:

| Observation                   | Poor orientation     | Better orientation                                                                                                                        |
| ----------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| PowerShell launched by Office | Malicious PowerShell | Office-spawned PowerShell is rare in this environment and should be reviewed with user, command-line, parent process and network context. |
| Login from foreign country    | Account compromised  | Check user travel, VPN, impossible travel, device, MFA result and session behaviour.                                                      |
| Large file transfer           | Data exfiltration    | Check destination, user role, business process, file type, time, volume and approval context.                                             |
| New scheduled task            | Persistence          | Review creator, command, path, timing, host role and whether it matches software deployment activity.                                     |

Orientation is also where the other frameworks in this book become useful.

MITRE ATT&CK may help describe the behaviour. The Diamond Model may help structure the relationship between adversary, infrastructure, capability and victim. The Kill Chain and Unified Kill Chain may help place the behaviour in a larger intrusion progression. OODA is different because it describes the analyst’s decision process rather than the adversary’s behaviour.

> Observation tells you what appeared. Orientation helps you understand what it may mean.
>
> -- Roger Johnsen

## Decide

Decide means choosing what to do next based on the current understanding. This does not always mean making a final conclusion. In threat hunting, a decision is often much smaller and more practical. It may simply be the next investigative move. Possible decisions include:

* pivot to related hosts
* review parent and child processes
* check network activity
* enrich an indicator
* search for similar behaviour
* validate with the asset owner
* escalate to SOC or incident response
* create a detection candidate
* document a false positive pattern
* close the lead as benign
* continue the hunt with a refined hypothesis

The decision should match the evidence. If the evidence is weak, the right decision may be to gather more context. If the evidence is strong, the right decision may be to escalate. If the evidence points to benign activity, the right decision may be to document the pattern and improve future triage.

A useful decision is explicit:

```text
Search for the same Office-spawned PowerShell pattern across all endpoints for the last 30 days.
```

A weak decision is vague:

```text
Investigate more.
```

The difference matters. A clear decision can be reviewed, repeated and challenged. A vague decision usually leads to wandering.

## Act

Act means carrying out the decision. In security work, actions may include:

* running a query
* pivoting across telemetry
* enriching data
* reviewing endpoint evidence
* contacting a system owner
* escalating to incident response
* creating a detection
* updating a playbook
* documenting a finding
* suppressing known benign activity
* recommending a control improvement

The action should do something useful. It should produce new information, reduce uncertainty, improve detection, improve response or change the situation in a deliberate way.

For example:

| Decision                                    | Action                              | New observation                                           |
| ------------------------------------------- | ----------------------------------- | --------------------------------------------------------- |
| Search for similar PowerShell behaviour     | Run query across endpoint telemetry | Three additional hosts show the same pattern.             |
| Validate whether activity is administrative | Contact system owner                | Activity matches software deployment script.              |
| Check for C2 after execution                | Review DNS and proxy logs           | Endpoint contacted rare external domain after execution.  |
| Improve future detection                    | Draft detection logic               | Candidate rule identifies rare Office-spawned PowerShell. |

This is why the loop repeats. The result of the action becomes a new observation. That new observation changes the next orientation. The next orientation changes the next decision. This is the discipline of OODA in practice.

## OODA in Daily Life

One reason I like the OODA Loop is that it is not limited to security work. People use this pattern all the time, often without naming it. You observe that traffic is heavier than usual. You orient by considering the time of day, weather, route options and whether you need to be somewhere at a specific time. You decide to take another route. You act, and then you observe whether that decision helped.

The same pattern appears in conversations, planning, problem solving, leadership, incident handling and technical troubleshooting.

This matters because OODA is not an artificial process forced onto the analyst. It describes a natural decision cycle and gives us a way to make that cycle more deliberate. In stressful environments, that can be valuable.

When pressure increases, people often skip orientation. They observe something, decide too quickly and act. Sometimes that works. Often it creates unnecessary noise, wrong conclusions or avoidable damage. OODA is a reminder to keep the full loop intact.

## The Loop in Threat Hunting

For threat hunters, the OODA Loop can be written as:

```text
Observe → Orient → Decide → Act → Repeat
```

A simple hunt may move through the loop many times.

Example:

| Loop | Observe                               | Orient                                           | Decide                                 | Act                           |
| ---- | ------------------------------------- | ------------------------------------------------ | -------------------------------------- | ----------------------------- |
| 1    | EDR alert for suspicious PowerShell   | Office spawned PowerShell on finance workstation | Search for similar behaviour           | Query endpoint telemetry      |
| 2    | Three more hosts show similar pattern | Same parent process, similar attachment timing   | Review email delivery                  | Search email logs             |
| 3    | Same sender targeted multiple users   | Possible phishing campaign                       | Check network activity after execution | Review DNS and proxy logs     |
| 4    | One host contacted rare domain        | Potential command and control                    | Escalate for containment and deeper IR | Hand over structured findings |

This is not a straight path. The hunter learns something, adjusts and continues. The value is not speed alone. The value is reducing uncertainty while maintaining enough tempo to remain useful.

## OODA and Tempo

The OODA Loop is often associated with speed, but speed alone is not the point. A fast wrong decision is still wrong.

In security operations, the goal is not simply to move faster than the adversary. The goal is to observe better, orient faster, decide with enough confidence and act in a way that changes the situation.

Sometimes that means acting quickly. Sometimes that means slowing down long enough to avoid a bad conclusion.

For example, disabling an account may be the right action if there is strong evidence of compromise. But if the evidence is weak and the account belongs to a critical business process, the team may need more context before acting.

Tempo is useful movement. Panic is just movement.

> Moving fast is not the same as thinking well. The defender needs tempo, but the tempo must be connected to evidence.
>
> -- Roger Johnsen

## OODA in the SOC

In a SOC, the OODA Loop appears constantly.

| OODA step | SOC example                                                                       |
| --------- | --------------------------------------------------------------------------------- |
| Observe   | Alert fires, user reports phishing, EDR records suspicious behaviour.             |
| Orient    | Analyst reviews context, asset, user, telemetry, history and threat intelligence. |
| Decide    | Analyst chooses to close, investigate, escalate, contain or enrich.               |
| Act       | Analyst performs the chosen action and documents the result.                      |

The quality of SOC work often depends on the quality of orientation. If analysts are forced to move directly from alert to action without context, the SOC becomes reactive and noisy. If analysts observe and orient well, their decisions become more reliable.

The OODA Loop also helps explain why playbooks matter. A good playbook does not remove judgement. It helps the analyst orient and decide faster by showing what evidence to collect, what questions to ask and what actions are available.

## OODA in Detection Engineering

OODA also applies to detection engineering. Detection engineering is not only about writing rules. It is a decision process where the engineer observes adversary behaviour, orients around available telemetry, decides what logic may detect the behaviour and acts by building, testing or tuning detection content.

A simple detection engineering loop might look like this:

| OODA step | Detection engineering example                                                                                           |
| --------- | ----------------------------------------------------------------------------------------------------------------------- |
| Observe   | Threat intelligence describes a technique, or a hunt identifies suspicious behaviour.                                   |
| Orient    | Engineer reviews telemetry, data quality, expected noise and environment-specific context.                              |
| Decide    | Engineer chooses whether to build detection logic, improve enrichment, create a hunt query or document a telemetry gap. |
| Act       | Engineer builds, tests, tunes or documents the detection approach.                                                      |

This is important because detection engineering can fail in the same way investigations fail. If the engineer observes a technique but does not orient around local telemetry, the result may be a noisy rule, a brittle rule or a rule that cannot work in the environment.

Good detection engineering is OODA with engineering discipline.

## OODA in Threat Hunting

Threat hunting is usually less urgent than incident response, but it still benefits from OODA thinking. A hunt begins with an observation, hypothesis, intelligence report, detection gap or question. The hunter orients by understanding the environment and the behaviour being tested. The hunter then decides how to test the idea and acts by querying data, pivoting, enriching and validating.

The loop repeats until the hunt reaches a useful output. That output may be:

* a confirmed finding
* a rejected hypothesis
* a detection candidate
* a visibility gap
* a triage improvement
* a control recommendation
* a better understanding of normal behaviour

This matters because a threat hunt should not be a long browsing session through logs. Each loop should improve the investigation. If the loop does not produce better understanding, the hunt is probably drifting.

## Where OODA Fits With Other Frameworks

The OODA Loop works differently from the other frameworks in this section.

| Framework                  | Main value                                                                         |
| -------------------------- | ---------------------------------------------------------------------------------- |
| Lockheed Martin Kill Chain | Describes intrusion progression and disruption opportunities.                      |
| Unified Kill Chain         | Describes broader adversary progression and operational phases.                    |
| MITRE ATT&CK               | Provides behavioural vocabulary for adversary techniques.                          |
| Diamond Model              | Structures relationships between adversary, infrastructure, capability and victim. |
| OODA Loop                  | Structures decision-making under uncertainty.                                      |

The other frameworks help describe the adversary, the intrusion or the evidence. OODA helps describe the analyst’s process.

That makes it useful as a mental model for investigations, SOC work, detection engineering and daily decision-making. It helps the analyst avoid two common failure modes: getting stuck in observation without decision, and acting without orientation.

## What Usually Goes Wrong

Several problems appear when teams use OODA poorly.

| Problem                     | Why it hurts                                                               |
| --------------------------- | -------------------------------------------------------------------------- |
| Observing without orienting | The analyst collects data but does not understand what it means.           |
| Orienting with weak context | The analyst makes decisions based on incomplete or misleading assumptions. |
| Deciding too early          | The team acts before the evidence supports the action.                     |
| Never deciding              | The investigation becomes endless analysis without output.                 |
| Acting without feedback     | The team takes action but does not observe whether it changed anything.    |
| Treating speed as the goal  | The team moves quickly but makes poor decisions.                           |
| Ignoring documentation      | Later loops lose the reasoning behind earlier decisions.                   |

The loop should improve decision quality. It should not become a slogan for rushing.

## Working Position for This Book

For this book, the OODA Loop is best treated as a decision-making model for threat hunters and security analysts.

It helps explain how an investigation moves from observation to context, from context to decision, and from decision to action. It also helps explain why good security work is iterative. The analyst should not expect the first observation to be enough, or the first decision to be final.

The model is simple:

```text
Observe.
Orient.
Decide.
Act.
Repeat.
```

But the discipline is in how the analyst uses it. Observe carefully. Orient with context. Decide explicitly. Act in a way that creates new information or improves security. Then repeat the loop with what you have learned.

> A good hunt is not only about what the hunter finds. It is also about how the hunter thinks, decides and adapts while the hunt is unfolding.
>
> -- Roger Johnsen

## Resources

* [OODA Loop - Wikipedia](https://en.wikipedia.org/wiki/OODA_loop)
* [Farnam Street: OODA Loop - Understand it, Apply it](https://fs.blog/ooda-loop/)
* [The OODA Loop Explained](https://www.artofmanliness.com/character/knowledge-of-men/ooda-loop/)
* [Visual Paradigm: What is OODA Loop?](https://online.visual-paradigm.com/knowledge/decision-analysis/what-is-ooda-loop/1000/)

## Revision

| Revised Date | Comment                                                                                                                                                                                                   |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide for using the OODA Loop during daily decision-making, SOC analysis, detection engineering, threat hunting and decision-making under uncertainty. |
| 2024-10-06   | Improved formatting and wording                                                                                                                                                                           |
| 2024-06-22   | Added page                                                                                                                                                                                                |
