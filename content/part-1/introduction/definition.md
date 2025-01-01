---
title: "Definition"
date: 2024-06-22T11:37:31+02:00
draft: false
weight: 1
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Improved formatting and wording | 

## Introduction

**If you inquire about "threat hunting," you are likely to encounter a wide range of interpretations, comparable to the multitude of stars in the night sky. One Security Operations Center (SOC) might claim to engage in threat hunting, yet upon closer examination, their activities may be confined to validating Indicators of Compromise (IOCs). Conversely, another SOC might describe their approach as centered around crafting detection rules informed by threat intelligence. In this article, I aim to provide a comprehensive explanation of threat hunting, distinguish it from detection engineering, elucidate its importance, explore various methodologies of threat hunting, and more. My primary objective is to establish a foundational framework for conducting effective threat hunts and to outline the essential knowledge needed to excel in this role.**

---

## Definition

Several companies and researchers have attempted to define the concept of threat hunting. Among these, Cyborg Security, established in 2019, stands out as a leading authority. Comprising a team of experienced threat hunters, threat intelligence analysts, and security researchers based in North America, Cyborg Security has played a pivotal role in popularizing and democratizing threat hunting through initiatives such as free training programs. Here is how Cyborg Security defines threat hunting:

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

> Threat Hunting can be seen as an amplified version of SOC analytics. While a SOC analyst typically responds reactively to alerts triggered by specific incidents, threat hunting involves a proactive approach. Rather than waiting for alerts, a threat hunter meticulously examines log data to uncover anomalies and potential blind spots. This process requires formulating and testing hypotheses by correlating diverse sets of data. Data can be logs themselves, but also statistics drawn out from the logs. Threat Hunting isn't just about following trails in logs, but also understanding patterns existing within the logs. 

## Reasons why for Threat Hunting

### Why Should We Employ Threat Hunters?

In today’s complex cybersecurity landscape, employing dedicated threat hunters has become crucial for organizations aiming to stay ahead of advanced and persistent threats. Here’s why threat hunters are essential and what they bring to the table:

#### The Value of Threat Hunters:

1. **Proactive Threat Detection:** 
   Threat hunters actively seek out potential threats within the network, rather than waiting for automated systems to flag an issue. This proactive approach helps identify and mitigate threats before they can cause significant damage. Becoming aware of threats that are lurking or hidden but active is a key advantage.

2. **Advanced Threat Identification:**
   Threat hunters specialize in uncovering sophisticated threats that often bypass traditional security measures. Their expertise enables them to detect subtle indicators of compromise that standard tools might miss. This course of action involves searching through networks for indicators of abnormal behavior caused by potential attacks, ensuring a human-driven process designed to identify threats that automated systems or conventional detection methods might overlook.

3. **Deep Analysis and Insights:**
   By thoroughly analyzing network activity, threat hunters provide valuable insights into the tactics, techniques, and procedures (TTPs) used by adversaries. This information is critical for strengthening overall security posture and informing future defense strategies. Threat hunters also offer a better understanding of the organization’s environment, aiding cybersecurity analysts in grasping the company’s security state and its resilience to various attacks.

4. **Enhanced Incident Response:**
   The early detection capabilities of threat hunters allow for a more efficient and effective incident response. By identifying threats early, they minimize potential damage and reduce recovery time and costs. This proactive approach normally means less possibility for malicious intruders and threats to cause damage to an organization’s systems and data.

5. **Improved Defense System:**
   Threat hunters help identify defense systems to further secure the environment, ensuring appropriate mitigation of threats. This process brings Security Operations Centers (SOCs) into the future by providing advanced detection rules and broadening the detection surface.

6. **Reduced False Positives and Improved SOC Efficiency:**
   Hunting’s strength lies in its human-driven, proactive, iterative, and analytical nature. This combination of tools, repetitive monitoring, and behavior-pattern searching, coupled with analysts’ ingenuity and ability to examine and evaluate data, leads to a reduction in false positives and time-wasting.

7. **Continuous Adaptation:**
   The threat landscape is constantly evolving, and threat hunters are adept at adapting to new and emerging threats. Their continuous learning and adaptation ensure that the organization remains resilient against the latest cyber threats, reducing investigation time and providing analysts with a comprehensive view of the organization’s current security state.

### Why SOC Analysts Aren’t Enough

