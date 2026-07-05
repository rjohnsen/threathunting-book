---
title: "Threat Hunt Book by Predefender"
date: 2024-06-22T10:25:28+02:00
draft: false
---

Threat hunting is not about running random queries and hoping something suspicious appears.

It is about understanding behaviour, building context, forming hypotheses, testing assumptions, and turning observations into knowledge the organisation can use.

This book is a practical guide to how threat hunters think, investigate, document, and improve detection capability.

-- Roger C.B. Johnsen

---

## What This Book Is

Threat Hunt Book is a practical handbook for threat hunting. It is written from the perspective of someone who has worked hands-on with SOC operations, investigations, detection, and threat hunting, and who has also helped establish SOC capabilities for Norwegian cybersecurity firms. The goal is not to provide a perfect academic model of threat hunting. The goal is to share practical methods, field notes, mental models, and lessons learned from real operational security work.

This book is about how to think during a hunt:

* how to move from observation to context
* how to build and test hypotheses
* how to understand security data
* how to reason about attacker behaviour
* how to document findings
* how to turn hunting knowledge into better detection
* how to avoid common analytical traps

Threat hunting is not a tool. It is a discipline.

---

## Who This Book Is For

This book is primarily written for threat hunters and security analysts who want to become better at structured investigation.

It is also relevant for:

* SOC analysts who want to move beyond alert handling
* detection engineers who want to understand how hunting findings become detection logic
* security architects who need to understand how detection, telemetry, and response fit together
* incident responders who need better context during investigations
* defenders who want to understand how attacker behaviour appears in real environments

The aim is simple: help analysts become more effective threat hunters by understanding the method behind the work.

---

## How to Use This Book

This is not meant to be read only from beginning to end. Use it as a practical reference. Read the methodology chapters to understand the thinking. Use the framework pages to compare hunting models. Use the field notes and technique pages when you need examples. Use the cheat sheets when you need quick reminders during investigations.

Some articles are conceptual. Some are practical. Some are reflective. Together, they describe how I approach threat hunting as a discipline.

If you are new to threat hunting, start with:

* [Definition](/part-1/introduction/definition/)
* [Analyst Mindset](/part-1/introduction/analystmindset/)
* [Context Before Conclusion](/part-1/introduction/context-before-conclusion/)
* [Creating Hypothesis](/part-1/introduction/creating-hypothesis/)
* [Planning a Threat Hunt](/part-1/introduction/planning-a-threat-hunt/)

If you are interested in detection engineering, start with:

* [Hunter to Detection](/part-6/hunter-to-detection/)

---

## About the Content

The content includes articles, notes, presentations, cheat sheets, field reflections, and practical guidance based on years of hands-on work in threat hunting, SOC operations, detection, and investigations.

Some pages are polished articles. Others are practical references or field notes. The common thread is that the content should help defenders understand what they are looking at, why it matters, and what to do next.

The content has been refined with the help of tools like ChatGPT to improve grammar, clarity, structure, and readability. The ideas, experience, opinions, and editorial direction are my own.

---

## Feedback

I welcome thoughts, suggestions, corrections, and professional disagreement.

If something is unclear, wrong, outdated, or useful, feel free to reach out via [my preferred contact links](/about/contact/).

---

## Latest Updates

### Jul. 05, 2026

**New article:** [Context Before Conclusion](/part-1/introduction/context-before-conclusion/)
An article about building context around an observation before reaching a conclusion.

### Jun. 28, 2026

**New section:** [Detection Engineering](/part-6/)
A new section covering how threat hunting findings can contribute to detection engineering.

**New article:** [Hunter to Detection](/part-6/hunter-to-detection/)
A practical look at the threat hunter's role in detection engineering, including the difference between hunt queries, detection candidates, and operational detection logic.

### Jun. 27, 2026

**New article:** [MAC Addresses](/part-4/mac-addresses/)
A practical look at MAC addresses, OUI lookups, MAC randomization, and how Layer 2 context can prevent mistakes during investigations.

