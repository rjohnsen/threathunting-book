---
title: "Analyst Mindset"
description: "A practical explanation of the analyst mindset in threat hunting, including thought patterns, structured reasoning, evidence handling, uncertainty, lateral thinking, bias, judgement and communication."
date: 2024-07-14T12:47:36+02:00
lastmod: 2026-07-14
draft: false
hidden: false
weight: 3
tags:
    - introduction
    - foundation
    - mindset
    - thinking
    - analysis
keywords:
    - analyst mindset
    - analytical thinking
    - threat hunting mindset
    - critical thinking
    - lateral thinking
    - structured reasoning
    - evidence handling
    - uncertainty
    - confirmation bias
    - SOC analysis
    - threat hunting
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Threat hunting depends on how the analyst thinks. Tools matter. Telemetry matters. Detections matter. Frameworks matter. But none of them remove the need for human judgement. A threat hunter often works with incomplete evidence, noisy telemetry, unclear ownership, missing context and questions that do not have immediate answers. The work is rarely as clean as a lab example. Logs may be missing, alerts may only show part of the story, users may not remember what happened and system owners may not know what is normal. The analyst still has to make progress, and that progress does not come from collecting facts alone. It comes from turning incomplete evidence into defensible judgement.**

At its simplest:

```text
Curiosity opens the question. Evidence earns the answer.
```

Threat hunting is not about guessing until something interesting appears. It is about asking better questions, testing them against evidence, recognising uncertainty and communicating what can and cannot be concluded. This chapter is about that discipline.

## What Analyst Mindset Means

Analyst mindset is the disciplined way an analyst approaches uncertainty. It is not a personality type, and it is not a gift. It is a set of habits that can be developed, tested and improved through practice.

A useful analyst mindset includes several connected habits.

| Habit | What it means in analysis |
| --- | --- |
| Curiosity | The willingness to ask better questions, look beyond the obvious explanation and follow weak signals carefully. |
| Discipline | The ability to keep the investigation controlled, structured and tied to the question being tested. |
| Scepticism | The habit of challenging weak evidence, unsupported claims, convenient explanations and assumptions. |
| Humility | The ability to accept that the first theory may be wrong and that the evidence may not support the preferred answer. |
| Structured reasoning | The practice of moving from question to evidence to interpretation to judgement in a way another analyst can follow. |
| Source assessment | The ability to judge whether information is relevant, reliable, current and complete enough for the question being asked. |
| Evidence handling | The discipline of separating observations, assumptions, interpretations, conclusions and limitations. |
| Tolerance for uncertainty | The ability to make progress without pretending that the evidence provides perfect certainty. |
| Awareness of bias | The habit of recognising when thinking may be shaped by pressure, fatigue, prior experience, authority or confirmation bias. |
| Communication of judgement | The ability to explain what was found, what it means, how confident the analyst is and what should happen next. |

These habits support each other. Curiosity opens the investigation, discipline keeps it controlled, evidence keeps it grounded and communication makes the result useful.

## The Analyst's Job

Being an analyst is about discovering and explaining situations in a structured, repeatable way that produces plausible, evidence-based answers.

The important words are **structured**, **repeatable**, **plausible** and **evidence-based**. Structured means the analyst follows a method rather than wandering through data. Repeatable means another analyst can understand what was done and why. Plausible means the answer fits the available evidence without pretending to prove more than the evidence supports. Evidence-based means conclusions are earned, not assumed.

A threat hunter may not always be able to prove exactly what happened, but that does not make the work useless. The analyst may still be able to narrow the possibilities, identify what is unlikely, explain what is missing and recommend what should happen next.

## Thinking Is Part of the Work

Thinking is something analysts do all day, but it is easy to treat thinking as invisible. It should not be invisible, because the way an analyst thinks affects what questions are asked, which evidence is trusted, which assumptions are challenged and which conclusions are communicated.

Different situations require different modes of thinking.

| Thinking mode | How it helps analysis |
| --- | --- |
| Sequential thinking | Organises observations and actions in a logical order. |
| Concrete thinking | Focuses on what was actually seen, heard, logged or reported. |
| Abstract thinking | Connects concrete observations to broader concepts, patterns or risks. |
| Holistic thinking | Considers the wider situation instead of only the event in front of the analyst. |
| Lateral thinking | Looks for indirect connections and alternative explanations. |
| Critical thinking | Tests whether the conclusion is supported by evidence. |

