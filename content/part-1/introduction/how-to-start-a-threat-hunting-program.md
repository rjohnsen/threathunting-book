---

title: "How to Start a Threat Hunting Program"
description: "How to start and grow a threat hunting programme by establishing foundations, defining objectives, running structured hunts, and turning results into operational improvement."
date: 2024-10-26T16:40:47+02:00
lastmod: 2026-07-09
draft: false
weight: 10
tags:
- fundamentals
- threat hunting
- program
- methodology
keywords:
- threat hunting program
- how to start threat hunting
- threat hunting methodology
- SOC
- detection engineering
- telemetry
- hunting hypotheses
- security operations
- SMART objectives
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**One question I often hear when discussing threat hunting is simple: how do we start a threat hunting programme? The answer depends on the organisation. Some companies already have a mature SOC, good telemetry, clear incident response processes and people who understand the environment. Others are still mostly reactive, with uneven logging, unclear ownership and detections that are difficult to trust. Both may want threat hunting, but they cannot start from the same place.**

That is why the first step is not to copy someone else’s programme. The first step is to understand what you have, what you lack, and what kind of hunting is realistic now.

This article separates the work into two parts.

First, there is the starting point: what must be in place to begin threat hunting in a controlled and useful way. Then there is the next stage: what to do when the organisation has gained momentum and the hunts are starting to produce value.

The distinction matters. Starting a threat hunting programme and maturing a threat hunting programme are related, but they are not the same task. A threat hunting programme should start small enough to work, but structured enough to grow.

---

## Getting Started

### Assess the Current Security Posture

Before starting a threat hunting programme, the organisation should understand its current security posture. This does not need to be a large maturity assessment. The point is to understand what the team can realistically hunt with today.

Start by asking:

| Area          | Question                                                                          |
| ------------- | --------------------------------------------------------------------------------- |
| Assets        | Do we know which systems, users and identities matter?                            |
| Telemetry     | Which data sources are available, searchable and reliable?                        |
| Detection     | Which detections already exist, and what do they miss?                            |
| Triage        | Can the SOC understand and act on hunting output?                                 |
| Response      | Who owns a confirmed finding if the hunt discovers something?                     |
| Documentation | Where will hypotheses, queries, results and lessons be stored?                    |
| Ownership     | Who can fix detections, telemetry gaps or process weaknesses discovered by hunts? |

A weak foundation does not always mean the organisation cannot hunt. It may mean the first hunts will expose missing telemetry, unclear ownership, poor enrichment or weak documentation. That is still useful.

But the team should be honest about the starting point. Threat hunting depends on reality, not ambition.

### Define Purpose and Objectives

A threat hunting programme needs a clear purpose. Different organisations start hunting for different reasons. Some want to find missed malicious activity. Some want to test detection assumptions. Some want to identify telemetry gaps. Some want to improve SOC triage. Some want to connect threat intelligence more directly to operational security.

Those are different purposes, and they may produce different outputs.

| Purpose                        | Example output                                          |
| ------------------------------ | ------------------------------------------------------- |
| Find missed malicious activity | Finding requiring investigation or response             |
| Test detection assumptions     | Confirmation that a behaviour is or is not visible      |
| Identify visibility gaps       | Logging, retention, parsing or coverage requirement     |
| Improve detection              | Detection idea or improved analytic logic               |
| Improve SOC triage             | Better analyst guidance or decision support             |
| Build knowledge                | Baseline, documentation or reusable investigation notes |

Objectives help keep the work grounded. SMART can be useful here: 

* Specific
* Measurable
* Achievable
* Relevant
* Time-bound.

A weak objective is:

```text
Improve threat detection.
```

A better objective is:

```text
During the next quarter, run three hypothesis-driven hunts against behaviours relevant to initial access and identity misuse. Each hunt must document the hypothesis, data sources, results, limitations and at least one output: a finding, detection idea, visibility gap, triage note or refined hypothesis.
```

