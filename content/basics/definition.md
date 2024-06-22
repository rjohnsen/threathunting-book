---
title: "Platform"
date: 2024-06-22T11:37:31+02:00
draft: false
---

## Definition

> Threat hunting is an iterative and proactive process whereby threat hunters seek out anomalous activity, artifacts, and 
> behaviors within an environment with the objective of identifying previously unknown and undetected threats. This definition has 
> two critical components:
>
> **Repeatable**
>
> A hunt (the commonly accepted term for activity carried out by these teams) has value in its execution, but only for the duration 
> of its execution. Once the hunt is complete, any subsequent malicious activity may remain unidentified. Therefore, hunts need to 
> be carried out in an iterative fashion based on the prevalence of the technique, and the relative risk to the organization.
> 
> **Threat hunting must be proactive**
> 
> The objective of threat hunting is, ultimately, to identify previously undetected malicious activity in an environment. This 
> objective is accomplished through a variety of analysis methods, especially those involving behavioral and statistical analysis. 
> This process, however, absolutely does not rely on searching through an environment using atomic indicators of compromise 
> (IOC). That practice belongs strictly to the domain of traditional security operations, not threat hunting.
> *– [Cyborg Threat Hunting Framework](https://www.cyborgsecurity.com/library/white-paper/threat-hunting-framework/)*

## Reasons why for Threat Hunting

| Why | What |
| --- | ---- |
| Proactively Uncover Security Incidents | Become aware of threats that are lurking or hidden but active. |
| Improve the Speed of Threat Response | Threat hunting’s course of action is searching through networks for indicators of abnormal behavior caused by potential attacks; this entails a human-driven process designed to look for the threats that automated systems or conventional detection methods might miss |
| Reduce Investigation Time | Better insights into and understanding of environment |
| Aid Cyber Security Analysts in Understanding the Company | Provides analysts a much better overall picture of the current state of the organization’s security, and its expected resilience to a variety of attacks |
| Help Achieve Appropriate Mitigation of Threats With an Improved Defense System | Identify defense systems to further secure environment
| Brings SOCs Into the Future | Providing aide and support to SOC, advanced, detection rules to SOC and Broaden detection surface |
| Reduces False Positives and Improves SOC Efficiency | Hunting’s strength is that it is human-driven, proactive, iterative and analytical. This combination of tools, repetitive monitoring and behavior-pattern searching, together with the analysts’ ingenuity and ability to examine and evaluate data, means a reduction in false positives and time-wasting.
| Reduces Damage and Overall Risk to the Organization| Threat hunting offers a faster response and a proactive approach, which normally means less possibility for malicious intruders and threats to damage to an organization, its systems and data. |

And not to forget, the importance of understanding the Organization’s [Threat-Discovery Maturity](https://www.infosecinstitute.com/resources/threat-hunting/10-benefits-of-threat-hunting/):

{{<mermaid align="left">}}
flowchart TD
    A["
       LEVEL 0 - INITIAL

       Relies primarily on automated alerting
       Little or no routine data collection
    "]

    B["LEVEL 1 - MINIMAL
       
       Incorporates threat intelligence indicator searches
       Moderate or high level of routine data collections
    "]
    
    C["LEVEL 2 - PROCEDURAL
       Follows data analysis procedures created by others
       High or very high level of routine data collection
    "]

    D["LEVEL 3 - INNOVATIVE
       Creates new data analysis procedures
       High or very high level of routine data collection
    "]
    
    E["LEVEL 4 - LEADING
        Automates the majority of successful data analysis procedures
        High or very high level of routine data collection
    "]

    A --> B --> C --> D --> E
{{< /mermaid >}}

## Differences between Threat Detection and Threat Hunting

Threat Detection and Threat Hunting are two separate discipline’s and a distinction is thus in place in order to avoid confusion:

| Threat Detection | Threat Hunting |
| ---------------- | -------------- |
| Threat Detection is reactive. There’s a wait for security systems to detect occurrences before acting/investigation can happen. | Is proactive. Threat Hunting is actively looking for and investigates suspicious artifacts and circumstances. |
| Threat Detection reacts to “knowns”. Things must be known and detection rules must be written and implemented prior to detection. |Lifts unknowns to known state - then provide detection for it. |
| Detects on pure technical grounds - can’t judge human behavior other than statistical aspect. | The human aspect of detection / interpretation | 
| Driven by Indicators sof Compromise (IOC) | Driven by Tactics, Techniques, and Procedures (TTP) used by cyber criminals and Indicators of Attack (IOA) | 
| Historical sweeps and real time lookouts for Indicators of Compromise (IOC) | Treats IOC’s as pivotal points for further examination. |

## Output from Hunts

## Plot Hole Detection
Measure where your dead angles / blind spots are - those dark areas that build up on plot holes. Every attack has a story - but does it tell the entire plot?

## Behavioral and Traffic Patterns

Detection and description of observed behavioral and traffic patterns. Example:

| Pattern | Example |
| ------- | ------- |
| Server communicate with unknown external IP | Server communicates to Taiwanese IP due to driver checking for updates. Threat exposed and understood by threat hunting process. |
| Web traffic | No alerts on web traffic. On closer inspection there are custom User Agents in use that doesn’t follow current standards, also vintage (20 year old) User Agents in use. Why? Tracked down to a an old software installation reaching out to mother ship - benign. |

### Threat Detection Content Creation

The active work of discovering unknown threats - analysis of activity and tool sets. Provides direct input on content for threat detection.

### Indicators of Compromise (IOC) Creation

Materializing IOC and sharing these with the security community.

### Playbook and runbook creation of enrichment

Improves detection by providing or revising playbook or runbooks for analysts to use.

### Read team engagements

Provide Red Teams with

* Source material for new attacks
* Use cases for testing

### Threat Intelligence Reporting

Not to be confused with pt. “Indicators of Compromise (IOC) Creation”. This point takes basis in IOC and enriches them for use in Threat Intelligence Reporting - or simply creating Threat Intelligence reports.

### Incident Response Engagement

If detecting attackers on the inside, escalate to Incident Response Team - aide in process.

## Various ways of Threat Hunting

### Types

Traditionally hunts have been differentiated into two types of hunts; structured and unstructured. I have extended this traditional model with a third definition, consisting of a mixture of both types. Further definitions of the three types are as follows:

| Type  | Key points |
| ----- | ---------- | 
| Structured Hunting | Hypothesis based hunting |
| Unstructured Hunting | Looking at data, such as Least Frequency Analysis (LFA), stacking, clustering, behavior analysis, other statistics |
| Combined Hunting | Any combination where either types provides value to each other. Example: 1). Combination of both Unstructured and Structured Hunts where Unstructured Hunt provides baseline for further Hypothesizes hunts. 2). Hypothesis than can only be proven by statistics. |

### Tactics

The following is an selection of actions or strategies employed by Threat Hunters to achieve a specific end. This overview does not differentiate between various type of hunts.

| Tactic | Key points | Example |
| ------ | ---------- | ------- |
| Intelligence Driven | Basing hypothesis on Intelligence Reports | From report X: Threat Actor Y utilizes technique Z to do harm. Can we observe Threat Actor Y’s modus operandi on our internal network for last 7 days? | 
| Target Driven / Crown Jewel Driven | Prioritizing important targets in the network - the CROWN JEWELS! | Sharepoint server,Payment processing server |
| Technique Driven (TTP) | Looking for techniques driven from MITRE ATT&CK. | https://attack.mitre.org/tactics/TA0003/, https://attack.mitre.org/tactics/TA0008/ |
| Volume Analysis | Volumetric analysis looks at the volume of a particular activity in relation to all other activities. | Logons vs. account locks. Traffic usage |
| Frequency Analysis | Examines frequency of an occurrence. | How many times does event X occur? |
| Clustering Analysis | Finding outliers. | Statistics show that during a normal situation looks like this. However, every now and then this occurs. It may be a sign of C2 communication.
| Grouping Analysis | Group the data based on the occurrence of specific simultaneous conditions | X and Y occurs together - sometimes Z joins the party. Why is that? |
| Stack Counting | Aggregating and counting the number of times a condition is observed, with the intent of identifying statistical extremes in either direction. | We average on 1000 events, 100 at the lowest and 1500 at peak. |
| Other | Other | Techniques not identified here. |