These modes are not separate boxes. Real analysis moves between them. An analyst may use concrete thinking to describe an authentication event, sequential thinking to build a timeline, lateral thinking to consider an unexpected path, critical thinking to challenge the first theory and holistic thinking to decide whether the activity matters in the wider environment.

The goal is not to label every thought. The goal is to notice how thinking shapes the work.

## Thinking Can Be Contaminated

Thinking is part of the analyst's toolset, and like any tool it can be damaged, misused or contaminated. Bias, pressure, fatigue and assumptions can change how evidence is interpreted long before the analyst writes a conclusion.

Common sources of analytical contamination include:

| Factor | How it affects analysis |
| --- | --- |
| Confirmation bias | The analyst selects evidence that supports the first theory. |
| Alert tunnel vision | The analyst only investigates what the alert already says. |
| Authority bias | A claim is accepted because it came from a senior person, vendor or customer. |
| Availability bias | Recent incidents make similar explanations feel more likely than they are. |
| Tool bias | The analyst believes the tool's view is the whole truth. |
| Fatigue | Poor sleep, stress or overload reduces analytical quality. |
| External noise | Chat, pressure, assumptions or rumours shape the investigation before evidence does. |
| Insecurity | The analyst avoids clear judgement because the topic feels unfamiliar. |

Bias cannot be removed by pretending to be objective. It must be managed. The analyst can reduce contamination by writing down the hypothesis, looking for evidence against it, separating facts from assumptions, stating confidence level, documenting limitations, asking what would change the conclusion and allowing another analyst to review the reasoning.

The analyst should not try to be bias-free. The analyst should make the reasoning visible enough that bias can be challenged.

## Thinking Is Verbing

Thinking is not passive. Analytical work is full of verbs, because an analyst does not only “look at logs”. The analyst performs actions that shape understanding.

| Verb | Analytical meaning |
| --- | --- |
| Identify | Find the issue, question, event, entity or behaviour that needs attention. |
| Break down | Split a large problem into smaller questions that can be tested. |
| Gather | Collect relevant evidence from useful sources. |
| Assess | Judge the quality, meaning and limitations of the evidence. |
| Compare | Place the observation against a baseline, peer group, timeline or expectation. |
| Connect | Look for relationships between events, entities, systems and behaviours. |
| Develop | Build possible explanations or next investigative steps. |
| Test | Check whether the evidence supports or weakens a hypothesis. |
| Review | Revisit the reasoning when new evidence appears. |
| Communicate | Make the judgement usable by others. |

These verbs are not decoration. They are the work. They also help the analyst avoid vague activity. If the analyst cannot describe what they are doing as an action, the work may have become unfocused.

## Thinking in Questions

Good analysis usually begins with a question, and weak questions often produce weak analysis.

For example:

```text
Is this bad?
```

That question is usually too broad. A better question may be:

```text
Is this behaviour expected for this user, device, account, application and time period?
```

Other useful questions may be:

```text
What would need to be true for this activity to be benign?

What evidence would change our assessment?
```

Threat hunting often improves when the analyst moves from vague suspicion to testable questions. Questions such as “What happened?”, “Who or what performed the action?”, “Compared to what?”, “What evidence contradicts this?” and “What should happen next?” are simple, but they force the analyst to work with evidence rather than mood, confidence or habit.

## Evidence, Interpretation and Judgement

Analysts must separate evidence from interpretation. Evidence is what was observed, interpretation is what the analyst thinks the evidence may mean, and judgement is the analyst's defensible assessment based on the available evidence, context and uncertainty.

A useful model is:

```text
Evidence → Interpretation → Judgement → Action
```

For example:

```text
Evidence:
A service account authenticated interactively from a standard user workstation.

Interpretation:
This behaviour does not match the documented baseline for the account.

Judgement:
The activity should be treated as suspicious until account ownership, source
device context and maintenance activity are verified.

Action:
Review the account owner, source host, recent account changes, related alerts
and approved maintenance windows.
```

This is stronger than simply writing:

```text
The service account was compromised.
```