The point is not to create paperwork. The point is to avoid vague ambition.

> A programme that cannot explain what it is trying to improve will struggle to prove that it is useful.
> 
> -- Roger Johnsen

### Gather the Right Data, Tools and Resources

Threat hunting does not require a perfect toolset to begin, but it does require access to useful data. The team should understand what is available, what is missing and what can be trusted.

| Tooling and data question     | Why it matters                                                 |
| ----------------------------- | -------------------------------------------------------------- |
| What data can we query?       | Defines which hypotheses can be tested now.                    |
| What data is missing?         | Reveals visibility gaps early.                                 |
| How far back can we search?   | Determines whether the time window is sufficient.              |
| How reliable are the fields?  | Affects whether conclusions can be trusted.                    |
| Can we enrich entities?       | Helps connect users, devices, IPs, files and business context. |
| Can we save and repeat hunts? | Supports documentation, review and improvement.                |

A SIEM, EDR, XDR platform, data lake or threat intelligence platform can all help. But none of them creates a hunting capability by itself. When I started out in threat hunting, I had an outdated QRadar installation and a collection of uneven logs. From there, I built a list of what I needed in order to hunt properly.

The company I worked for at the time was not willing to invest much, so I built Python scripts to support the work. Some scripts pulled threat intelligence from online sources. Others enriched events or correlated alerts across tenants in QRadar.

I would not call that the ideal way to build a hunting capability, but it taught me something important: you often start with what you have, not what you wish you had.

> Tooling matters, but it is not the starting point. The starting point is knowing what question you are trying to answer.
>
> -- Roger Johnsen

The best early tool is still a disciplined analyst with a clear question and a place to write down the reasoning.

### Build the First Hunting Team

Threat hunting benefits from a mix of skills. The first hunting team does not need to be large, but it should have enough range to reason across data, systems and security operations. Depending on the organisation, that may include SOC analysts, detection engineers, incident responders, threat intelligence analysts, platform engineers, identity specialists or people with strong systems knowledge.

Useful areas include:

* networking
* endpoint telemetry
* identity and access
* cloud logs
* scripting and automation
* malware and tradecraft
* incident response
* detection engineering
* threat intelligence
* business and asset context

It is rarely necessary for one person to master everything. What matters is that the team can ask useful questions, test assumptions, understand the data, and know when to involve someone else.

I have a simple stance on this:

> Threat hunting is a bit like pair programming. You can do it alone, but the work improves when someone else can challenge the reasoning, validate the logic and ask what the evidence really supports.
>
> -- Roger Johnsen

That does not mean every hunt needs a large team. It means early hunting programmes should create space for peer review, discussion and shared reasoning. One person can hunt. Two people can challenge each other.

### Use Threat Intelligence as Input

Threat intelligence can be a strong input to hunting, but it should not replace thinking.

Threat intelligence may provide:

* adversary behaviours
* known techniques
* infrastructure
* malware characteristics
* targeting patterns
* recent incident examples
* detection ideas
* hypotheses to test locally

But the hunter still has to translate intelligence into the local environment.

A report may say that an actor uses a specific tool. That does not mean the hunt should only search for the tool name or known hashes. The better question is:

```text
What behaviour does this tool or technique create, and can we observe that behaviour here?
```

IOC searches can be useful, especially during incident response or after high-quality reporting. But IOC searching is not the same as threat hunting. Hunting should move from indicators to behaviour, from behaviour to context, and from context to a testable hypothesis.

> Threat intelligence is more than following the news. For hunting, intelligence must become technical data, observable behaviour and something you can query in your own environment.
>
> -- Roger Johnsen

Threat intelligence becomes useful for hunting when it helps the team ask better questions about its own telemetry.

### Develop Testable Hypotheses

A hunt should start with a question that can be tested. Examples of simple hypotheses:

* If there is a sudden increase in outbound traffic to an uncommon destination during non-business hours, it may indicate data staging or exfiltration.
* If a user account experiences many failed logins from different geographic locations within a short time window, it may indicate password spraying or credential abuse.
* If Office applications spawn command interpreters on workstations, it may indicate initial execution from a malicious document.
* If administrative privileges change across multiple accounts within a short period, it may indicate account compromise or misuse.
* If a user logs in from locations inconsistent with normal behaviour, it may indicate credential misuse or session abuse.

A good hypothesis does not need to be perfect. It needs to be testable.

The team should be able to explain:

| Question                           | Purpose                                           |
| ---------------------------------- | ------------------------------------------------- |
| What behaviour are we testing?     | Gives the hunt direction.                         |
| Why does the behaviour matter?     | Connects the hunt to risk or attacker tradecraft. |
| Which data sources can show it?    | Makes the hunt practical.                         |
| What would support the hypothesis? | Helps define evidence.                            |
| What would weaken it?              | Protects against confirmation bias.               |
| What could the output become?      | Connects the hunt to improvement.                 |

Sometimes the hunt proves the hypothesis wrong. Sometimes it finds nothing. Sometimes it reveals that the required data does not exist. That does not automatically mean the hunt failed. A hunt may still produce a detection idea, a visibility gap, a baseline or a better question.

### Conduct the First Hunts

The first hunts should be small, scoped and realistic. Avoid starting with vague goals such as:

```text
Find advanced persistent threats.
```

or:

```text
Look for suspicious activity.
```

Those are too broad and too hard to evaluate.

Better first hunts are specific enough to test:

```text
Identify Office applications spawning command interpreters on managed workstations during the last 30 days.
```

or:

```text
Identify cloud accounts with repeated failed logins from shared infrastructure followed by successful authentication.
```

Good first hunts often have these qualities:

| Quality               | Why it matters                                                  |
| --------------------- | --------------------------------------------------------------- |
| Clear behaviour       | The team knows what activity it is looking for.                 |
| Available telemetry   | The data exists and can be queried.                             |
| Limited scope         | The hunt can be completed without becoming endless.             |
| Operational relevance | The behaviour matters to the organisation.                      |
| Useful output         | The result can feed SOC, detection, telemetry or documentation. |

The goal of the first hunts is not to cover everything. The goal is to learn how the organisation hunts.

### Example: A Small First Hunt

A first hunt does not need to be large to be useful. Imagine that the team decides to start with one data source: endpoint process telemetry from managed workstations. The objective is limited and practical:

```text
Identify Office applications spawning command interpreters during the last 30 days.
```

The hypothesis is simple:

```text
If malicious documents are used for initial execution, Office applications may spawn PowerShell, cmd.exe, wscript.exe or similar interpreters.
```

The team searches the available endpoint telemetry and discovers three things. First, the behaviour is rare, but it does occur. Second, most occurrences are tied to known administrative packaging work. Third, the data is missing from a subset of unmanaged devices.

That gives the team three useful outputs:

| Output          | Result                                                                                                        |
| --------------- | ------------------------------------------------------------------------------------------------------------- |
| Baseline        | Office-spawned command interpreters are rare on managed workstations.                                         |
| Triage guidance | Known packaging hosts and administrative users should be treated differently from ordinary user workstations. |
| Visibility gap  | Unmanaged devices do not provide enough endpoint process telemetry to test the hypothesis properly.           |

The hunt may not find an attacker. It still improves the organisation. The team now knows more about normal behaviour, can give the SOC better triage guidance, and has a concrete telemetry gap to raise with the platform owners.

That is a good first hunt.

### Document and Share Findings

Documentation is what turns hunting from individual effort into organisational knowledge.

At minimum, document:

| Item              | Purpose                                              |
| ----------------- | ---------------------------------------------------- |
| Hypothesis        | Explains what was tested.                            |
| Reason            | Explains why the hunt mattered.                      |
| Scope             | Defines systems, users, time range and data sources. |
| Queries or method | Makes the hunt repeatable.                           |
| Results           | Shows what was observed.                             |
| Limitations       | States what the data could not prove.                |
| Conclusion        | Separates evidence from interpretation.              |
| Output            | Defines what should happen next.                     |

