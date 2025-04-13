---
title: "Diamond Model"
date: 2024-06-23T20:15:39+02:00
draft: false
weight: 1
tags:
    - framework
summary: ""
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Improved formatting and wording | 

## Introduction

**The Diamond Model of Intrusion Analysis is a framework used in cybersecurity to understand and analyze cyber intrusions. Developed by Sergio Caltagirone, Andrew Pendergast, and Christopher Betz in 2013, it deconstructs and examines attacks in a structured manner to provide a comprehensive understanding of threats.**

---

## About model

The model is built around four core elements, often visualized as the vertices of a diamond:

1. **Adversary**: This represents the individuals, groups, or organizations conducting the intrusion. Understanding the adversary involves identifying their motives, capabilities, and resources.

2. **Infrastructure**: This includes the tools, systems, and networks used by the adversary to carry out the attack. Infrastructure can encompass command and control servers, malware, exploited systems, and communication channels.

3. **Capability**: This vertex refers to the techniques, tactics, and procedures (TTPs) employed by the adversary. It includes the malware, exploits, and other methods used to breach systems and achieve their objectives.

4. **Victim**: The target of the intrusion, which could be individuals, organizations, or systems. Analyzing the victim helps in understanding why they were targeted and what the adversary aimed to achieve.

From [official documentation (PDF)](https://www.threatintel.academy/wp-content/uploads/2020/07/diamond_summary.pdf) by Sergio Caltagirone he depicted the model such as this:

![Example Diamond Model from Sergio Caltagirone](/images/diamond-model.png)

In addition to these four vertices, the Diamond Model incorporates several meta-features to provide a richer context:

- **Timestamp**: The time of the intrusion activity.
- **Phase**: The stage of the intrusion lifecycle (e.g., reconnaissance, exploitation, etc.).
- **Result**: The outcome of the adversary's actions (e.g., data exfiltration, system compromise).
- **Direction**: The flow of the attack, from adversary to victim.
- **Methodology**: The specific methods used by the adversary.

The Diamond Model emphasizes the relationships between these elements and encourages analysts to explore these connections to gain insights into the attack. This holistic approach helps in identifying patterns, attributing attacks to specific threat actors, and developing effective defensive strategies.

## Example

One of the best application of this model can be found over at [The DFIR Report](https://thedfirreport.com/) and in their various. For instance, the screenshot below has been obtained from their report [IcedID Brings ScreenConnect and CSharp Streamer to ALPHV Ransomware Deployment](https://thedfirreport.com/2024/06/10/icedid-brings-screenconnect-and-csharp-streamer-to-alphv-ransomware-deployment/#diamond-model). As we see they have carefully noted down important information into the Diamond Model from their investigation. 

![Example Diamond Model from DFIR Report](/images/dfir-report-diamond-model.png)

## Practical application

The Diamond Model of Intrusion Analysis has proven to be an invaluable framework for structuring and organizing my findings during threat hunts. This model enables a systematic approach to categorizing observations related to "infrastructure," "capability," and "victim," often in real-time as I gather and sort data. By using this model, I can efficiently document and analyze network traffic, techniques, tactics, procedures (TTPs), and device types, which significantly enhances the clarity and depth of my threat investigations.

> Use Case: Insider Threat Exfiltrating Sensitive Data

First and overly simplified, I analyze network traffic to identify communication points, documenting each one in the infrastructure section. Next, I assess the techniques, tactics, and procedures (TTPs) observed, recording them in the Capabilities/TTP section. Additionally, I examine the devices involved in the communication to determine their nature — whether they are PCs, servers, or other types of devices — and log this information in the victim section. This iterative process continues for each new piece of information I uncover.

The most challenging aspect is the Adversary section. While the model may seem focused on attributing an attack to a specific attacker, attribution is inherently difficult. To address this, I map the identified TTPs to the MITRE ATT&CK framework using the [MITRE Navigator](https://mitre-attack.github.io/attack-navigator/). This approach helps me correlate observed behaviors with known threat actor profiles, providing a more comprehensive understanding of potential adversaries. Although it may not provide an exact identification of the attacker, it offers valuable insights and hunches. Even though in this case I am looking for an insider, I still want to get a feeling to whom outside the communication flows.

Once completed, this structured methodology provides comprehensive notes on my findings. These notes can be handed over to the Security Operations Center (SOC) for further processing or to the threat intelligence team for additional correlation and analysis - or whoever I need to handover to. This collaborative effort ensures a thorough investigation and enhances our overall threat detection and response capabilities.

## Resources

For more detailed information, you can explore the following resources:
- [Recorded Future](https://www.recordedfuture.com/blog/what-is-the-diamond-model-of-intrusion-analysis)
- [Threat Intelligence Lab](https://www.threatintelligencelab.com/understanding-the-diamond-model-of-intrusion-analysis/)
- [ThreatConnect](https://threatconnect.com/blog/importance-of-diamond-model-cyber-threat-intelligence/)
- [Official documentation (PDF)](https://www.threatintel.academy/wp-content/uploads/2020/07/diamond_summary.pdf)

