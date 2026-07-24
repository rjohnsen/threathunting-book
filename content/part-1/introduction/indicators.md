---
title: "Indicators"
description: "A practical explanation of indicators of compromise, attack and behaviour, and how threat hunters use indicators to build context, form hypotheses and improve detections."
date: 2026-07-14T00:00:00+02:00
lastmod: 2026-07-14
draft: false
hidden: false
weight: 5
tags:
    - indicators
    - IOC
    - IOA
    - IOB
    - threat hunting
    - baselines
    - detection engineering
keywords:
    - IOC
    - IOA
    - IOB
    - indicators of compromise
    - indicators of attack
    - indicators of behaviour
    - threat hunting indicators
    - behavioural indicators
    - baselines
    - detection engineering
---

**Author:** *Roger C.B. Johnsen*

## Introduction

**Indicators are not answers. They are signals that help analysts decide what to investigate next. In SOC and threat hunting documentation, the word indicator is often used as if it only means IOC: IP addresses, domains, hashes, file names, registry keys and other concrete artefacts. Those indicators matter, but they are only one part of the picture. Threat hunting also depends on recognising attack-relevant activity and behaviour that differs from what is expected in the local environment.**

Threat hunting often begins with something incomplete. An IP address appears in traffic, a file hash appears in an alert, a process behaves strangely, a user account logs in from a new source, a service account performs an unexpected action or a pattern deviates from a baseline. These observations may be useful, but they do not all mean the same thing.

Some indicators point to evidence that compromise may already have happened. Some describe activity that may be part of an attack. Some reveal behaviour that differs from what is expected.

This is where IOC, IOA and IOB become useful. Not as rigid boxes, but as analytical lenses.

At its simplest:

```text
IOC → What evidence suggests compromise?
IOA → What activity suggests an attack may be happening?
IOB → What behaviour differs from what is expected?
```

The labels are less important than the analytical question behind them. An indicator is usually a question in disguise: where else did this appear, is it expected here, what happened before it, what happened after it, and does this signal matter locally?

## Why This Distinction Matters

Many security teams are comfortable with IOCs because they are concrete. A hash can be searched. A domain can be blocked. An IP address can be added to a watchlist. These actions are useful, especially during incident response or retrospective searching, but they can also create a narrow view of detection and hunting.

David Bianco's Pyramid of Pain captures this problem well. Hashes, IP addresses and domains sit low in the pyramid because they are relatively easy for adversaries to change. Higher up the pyramid are network artefacts, host artefacts, tools and tactics, techniques and procedures, where detection starts to affect how the adversary operates rather than only which artefacts they used.

That does not mean IOCs are worthless. It means they are incomplete when used alone.

A threat hunter should be able to move between different kinds of signal:

| Signal type | What it gives the analyst |
| --- | --- |
| IOC | A concrete trace or artefact to search for, enrich or block. |
| IOA | Activity that may indicate an attack path, sequence or intent-like pattern. |
| IOB | Behaviour that differs from what is expected for an entity, system or peer group. |

Different signals support different kinds of work. An IOC may support scoping. An IOA may support attack-path analysis. An IOB may support anomaly-driven hunting and baseline improvement. Treating all of them as “IOCs” hides those differences.

## What an Indicator Is

An indicator is an observable signal that may help an analyst understand, investigate or act on security-relevant activity.

That definition is intentionally broader than IOC.

An indicator may be a static artefact, such as a hash, domain, URL or registry key. It may be a sequence of actions, such as discovery commands followed by lateral movement attempts. It may also be a behavioural deviation, such as a service account logging on interactively from a user workstation when the documented baseline shows non-interactive application use only.

A useful indicator should help the analyst answer at least one practical question:

* What did we observe?
* Why does it matter?
* Is it expected here?
* What context is missing?
* What should we investigate next?
* Can this improve a baseline, hunt or detection?

The value of an indicator is not only whether it can be matched. The value is whether it helps the analyst build context and make a better decision.

## IOC: Indicator of Compromise

An Indicator of Compromise, or IOC, is evidence that may indicate that a system, account, network or environment has been compromised.

IOCs are often concrete artefacts. They include IP addresses, domain names, URLs, file hashes, file paths, registry keys, mutexes, command-line fragments, email headers, malicious attachments, suspicious certificates and known malware artefacts.