The compromise statement may turn out to be true, but it is not earned by the evidence alone. The stronger version keeps the observation, interpretation, judgement and action separate, which makes the reasoning easier to review. A good analyst knows the difference between what is observed, what is inferred and what is proven.

The same principle applies to peer review. A judgement should be open to challenge, because analysis that cannot be challenged is too private to be reliable. Another analyst should be able to see the evidence, understand the interpretation and question the conclusion. That does not mean every conclusion must be wrong until debated. It means the reasoning should be visible enough that another analyst can test it, improve it or reject it.

A simple rule of thumb is:

```text
If it cannot be challenged, it is not analysis.
```

## Plausible, Not Certain

Security work often happens under uncertainty. An analyst may not have complete logs, perfect timelines, full packet capture, user testimony, clean asset ownership or reliable baselines. Waiting for perfect certainty may mean doing nothing.

The goal is not always certainty. The goal is defensible judgement. A defensible judgement should explain what was observed, what the analyst thinks it means, how confident the analyst is, what the limitations are and what should happen next. This matters because overconfidence can be dangerous, but so can paralysis.

The analyst must be able to say:

```text
We do not know enough to confirm compromise.
```

But also:

```text
We know enough to justify further investigation.
```

Both statements can be true at the same time.

## Analytical Thinking

Analytical thinking is the process of breaking a problem into parts, examining the evidence, identifying relationships and developing a reasoned assessment.

In security operations, analytical thinking often includes identifying the issue, breaking it into smaller questions, gathering relevant evidence, assessing source quality, looking for relationships, developing possible explanations, testing those explanations, reviewing what changed and communicating the result.

This is not a rigid sequence. Real investigations loop. New evidence may force the analyst back to an earlier question. A weak hypothesis may be discarded. A finding may become a new hunt.

The important point is that analysis should not be random. The analyst should be able to explain the path from question to evidence to judgement.

## Critical Thinking

Critical thinking is what prevents analysis from becoming storytelling. It challenges claims, checks assumptions and asks whether the conclusion is stronger than the evidence allows.

Useful critical thinking questions include:

* Why do I believe this?
* What evidence supports it?
* What evidence contradicts it?
* Is there a simpler explanation?
* Is there a benign explanation?
* Is the data complete enough?
* Is the source reliable?
* Am I repeating someone else's assumption?
* What would I expect to see if this theory were true?
* What would I expect to see if it were false?

Critical thinking does not mean rejecting everything. It means refusing to accept weak conclusions too easily.

## Lateral and Creative Thinking

Threat hunting also requires creative thinking. Attackers do not always behave like playbooks, alerts or lab exercises. They may use legitimate tools, unusual paths, trusted accounts, timing, misdirection or environmental knowledge.

The analyst sometimes needs to imagine what could be happening before the evidence is obvious. That imagination is useful, but it must be controlled. Lateral thinking is the ability to approach a problem indirectly and look for connections that are not immediately obvious. It helps the analyst avoid being trapped by the most visible explanation. The analyst may ask what could explain the activity if the obvious explanation is wrong, how an attacker could achieve the same result without triggering the alert, or what the activity would look like if it were staged through a trusted account.

Creative thinking opens possible paths, while critical thinking decides which paths deserve evidence. That balance matters. Imagination can help the analyst find a path that the tool did not present, but it can also lead the analyst into fiction if it is not tested.

A useful way to think about it is freedom under responsibility. The analyst may imagine several explanations, but only evidence can decide which explanation deserves confidence.

## Analytical Exercise: The Lighthouse Problem

The following exercise is a classic lateral thinking problem. It is useful because the answer depends on indirect clues, not explicit information.

```text
It is almost midnight. There is a storm outside. A man sits inside watching TV.
The TV signal comes and goes. The man gets bored, turns off the light and goes
to bed. The next morning he reads in the newspaper that 40 men died outside his
door during the night.

What happened, and what is the man's occupation?
```

The answer is:

```text
He is a lighthouse keeper. He turned off the lighthouse light, which caused a
ship to run aground or sink during the storm.
```

The point is not the riddle. The point is the reasoning. Some facts are noise, some are clues, and some words carry hidden assumptions. The first explanation may be wrong. The analyst must combine concrete evidence, lateral thinking and critical testing.