While Security Operations Center (SOC) analysts play a vital role in maintaining cybersecurity, their primary focus is often on monitoring alerts and managing incidents as they arise. Here’s why SOC analysts alone may not suffice:

1. **Reactive vs. Proactive Approach**
   SOC analysts typically respond to alerts generated by security tools, which can result in a reactive approach. Threat hunters, on the other hand, take a proactive stance, actively searching for threats that may not trigger alerts.

2. **Volume of Alerts**
   SOC analysts are often overwhelmed by the sheer volume of alerts, leading to potential alert fatigue. Threat hunters can help alleviate this burden by focusing on uncovering advanced threats that automated systems might overlook.

3. **Specialized Skill Set**
   Threat hunting requires a unique set of skills, including advanced knowledge of threat actors, their behaviors, and their methods. While SOC analysts are skilled in incident response, threat hunters bring specialized expertise in tracking and neutralizing advanced threats.

### Conclusion

Employing threat hunters adds a crucial layer of defense to your cybersecurity strategy. Their proactive approach, specialized skills, and ability to uncover sophisticated threats complement the efforts of SOC analysts, ensuring a more robust and resilient security posture. By integrating threat hunters into your security team, you can enhance your organization’s ability to detect, respond to, and prevent advanced cyber threats. And not to forget, Threat Hunters will add value to the importance of understanding the Organization’s [Threat-Discovery Maturity](https://www.infosecinstitute.com/resources/threat-hunting/10-benefits-of-threat-hunting/):

{{<mermaid align="center">}}
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

Over the years, as I’ve built and managed various SOC departments, conducting job interviews and engaging with candidates has become an integral part of my role. Through these conversations, I’ve observed that there are numerous misconceptions surrounding the concept of Threat Hunting. To clear up this confusion, it’s important to distinguish between Threat Detection and Threat Hunting. These are distinct disciplines, and understanding their differences is crucial for clarity. Let’s delve into this differentiation.

| Threat Detection | Threat Hunting |
| ---------------- | -------------- |
| Threat Detection is reactive. There’s a wait for security systems to detect occurrences before acting/investigation can happen. | Is proactive. Threat Hunting is actively looking for and investigates suspicious artifacts and circumstances. |
| Threat Detection reacts to “knowns”. Things must be known and detection rules must be written and implemented prior to detection. |Lifts unknowns to known state - then provide detection for it. |
| Detects on pure technical grounds - can’t judge human behavior other than statistical aspect. | The human aspect of detection / interpretation | 
| Driven by Indicators of Compromise (IOC) | Driven by Tactics, Techniques, and Procedures (TTP) used by cyber criminals and Indicators of Attack (IOA) | 
| Historical sweeps and real time lookouts for Indicators of Compromise (IOC) | Treats IOC’s as pivotal points for further examination. |

The confusion between Threat Detection and Threat Hunting is understandable, given that their methodologies, skills, and technologies often overlap. This document adopts an evolved SOC analyst mindset to address these distinctions. Despite the similarities, there are clear differences between the two disciplines. To further clarify, let's highlight how they differ from other fields within cyber security:

| Definition | What it really is |
| ---------- | ---------- |
| Responding to a detected threat to discover the scope of the compromise and develop strategies to contain, evict, and remediate it. | Incident Response. |
| Investigating a security tool alert to determine whether it is valid or a false positive. | Threat Detection or Security Monitoring. |
| Gathering and producing information about threat actors and their TTP and infrastructure. | Threat Intelligence. |
| Adding a list of known indicators of compromise, such as IP addresses, hashes, domains, URLs, etc., to a tool and looking for hits within the specific time window. | Retroactive Investigation. |

## Output from Hunts

What outcomes can we expect from Threat Hunts, and how do they compare to what a SOC delivers? Understanding these differences is crucial for aligning expectations and leveraging the full potential of each function. Let’s explore how the results of Threat Hunting might differ from those provided by a SOC and what unique value Threat Hunts bring to the table.

### Plot Hole Detection

Identify your organization's blind spots—those areas where potential threats might remain undetected. Every attack has its own narrative, but does it reveal the complete picture Understanding the differences between Threat Hunting and SOC operations can shed light on these gaps. SOCs focus on monitoring and responding to alerts, which might not always capture the full scope of a sophisticated attack. In contrast, Threat Hunting seeks to uncover hidden threats by proactively searching for anomalies that could indicate broader, undetected vulnerabilities. This proactive approach helps to illuminate those dark areas and ensures that the full plot of every attack is revealed, enhancing your overall security posture.

### Behavioral and Traffic Patterns

Understanding the intricate details of observed behavioral and traffic patterns is a key advantage of Threat Hunting and highlights its importance in modern cybersecurity. Threat hunters excel at detecting and analyzing these patterns, which provides a deeper insight into potential threats that traditional detection methods might miss.

By scrutinizing behavioral anomalies and unusual traffic patterns, threat hunters can identify indicators of compromise that go beyond simple alert triggers. This process involves:

1. **Identifying Subtle Indicators:** Threat hunters look for subtle deviations from normal behavior, such as unusual data flows, irregular login patterns, or atypical access requests. These indicators often signal sophisticated attacks or lateral movement within the network that standard detection systems might overlook.

2. **Understanding Attack Methodologies:** Analyzing traffic patterns helps threat hunters understand the tactics, techniques, and procedures (TTPs) employed by attackers. This knowledge enables them to recognize specific attack vectors and anticipate future threats, providing valuable context that enhances overall threat detection capabilities.

3. **Revealing Hidden Threats:** Behavioral analysis allows threat hunters to uncover hidden threats that may not yet have generated alerts. By understanding normal network behavior, they can detect anomalies that indicate ongoing or imminent attacks, even if those attacks have not yet been flagged by automated systems.

4. **Improving Incident Response:** A thorough understanding of observed patterns aids in more accurate and timely incident response. By pinpointing unusual activities and understanding their potential impact, threat hunters can prioritize their responses and mitigate threats more effectively.

In summary, the ability of threat hunters to analyze and interpret complex behavioral and traffic patterns is crucial. It not only helps in detecting hidden threats but also enhances the overall security posture by providing a more comprehensive view of the threat landscape, ultimately leading to better-informed and more effective defense strategies.

### Threat Detection Content Creation

The findings from threat hunting efforts contribute to the development and optimization of threat detection content. This includes creating and updating detection rules, refining alert thresholds, and developing new signatures based on the newly discovered threat patterns and behaviors. This continuous improvement cycle ensures that detection mechanisms remain effective against evolving threats.

### Indicators of Compromise (IOC) Creation

Materializing Indicators of Compromise (IOCs) and sharing them with the security community is a vital practice in advancing collective cybersecurity efforts. Here’s why Threat Hunters and Hutning is crucial for handling new IOC's:

1. **Turning IOCs into Actionable Intelligence:** Threat hunters and security analysts work to identify and validate IOCs from their investigations, such as malicious IP addresses, domain names, file hashes, and patterns of behavior. By materializing these indicators, they transform raw data into actionable intelligence that can be used to detect and respond to threats.

2. **Enhancing Collective Defense:** Sharing IOCs with the broader security community helps to strengthen collective defense. When IOCs are disseminated across organizations, they provide a wider net for detecting and mitigating threats. This collaborative approach ensures that more entities are aware of potential threats and can take preventive measures, reducing the overall risk of widespread attacks.

3. **Accelerating Threat Detection and Response:** The timely sharing of IOCs facilitates faster identification of threats. Security teams can integrate these indicators into their detection systems, such as SIEMs (Security Information and Event Management) and threat intelligence platforms, to enhance their ability to recognize and respond to attacks in real-time.

4. **Building a Stronger Security Ecosystem:** Sharing IOCs contributes to a more resilient cybersecurity ecosystem. By pooling knowledge and resources, organizations can leverage the collective expertise and experience of the community. This shared knowledge base helps in identifying patterns and trends that may not be apparent when viewed in isolation.

5. **Promoting Industry Collaboration:** Materializing and sharing IOCs fosters greater collaboration among industry peers, threat intelligence providers, and security vendors. This collaboration can lead to the development of better detection tools, refined threat models, and more effective security practices.

### Playbook and runbook creation or enrichment

Improving detection through the refinement of playbooks and runbooks is a crucial aspect of enhancing a security team's effectiveness. By updating and optimizing these procedural documents, organizations can significantly bolster their ability to identify and respond to complex threats.

1. **Enhanced Procedures for Complex Investigations:** Revamping playbooks and runbooks ensures that analysts have detailed, up-to-date procedures for investigating difficult-to-detect threats. This includes developing new methodologies for exploring elusive indicators or accessing less obvious clues hidden in logs. Well-defined procedures enable SOC teams to systematically approach and dissect complex incidents that might otherwise be overlooked.

2. **Optimized Detection Techniques:** By integrating findings from threat hunts and previous investigations into playbooks, organizations refine their detection techniques. This iterative improvement process incorporates lessons learned from past incidents, allowing SOC analysts to better recognize and act on subtle or unconventional attack patterns that automated systems might miss.

3. **Systematic Investigation of Anomalies:** Updated runbooks provide structured approaches for analyzing anomalies, such as unexpected traffic patterns or unusual user behavior. These guidelines help SOC teams navigate and interpret complex data, improving their ability to pinpoint and investigate hard-to-reach areas within the network.

4. **Improved Response Efficiency:** Clear, revised playbooks streamline response processes, enabling analysts to quickly and effectively address emerging threats. With well-documented procedures, SOC teams can reduce response times and ensure a consistent, coordinated approach to threat management.

5. **Facilitating Knowledge Transfer:** Comprehensive playbooks and runbooks also serve as valuable training tools for new analysts. By codifying best practices and investigative techniques, organizations ensure that knowledge is effectively transferred and that all team members are equipped with the tools and strategies needed for effective threat detection.

### Incident Response Engagement

Threat hunters possess advanced expertise and maturity in log analysis, which makes them invaluable during incidents requiring detailed investigation. Their deep understanding of log data allows them to uncover subtle anomalies and sophisticated attack patterns that traditional detection systems might miss. Here’s why involving threat hunters in log analytics during incidents is a strategic advantage:

1. **Expertise in Log Interpretation:** Threat hunters are adept at sifting through vast amounts of log data to identify indicators of compromise (IOCs) and patterns of malicious activity. Their ability to interpret complex log information provides crucial insights into the nature and scope of an attack, enhancing the overall effectiveness of incident investigations.

2. **Enhanced Detection of Internal Threats:** When dealing with potential attackers inside the network, threat hunters can leverage their skills to detect and analyze internal threats that may not immediately trigger conventional alerts. Their expertise in recognizing subtle deviations and unusual behaviors within logs ensures a more comprehensive identification of insider threats.

3. **Seamless Integration with Incident Response:** Threat hunters’ involvement in log analytics can greatly streamline the incident response process. By providing detailed analyses and actionable insights, they support the Incident Response Team in developing effective containment, eradication, and remediation strategies. This collaboration helps ensure that responses are well-informed and targeted.

4. **Augmented Investigation Efficiency:** By contributing their specialized knowledge to log analysis, threat hunters enhance the efficiency of incident investigations. Their capability to quickly identify and prioritize relevant data reduces the time required to understand and address complex security incidents.

5. **Continuous Improvement of Detection Mechanisms:** The insights gained from threat hunters' log analysis can also inform improvements in detection mechanisms and security posture. By identifying gaps and refining detection rules based on real-world attack data, they help strengthen the organization’s defenses against future threats.

In summary, involving threat hunters in log analytics during incidents leverages their advanced skills to enhance detection, improve incident response efficiency, and provide critical support in identifying and addressing internal threats. Their expertise ensures a more robust and effective approach to managing and mitigating security incidents.

## Various ways of Threat Hunting

### Types

Traditionally, threat hunts have been categorized into two distinct types: structured and unstructured. I have expanded this traditional model to include a third category, which combines elements of both types. The definitions for these three types are as follows:

| Type  | Key points |
| ----- | ---------- | 
| Structured Hunting | Hypothesis based hunting |
| Unstructured Hunting | Looking at data, such as Least Frequency Analysis (LFA), stacking, clustering, behavior analysis, other statistics |
| Combined Hunting | Any combination where either types provides value to each other. Example: 1). Combination of both Unstructured and Structured Hunts where Unstructured Hunt provides baseline for further Hypothesizes hunts. 2). Hypothesis that can only be proven by statistics. |

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
* [Flavio Queiro on what Threat Hunting isn't](https://www.linkedin.com/posts/flavioqueiroz_threathunting-threatdetection-threatanalysis-activity-7207336024216133632-bdvv/?utm_source=share&utm_medium=member_desktop)