IOCs are useful because they are searchable, shareable and often easy to operationalise. They can support incident response, retrospective searching, enrichment, blocking, alert triage and threat intelligence sharing. When a team needs to answer “where else did this specific thing appear?”, IOCs are often the right starting point.

The weakness is that IOCs are easy to misuse. An IP address may be shared infrastructure. A domain may change ownership. A file hash may represent one specific sample and miss every modified version. A suspicious command line may be approved administration in one environment and malicious in another.

A useful IOC should therefore be interpreted in context. The analyst should know where and when it was observed, which asset or account was involved, whether there was execution or only presence, whether the indicator is still valid and what happened before and after it appeared.

An IOC is often a starting point. It should not be the whole investigation.

## IOA: Indicator of Attack

An Indicator of Attack, or IOA, describes activity that may indicate an attack is happening, developing or being prepared.

IOAs are usually closer to behaviour than static artefacts. They often describe suspicious sequences, relationships or activity patterns rather than a single object. Examples include repeated failed logons followed by a successful logon, discovery commands executed by a user process, unexpected lateral movement attempts, a web server communicating with internal hosts, unusual process spawning or sudden access to sensitive data.

An IOA does not need to prove that an attack succeeded. Its value is that it helps the analyst recognise that activity may be moving in an attack-relevant direction.

For example:

```text
A service account performs an interactive logon from a standard user workstation,
then accesses systems it normally reaches only through application workflows.
```

That may not be confirmed compromise by itself, but it is attack-relevant behaviour. It combines account type, logon method, source system and follow-on access into a pattern that deserves investigation.

IOAs require context because a single event may not be enough. The analyst needs to understand the sequence, what changed before the activity, what happened after it, whether the behaviour aligns with known attack techniques and whether the activity is expected for the account, host or application.

IOAs help the analyst move from artefact matching to behaviour investigation.

## IOB: Indicator of Behaviour

An Indicator of Behaviour, or IOB, describes behaviour or behavioural deviation.

IOB is closely related to baselining because behaviour only becomes meaningful when compared against something. The analyst needs some idea of what is expected before a deviation can be interpreted.

Examples include a user accessing systems outside normal working patterns, a service account authenticating from a new source host, a workstation communicating with a new internal peer group, a server transferring more data than usual, an account using an unusual authentication method or an application contacting destinations outside its normal pattern.

An IOB does not automatically mean malicious activity. It means the behaviour deserves interpretation.

For example:

```text
A Finance-owned service account normally authenticates non-interactively from
two known application servers. It suddenly performs an interactive logon from a
standard user workstation.
```

The behaviour is notable because it differs from the baseline. It may indicate credential misuse, human use of a service account, undocumented maintenance, application change or logging error. The analyst must investigate the context before deciding what it means.

Baseline deviation is not the same as malicious activity.

That distinction matters. A weak baseline can make normal behaviour look suspicious, or suspicious behaviour look normal. IOBs are powerful because they make local context visible, but they are only as useful as the baseline and entity context behind them.

## IOC, IOA and IOB Compared

The difference between IOC, IOA and IOB is not always clean. The same observation may fit more than one lens depending on how it is used.

Still, the distinction is useful.

| Type | Main question | Typical focus | Example |
| --- | --- | --- | --- |
| IOC | What evidence may indicate compromise? | Artefact or trace | A known malicious hash appears on an endpoint. |
| IOA | What activity may indicate an attack? | Sequence or behaviour associated with attack activity | A user process launches discovery commands and connects to multiple internal hosts. |
| IOB | What behaviour differs from what is expected? | Deviation from baseline or peer behaviour | A service account logs on interactively from a workstation where the baseline shows non-interactive application use only. |

A simple way to think about the relationship is:

```text
IOC → trace
IOA → attack-relevant activity
IOB → behavioural deviation
```

These are lenses, not boxes. A command line may be an IOC if it matches a known malicious fragment from a previous incident. It may be an IOA if it forms part of a suspicious execution sequence. It may be an IOB if it is unusual for the user, host or peer group.

The analyst should not argue over the label longer than necessary. The useful question is what the indicator helps the analyst understand or do.

## Indicators and the Pyramid of Pain

The Pyramid of Pain is useful because it explains why not all indicators have the same defensive value.

At the lower levels, indicators such as hashes, IP addresses and domains are useful but often easy for adversaries to change. They are still worth collecting and using, but they tend to support matching, scoping and short-term response more than long-term disruption.