| Clue | Analytical value |
| --- | --- |
| Almost midnight | Light matters more at night, and night increases the consequence of poor visibility. |
| Storm outside | Suggests dangerous conditions and opens the possibility of sea, navigation or travel-related risk. |
| A man sits inside | The man is isolated from what is happening outside, but his actions may still affect it. |
| Watching TV | A normal activity that makes the situation appear domestic and ordinary. |
| TV signal comes and goes | Reinforces bad weather and possible isolation, but may also be a distractor. |
| He gets bored | Explains why he stops what he is doing, but not why the next action matters. |
| Turns off the light | The light is the central clue. It is mentioned because it has significance beyond the room. |
| Goes to bed | The action appears routine to him, which suggests he does not immediately understand the consequence. |
| The next morning | The consequence is discovered later, which fits delayed awareness rather than direct violence. |
| Reads it in the newspaper | The event is public news, not something he personally witnessed. |
| 40 men died | Suggests a group incident rather than a single accident, such as a crew, passengers or workers. |
| Last night | Connects the deaths to the same time window as the storm and the light being turned off. |
| Right outside his door | The location is unusual unless “door” belongs to a special place, such as a lighthouse near the sea. |
| His occupation is asked | The question signals that the man's role is part of the explanation. |

This resembles security analysis more than it first appears. A log line can work the same way. The important question is not only what the event says, but what it implies, what it omits and what must be true for the explanation to hold.

## Source Assessment

Analysts rarely work from a single source. They may use SIEM events, EDR telemetry, identity logs, firewall logs, email data, asset inventories, vulnerability data, threat intelligence, user reports, system owner input, vendor documentation and public reporting.

Not all sources have the same value. Source assessment means asking whether the information is relevant, reliable, current and complete enough for the question being asked.

Useful questions include:

* Where did this information come from?
* When was it produced?
* What does this source actually show?
* What does it not show?
* Is the source authoritative for this question?
* Could the source be incomplete?
* Could the source be wrong?
* Does another source confirm or contradict it?

A log source may be technically accurate but incomplete. A user report may be honest but imprecise. A vendor alert may be useful but generic. Threat intelligence may be relevant globally but not locally. The analyst must understand the strength and weakness of each source before using it to support a conclusion.

## Traces, Links and Evidence

Threat hunting is closely related to the idea that actions leave traces.

Dr. Edmond Locard, the French criminologist and pioneer of forensic science, is associated with the principle:

> Every contact leaves a trace.
>
> -- Edmond Locard, commonly known as Locard's exchange principle

In security work, the same idea appears everywhere. A process starts, a user authenticates, a file is written, a connection is made, a permission changes, a token is issued, a command runs and a log is generated, or should have been generated. The hunter looks for traces of contact.

Traces alone are not enough. The analyst must also understand relationships.

In *A Study in Scarlet*, Arthur Conan Doyle lets Sherlock Holmes describe this chain of reasoning:

> All life is a great chain, the nature of which is known whenever we are shown a single link of it.
>
> -- Sherlock Holmes, in Arthur Conan Doyle's *A Study in Scarlet*

The quote is useful because analysis often begins with one link. A single event may lead to an account, a device, a process, a parent process, a destination, a file, a user action, a business process or another system. The analyst follows the link, but must not invent the chain.

There is also an old legal phrase that fits analytical work:

> Res ipsa loquitur.
>
> -- Latin legal phrase meaning “the thing speaks for itself”

For analysts, this is both useful and dangerous. Evidence should be allowed to speak, and the analyst should not force it into a preferred story. At the same time, evidence rarely explains itself completely. A log event does not come with intent attached. A failed logon does not explain whether it was attack, error, automation or stale credentials.

The analyst must let the evidence speak without pretending that it says more than it does.

## Communication Is Part of Analysis

Analysis is not finished when the analyst understands the case. It is finished when the result can be used.

That may mean updating a case, writing a report, creating a detection candidate, briefing the SOC, informing incident response, documenting a baseline or explaining uncertainty to a manager.

Good communication should make the analyst's reasoning visible. It should explain what was investigated, what was found, what was not found, what it means, how confident the analyst is, what limitations exist and what should happen next.