### Oct. 18, 2025

**New article:** [From Alerts to Hypothesis](/part-1/introduction/from-alerts-to-hypothesis/)
How to move from alert-driven triage toward hypothesis-driven threat hunting.

### Apr. 20, 2025

**New section:** [Threat Hunting Frameworks](/part-1/frameworks/threathunting/)
Collected threat hunting framework content under one dedicated section.

### Apr. 13, 2025

**New framework pages:** [Magma](/part-1/frameworks/threathunting/magma/), [TaHiTI](/part-1/frameworks/threathunting/tahiti/), and [PEAK](/part-1/frameworks/threathunting/peak/)
Added practical references for several established threat hunting frameworks.

### Mar. 29, 2025

**New page:** [When to Engage Threat Hunters](/part-1/introduction/when-to-engage-threat-hunters/)
Guidance on when threat hunters should be brought into security work, investigations, and operational decisions.

### Mar. 21, 2025

**New cheat sheets:** [Kusto Sentinel Tables](/part-5/kusto-sentinel-tables/), [Sysmon](/part-5/sysmon/), [Windows Registry](/part-5/windows-registry/), and [PowerShell](/part-5/powershell/)
Quick references based on recent hunts and common investigation needs.

---

## Earlier Changes

### Mar. 16, 2025

* Added new [Cheat Sheets](/part-5/) section.
* Added support for tags to make grouping articles easier. Testing started in the Cheat Sheets section.

### Feb. 28, 2025

* Updated contact information.
* Added e-mail.

### Feb. 22, 2025

* Overhauled multiple pages to improve clarity and meaning.
* Added [Hierarchy of Needs](/part-1/introduction/hierarchy-of-needs/).

### Feb. 15, 2025

* Added [SIEM Query Languages](/part-4/siem-query-languages/).

### Jan. 11, 2025

* Added [The Threat Hunter Persona](/part-1/introduction/the-threathunter-persona/).

### Dec. 27, 2024

* Added the [Release Plan](/releases/) page, accessible from the left site menu.
* Improved the left site menu by removing chapter and part prefixes.

### Nov. 03, 2024

* Added [No Result Hunts](/part-1/delivieries/no-result-hunts/).

### Nov. 02, 2024

* Added [Creating Hypotheses](/part-1/introduction/creating-hypothesis/).

### Oct. 27, 2024

* Added [T1105](/part-4/mitre/t1105/), based on a recent threat hunt investigation.
* Added [Planning a Threat Hunt](/part-1/introduction/planning-a-threat-hunt/).
* Added [Intelligence Resources](/part-4/intelligence-resources/).

### Oct. 26, 2024

* Added [Starting a Threat Hunting Program](/part-1/introduction/how-to-start-a-threat-hunting-program/).
* Added pages on [Threat Hunting Deliveries](/part-1/delivieries/).

### Oct. 21, 2024

* Revised the introduction for Threat Hunting Deliveries.

### Oct. 20, 2024

* Added a section on Threat Hunting Deliveries.

### Oct. 19, 2024

* Added a section on conditional access for [T1566 - Phishing](/part-4/mitre/t1566/).

### Oct. 15, 2024

* Added a tip on Windows logon events.

### Oct. 13, 2024

* Introduced Deliveries under Part 1, including the new SITREP page.

### Oct. 12, 2024

* Introduced Mitre Field Notes, featuring T1566 - Phishing.

### Oct. 11, 2024

* Added links to social platforms, X and Mastodon.

### Oct. 06, 2024

* Quality assured the OpenSearch Python API ingester.
* Improved document formatting across the site.
* Added tips for Windows event log success and failure.

### Sep. 30, 2024

* Added the [FAQ](/about/faq/) page.

### Sep. 29, 2024

* Added [Understanding Data](/part-3/understanding-data/).

---

![Threat Hunting Illustration](/images/huntbook-raven.png)