Higher up the pyramid, the focus moves towards artefacts, tools and TTPs. This is where IOA and IOB become important. They help the team look beyond static traces and ask better questions about activity, behaviour and operating patterns.

| Pyramid-style focus | Related indicator lens | Practical use |
| --- | --- | --- |
| Hashes, IP addresses, domains | IOC | Search, enrich, block, scope and retrospectively review. |
| Host and network artefacts | IOC / IOA | Identify traces of activity and recurring technical patterns. |
| Tools | IOA | Detect how tools behave, not only the exact files or hashes they use. |
| TTPs and operating patterns | IOA / IOB | Detect behaviour, sequence, intent-like activity and local deviation. |

This does not mean the analyst should ignore IOCs. It means the analyst should not stop there.

A mature hunting practice uses IOCs as leads, IOAs as behaviour patterns and IOBs as local context. The higher the team moves from artefact to behaviour, the more the work depends on understanding the environment.

## When Indicators Are Not Enough

Indicators are useful, but they are rarely enough by themselves.

An IOC may tell the analyst what appeared, but not why it appeared. An IOA may describe attack-relevant activity, but not whether the activity succeeded. An IOB may show deviation from expected behaviour, but not whether the deviation is malicious.

Indicators may be insufficient when:

* the signal has no local context
* the indicator is stale or overbroad
* the behaviour requires baseline comparison
* the SOC has no useful action
* the alert depends on reputation alone
* the indicator explains what appeared, but not what happened

In those cases, the analyst should treat the indicator as a lead, not a conclusion.

## From Indicator to Context

Context is what turns an indicator from a label into a lead.

A single indicator may answer very little. A hash tells the analyst that a specific file was seen. It does not explain how it arrived, whether it executed, which account was involved, what happened next or whether other systems show related activity.

Useful context may include asset role, account type, account owner, user behaviour, peer group, business unit, time of day, source and destination, process tree, authentication method, data volume, recent changes, related alerts, known maintenance, baseline behaviour and threat intelligence.

For example:

```text
Indicator:
Service account interactive logon from a workstation.

Context:
The account is owned by Finance, normally authenticates non-interactively from
two application servers, has access to a business-critical database and has no
documented maintenance workflow from user workstations.

Assessment:
The activity is suspicious and should be investigated as possible credential
misuse unless account ownership or change records explain it.
```

The indicator alone is not enough. The context makes it meaningful.

## From Indicator to Hunt

A hunt begins when an indicator becomes a question.

A known IOC may lead to retrospective searching. An IOA may lead to a hypothesis about attacker behaviour. An IOB may lead to an anomaly-driven hunt. Threat intelligence may lead the analyst to ask whether reported behaviour appears locally. A baseline deviation may lead the analyst to ask whether the behaviour is isolated or part of a broader pattern.

| Starting point | Hunt question |
| --- | --- |
| IOC | Where else has this hash, domain, URL or command fragment appeared? |
| IOA | Are there other accounts showing the same attack-like sequence? |
| IOB | Are there other service accounts behaving outside their baseline? |
| Threat intelligence | Does this reported behaviour appear in our environment? |
| Baseline deviation | Is this rare behaviour isolated or part of a broader pattern? |

A good hunt does not stop at matching the indicator. It asks what the indicator may reveal.

For example:

```text
Weak hunt:
Search for this IP address.

Stronger hunt:
Search for this IP address, identify related accounts and hosts, review what
happened before and after the connection, and look for similar outbound patterns
to related infrastructure.
```

The second hunt uses the IOC as a starting point, not as the whole method.

## From Indicator to Detection

Indicators can also support detection engineering, but indicators should not bypass the detection quality test.

Even a strong indicator still needs the same questions as any other detection candidate. Can the behaviour be observed reliably? Does it matter locally? Can the SOC triage it? What false positives are expected? What baseline or context is required? Who owns the detection? When should it be reviewed?

Examples:

```text
IOC-based detection:
Alert when a known malicious hash is executed.

IOA-based detection:
Alert when a user process launches discovery commands and then initiates
connections to multiple internal hosts.

IOB-based detection:
Alert when a scoped service account performs an interactive logon from a
standard user workstation where the baseline shows non-interactive use only.
```

The IOC-based detection may be high confidence but brittle. The IOA-based detection may be more behavioural but require more tuning. The IOB-based detection may be locally powerful but depends on baseline quality.

A detection is not good because it contains an indicator. It is good when it creates useful, reviewable work.