Poor communication can waste good analysis. A hunter who finds something important but cannot explain it clearly may fail to create action. A hunter who overstates weak evidence may create false confidence or unnecessary escalation.

Communication is not decoration. It is part of the analytical work.

## Adaptation and the OODA Loop

Analysis changes as new information appears. A good analyst must be willing to revise the assessment when the evidence changes.

This is where the OODA loop is useful:

```text
Observe → Orient → Decide → Act
```

The analyst observes evidence, orients around context, decides what the evidence supports and acts through investigation, escalation, documentation or closure. Then new information appears, and the loop starts again.

Being wrong is not always the worst outcome. Staying wrong after the evidence changes is worse. A useful mindset allows the analyst to restart the loop without treating revision as failure.

## Common Failure Modes

Analytical work often fails in predictable ways.

| Failure mode | Why it hurts |
| --- | --- |
| Alert tunnel vision | The analyst only investigates what the alert already says. |
| Confirmation bias | Evidence is selected to support the first theory. |
| Premature closure | The case is closed before uncertainty is resolved. |
| Tool-first thinking | The analyst asks what the tool can show instead of what needs to be known. |
| Weak source assessment | Untrusted or incomplete information is treated as fact. |
| No hypothesis discipline | The analyst jumps between ideas without testing them. |
| Overconfidence | The conclusion is stronger than the evidence supports. |
| Analysis paralysis | The analyst waits for certainty when a defensible next step is already available. |
| Poor communication | Good analysis fails to become useful action. |
| No learning loop | The same analytical mistake repeats because nothing is documented or improved. |

The most common failure is treating analysis as a private mental activity. Analysis should leave a trail so another analyst can understand what was asked, what was checked, what was concluded and why.

## How Mindset Connects to Threat Hunting Outputs

Analyst mindset is not separate from the rest of the work. It shapes the quality of every threat hunting output. A weak mindset creates weak baselines, noisy detections, shallow audits and reports nobody can use. A strong mindset improves the way the analyst asks questions, handles evidence, tests assumptions and communicates judgement.

| Output | How analyst mindset matters |
| --- | --- |
| Baseline | The analyst must understand what is expected without assuming that expected means benign. |
| Detection rule | The analyst must decide whether behaviour is worth alerting on and whether the SOC can act on it. |
| Audit | The analyst must be willing to question whether something still works. |
| Report | The analyst must explain what was found, what it means, what is uncertain and what should happen next. |
| Hunt | The analyst must turn curiosity into structured, testable investigation. |

Mindset is therefore not an abstract quality. It is visible in the outputs the analyst leaves behind.

## Working Position for This Book

For this book, analyst mindset is treated as a practical operating discipline.

It is the way an analyst asks questions, handles evidence, manages uncertainty, challenges assumptions and communicates judgement. A useful analyst mindset is curious, but not careless; creative, but not speculative; sceptical, but not paralysed; confident, but not overconfident.

The analyst earns conclusions through evidence and makes the reasoning visible enough for others to review, use and improve.

That is the standard. Not because analysts need to sound clever, but because security decisions should not depend on guesswork, habit or untested assumptions.

## Resources

* [Locard's Exchange Principle](https://en.wikipedia.org/wiki/Locard%27s_exchange_principle)
* [Arthur Conan Doyle: A Study in Scarlet](https://www.gutenberg.org/files/244/244-h/244-h.htm)
* [Res ipsa loquitur](https://en.wikipedia.org/wiki/Res_ipsa_loquitur)
* [The OODA Loop](https://en.wikipedia.org/wiki/OODA_loop)
* [Confirmation Bias](https://en.wikipedia.org/wiki/Confirmation_bias)
* [Critical Thinking](https://en.wikipedia.org/wiki/Critical_thinking)
* [Lateral Thinking](https://en.wikipedia.org/wiki/Lateral_thinking)
* [MITRE ATT&CK](https://attack.mitre.org/)
* [Splunk SURGe: PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html)

## Revision

| Revised Date | Comment |
| --- | --- |
| 2024-10-06 | Improved formatting and wording. |
| 2026-07-14 | Rewritten to focus on analyst mindset, structured reasoning, evidence handling, uncertainty, bias, communication and common analytical failure modes. IOA, IOB and IOC material moved to a separate article. |