This does not need to be complicated. A notebook, Markdown file, case template or internal wiki can be enough.

The important part is that the reasoning survives the hunt.

> The best tools a threat hunter can have are a brain and a place to write down what that brain is doing.
>
> -- Roger Johnsen

Documentation also helps with communication. Stakeholders do not need every query detail, but they do need to understand what was tested, what was found, what remains uncertain and what should happen next.

---

## Once the Programme Has Momentum

### Build Feedback Loops

Once the first hunts start producing output, the programme should build feedback loops. A hunt should not end with a report only. It should feed the work around it.

| Hunt output                    | Possible owner                        |
| ------------------------------ | ------------------------------------- |
| Confirmed finding              | SOC or incident response              |
| Detection idea                 | Detection engineering                 |
| Visibility gap                 | Platform, logging or engineering team |
| Triage guidance                | SOC                                   |
| Threat intelligence refinement | CTI or detection team                 |
| Baseline                       | SOC, hunting team or knowledge base   |
| Better question                | Future hunt backlog                   |

This is where threat hunting becomes part of security operations rather than a separate activity. If no one owns the output, the programme will struggle. Findings will be interesting, but they will not improve anything.

### Iterate and Improve

A threat hunting programme improves through repetition. After each hunt, review:

* Did the hypothesis make sense?
* Did we have the required data?
* Did the query test what we thought it tested?
* Did we document the reasoning well enough?
* Did the output reach the right owner?
* Did anything improve afterwards?
* What should we hunt next?

This creates the improvement cycle. Threat hunting should make the organisation better at hunting, but also better at monitoring, detection, triage and response. The programme has momentum when hunts no longer feel like isolated activities. They become part of how the organisation learns.

### Measure What Matters

Metrics are useful, but weak metrics can damage a hunting programme. Counting the number of hunts is easy. Counting the number of findings is easy. Neither tells the full story.

Better measures include:

* visibility gaps identified and closed
* detection ideas handed over and implemented
* detections improved because of hunts
* triage guidance created
* repeatable hunts documented
* hypotheses tested
* assumptions confirmed, weakened or replaced
* time from hunt finding to operational improvement

Avoid measuring threat hunting only by confirmed compromises. A hunt that finds no attacker but reveals a missing log source may be more valuable than a hunt that produces a dramatic report but changes nothing.

The question should be:

```text
What improved because we hunted?
```

That question is more useful than asking how many hunts were performed.

### Expand the Hunting Scope

Early hunts should be narrow. Once the team gains confidence, the scope can expand.

Expansion may include:

* more data sources
* longer time windows
* more business units
* cloud and SaaS telemetry
* identity-focused hunts
* endpoint-to-network correlation
* threat intelligence-led hunts
* detection validation hunts
* purple team follow-up hunts
* hunts based on incident lessons learned

The scope should expand because the team has learned enough to handle it, not because the programme needs to look mature. A wider scope without better process usually creates more noise.

### Improve Detection Engineering Handover

As the programme matures, the handover to detection engineering should become sharper.

A weak handover says:

```text
Suspicious PowerShell activity was observed.
```

A useful handover says:

```text
Office applications spawning PowerShell with encoded command-line arguments were rare in this environment. Where observed, the activity should be reviewed with user context, parent process, command-line content and follow-on network activity.
```

Detection engineering needs behaviour, logic, context and test cases. It does not need vague suspicion.

A good handover should describe:

* the behaviour
* why it matters
* where it was observed
* how common it is
* which fields are needed
* what false positives may occur
* what triage guidance should accompany the detection

This is how hunting turns into durable security improvement.

### Engage With the Threat Hunting Community

Threat hunting improves when practitioners learn from each other. Read DFIR reports. Study detection write-ups. Follow incident case studies. Join communities where people discuss tradecraft, telemetry, false positives, detection logic and hunting methods. Community engagement should not be passive consumption. The useful part is not only reading what others found. It is asking:

```text
Can we test this in our environment?
```

and:

```text
What would this behaviour look like in our telemetry?
```

This turns external learning into local hunting value.

### Review and Adapt Objectives

Once the programme has momentum, the original objectives should be reviewed.

Some may still be useful. Others may need to change because the team has learned more about the environment.

Review:

* Are the objectives still relevant?
* Are hunts producing useful outputs?
* Are outputs reaching the right owners?
* Are visibility gaps being closed?
* Are detections improving?
* Are SOC analysts getting better guidance?
* Are new hypotheses emerging from previous hunts?
* Are we measuring value or just activity?

This is where SMART objectives can be revisited. The first objectives helped start the work. Later objectives should help mature it.

The programme should adapt as the organisation learns.

---

## What Usually Goes Wrong

Several patterns repeat when organisations try to start and grow threat hunting:

* **Programme before practice:** the organisation creates a formal programme before it has run small, useful hunts.
* **Tool-first hunting:** the team starts with a platform instead of a question.
* **No telemetry reality check:** hypotheses are written without checking whether the data exists.
* **IOC-only hunting:** the work becomes indicator searching instead of behaviour-driven investigation.
* **No owner for output:** findings, gaps and detection ideas have nowhere to go.
* **No documentation:** each hunt becomes a one-off activity that cannot be repeated or improved.
* **Metrics theatre:** the programme is measured by activity count rather than operational improvement.
* **Stalled momentum:** the first hunts produce output, but the organisation never builds feedback loops or ownership around them.

These problems are ordinary. They are also avoidable.

Start small. Ask clear questions. Test what can be tested. Document what happened. Make sure something improves. Then build the programme around the evidence that the work is useful.

## Working Position for This Book

A threat hunting programme should not start as a large organisational ceremony. It should start as a repeatable way to ask better questions and turn the answers into better security work.

The first version can be small:

```text
One purpose.
One objective.
One hypothesis.
One dataset.
One documented result.
One owner for the output.
```

That is enough to start.

Once the programme has momentum, the work changes. The organisation should build feedback loops, measure what matters, expand scope carefully, and improve how hunting feeds SOC, detection engineering, incident response and telemetry improvement.

Or as I usually put it:

> Do not start by pretending you have a mature threat hunting programme. Start by proving that structured hunts can improve something, then build the programme around that evidence.
>
> -- Roger Johnsen

## Resources

* [MITRE ATT&CK](https://attack.mitre.org/)
* [Threat Hunting: A Practical Guide](https://www.cisecurity.org/white-papers/threat-hunting-a-practical-guide/)
* [SANS Threat Hunting Resources](https://www.sans.org/white-papers/34847/)
* [Threat Hunting with Splunk](https://www.splunk.com/en_us/blog/security/threat-hunting-using-splunk.html)
* [Verizon Data Breach Investigations Report](https://enterprise.verizon.com/resources/reports/dbir/)
* [The Threat Hunting Process: A Practical Guide](https://www.cybintsolutions.com/threat-hunting-process-guide/)
* [Incident Response and Threat Hunting](https://www.cyber.gov.au/acsc/view-all-content/publications/incident-response-threat-hunting)
* [Hunt for Threats: The Art and Science of Threat Hunting](https://www.csoonline.com/article/3569268/hunt-for-threats-the-art-and-science-of-threat-hunting.html)
* [Threat Hunting: Analyzing Security Events](https://www.microsoft.com/security/blog/2020/06/16/threat-hunting-analyzing-security-events/)
* [Open Threat Hunting Framework](https://www.openthreat.hunting)

## Revision

| Revised Date | Comment                                                                                                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-09   | Rewritten to establish a clearer practitioner voice, separate programme start-up from programme momentum, and align the page with the book’s fundamentals section |
| 2024-10-26   | Added page                                                                                                                                                        |