## Indicators and Baselines

Baselines give indicators something to be compared against.

This is most obvious for IOBs, but all indicator types benefit from baseline context. Without a baseline, analysts may overreact to normal behaviour or miss abnormal behaviour that does not match a known IOC.

For example:

```text
Observation:
An internal host connects to an external storage provider.

Without baseline:
This may or may not matter.

With baseline:
This peer group does not normally use external storage providers, and the
connection occurred shortly after unusual authentication from a new device.
```

The same event becomes more meaningful when compared to expected behaviour.

Baselines do not make indicators unnecessary, and indicators do not make baselines unnecessary. They support each other. Indicators help analysts notice signals. Baselines help analysts decide whether those signals are expected.

## Indicator Quality

Indicators vary in quality.

A useful indicator should be specific enough to guide investigation, but not so narrow that it only finds one known sample. It should be reliable enough to support analysis, but not treated as truth without context.

Useful quality factors include:

| Quality factor | Why it matters |
| --- | --- |
| Specificity | Broad indicators create noise. |
| Reliability | Weak or stale indicators mislead analysts. |
| Context | Indicators without context are easy to misuse. |
| Timeliness | Some indicators decay quickly. |
| Observability | The environment must be able to see the behaviour or artefact. |
| Actionability | The indicator should support triage, hunting, detection or response. |
| Reviewability | Analysts should understand where the indicator came from and why it matters. |

A hash may be highly specific but easy to evade. A behaviour may be harder to evade but more difficult to tune. A baseline deviation may be powerful locally but meaningless without good entity context.

Indicator quality is not only a technical property. It depends on how the indicator is used.

## Common Failure Modes

Indicator-driven work often fails in predictable ways.

| Failure mode | Why it hurts |
| --- | --- |
| IOC tunnel vision | The analyst only searches for known artefacts and misses related behaviour. |
| Indicator without context | The team cannot explain whether the signal matters locally. |
| Stale indicators | Old infrastructure, domains or hashes create noise or false confidence. |
| Overbroad indicators | The search returns too much activity to interpret. |
| Overfitted indicators | The detection only matches one sample or lab scenario. |
| Baseline confusion | Normal behaviour is treated as suspicious because expected behaviour is unknown. |
| Behavioural overreach | A deviation is treated as malicious without enough evidence. |
| No source assessment | Indicators from weak or unknown sources are treated as reliable. |
| No operational path | The indicator creates alerts or hunts but no useful decision. |
| No feedback loop | Findings are not turned into better baselines, detections or intelligence. |

The most common mistake is treating an indicator as an answer.

An indicator is usually a question in disguise.

## Working Position for This Book

For this book, indicators are treated as analytical starting points.

An IOC may provide a trace. An IOA may describe attack-relevant activity. An IOB may reveal behaviour that differs from what is expected. None of them should replace judgement.

A useful analyst uses indicators to build context, form hypotheses, test assumptions, improve baselines and create better detections.

That is the standard. Not because acronyms matter, but because security work improves when the team understands what kind of signal it is working with.

## Resources

* [David Bianco: The Pyramid of Pain](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html)
* [MITRE ATT&CK](https://attack.mitre.org/)
* [MITRE ATT&CK: Data Sources](https://attack.mitre.org/datasources/)
* [MITRE ATT&CK: Valid Accounts](https://attack.mitre.org/techniques/T1078/)
* [MITRE ATT&CK: Account Discovery](https://attack.mitre.org/techniques/T1087/)
* [CISA: Indicators of Compromise](https://www.cisa.gov/news-events/news/indicators-compromise)
* [Mandiant: Indicators of Compromise](https://www.mandiant.com/resources/blog/indicators-of-compromise)
* [MISP Project: Decaying Indicators of Compromise](https://arxiv.org/abs/1803.11052)
* [Sigma: Generic Signature Format for SIEM Systems](https://sigmahq.io/)
* [Splunk SURGe: PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html)

## Revision

| Revised Date | Comment |
| --- | --- |
| 2026-07-14 | Initial version. Split from the Analyst Mindset article and rewritten to explain IOC, IOA and IOB as practical analytical concepts for threat hunting, baselining and detection engineering. |
| 2026-07-14 | Reworked the article to place IOC, IOA and IOB in analytical context, reduce dictionary-style structure, add Pyramid of Pain framing and strengthen the role of context, baselines and detection quality. |
