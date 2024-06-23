---
title: "Platform"
date: 2024-06-22T11:37:31+02:00
draft: false
---

{{% notice info %}}
This public preview offers a glimpse of upcoming content. Please note that the content may be subject to change without prior notice as I continue to develop this site.
{{% /notice %}}

If you inquire about "threat hunting," you are likely to encounter a wide range of interpretations, comparable to the multitude of stars in the night sky. One Security Operations Center (SOC) might claim to engage in threat hunting, yet upon closer examination, their activities may be confined to validating Indicators of Compromise (IOCs). Conversely, another SOC might describe their approach as centered around crafting detection rules informed by threat intelligence. In this article, I aim to provide a comprehensive explanation of threat hunting, distinguish it from detection engineering, elucidate its importance, explore various methodologies of threat hunting, and more. My primary objective is to establish a foundational framework for conducting effective threat hunts and to outline the essential knowledge needed to excel in this role.

## Definition

Several companies and researchers have attempted to define the concept of threat hunting. Among these, Cyborg Security, established in 2019, stands out as a leading authority. Comprising a team of experienced threat hunters, threat intelligence analysts, and security researchers based in North America, Cyborg Security has played a pivotal role in popularizing and democratizing threat hunting through initiatives such as free training programs. Their collaboration with Intel471 in 2024 further underscores their influence in the industry. Here is how Cyborg Security defines threat hunting:

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
> *

-– [Cyborg Threat Hunting Framework](https://www.cyborgsecurity.com/library/white-paper/threat-hunting-framework/)*

Their definition resonates with my perception of threat hunting:

> Threat Hunting can be seen as an amplified version of SOC analytics. While a SOC analyst typically responds reactively to alerts triggered by specific incidents, threat hunting involves a proactive approach. Rather than waiting for alerts, a threat hunter meticulously examines log data to uncover anomalies and potential blind spots. This process requires formulating and testing hypotheses by correlating diverse sets of data.

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

## References

Resources and references

* [Threat Hunting vs. Threat Detecting: What's The Difference?](https://www.splunk.com/en_us/blog/learn/threat-hunting-vs-threat-detecting.html)
* [Cyborg Security - Threat Hunting Framework](https://info.cyborgsecurity.com/hubfs/Gated%20Content/Cyborg%20Security%20-%20Threat%20Hunting%20Framework.pdf)
* [50 Threat Hunting Hypothesis Examples](https://www.cyborgsecurity.com/blog/50-threat-hunting-hypothesis-examples/)
* [Protecting the ‘Crown Jewels’ with Threat Hunting](https://techspective.net/2019/12/13/protecting-the-crown-jewels-with-threat-hunting/)
* [How to Identify Cyber Critical Systems with a Crown Jewel Analysis](https://www.dragos.com/blog/how-to-identify-cyber-critical-systems-with-a-crown-jewel-analysis/)
* [Cyber Threat Hunting: Types, Methodologies, Best Practices](https://www.knowledgehut.com/blog/security/cyber-threat-hunting)
* [SOC Analyst vs Threat Hunter](https://www.siemxpert.com/blog/soc-analyst-vs-threat-hunter/#:~:text=The%20threat%20hunting%20process%20do,for%20the%20new%25)
* [WHAT IS CYBER THREAT HUNTING?](https://www.crowdstrike.com/cybersecurity-101/threat-hunting/)
* [IOA VS IOC](https://www.crowdstrike.com/cybersecurity-101/indicators-of-compromise/ioa-vs-ioc/)
* [BLOG What is Proactive Cyber Threat Hunting and Why Should You Do It?](https://www.securebrain.co.jp/eng/blog/what-is-proactive-threat-hunting/#:~:text=Proactive%20threat%20hunting%20is%20an,irreversible%20problems%20within%20your%20network)
* [Corelight Threat Hunting Guide](https://go.corelight.com/threat-hunting-guide-ws)
* [Security analytics use cases for threat hunting](https://atos.net/wp-content/uploads/2021/01/security-analytics-use-cases-for-threat-hunting.pdf)
* [Threat Hunting Use Case: Web Proxy](https://www.reliaquest.com/blog/threat-hunting-use-case-web-proxy/)
* [Threat Hunting with MITRE’s ATT&CK Framework: Part 1](https://www.digitalguardian.com/blog/threat-hunting-mitres-attck-framework-part-1?_gl=1%2A1fdnrup%2A_ga%25%202AMTQ4NDk4ODI5Ny4xNjgyNDkyNTE1%2A_ga_Q142HN6432%2AMTY4MjQ5MjUxNC4xLjAuMTY4MjQ5MjUxNC42MC4wLjA.)
* [Threat Hunting with MITRE’s ATT&CK Framework Part 2 – Advanced Use Cases](https://www.digitalguardian.com/blog/threat-hunting-mitre%E2%80%99s-attck-framework-part-2-%E2%80%93-advanced-use-cases)
* [Cyber Wardog Lab by Roberto Rodriguez](https://cyberwardog.blogspot.com/)
* [10 Benefits of Threat Hunting](https://resources.infosecinstitute.com/topic/10-benefits-of-threat-hunting)