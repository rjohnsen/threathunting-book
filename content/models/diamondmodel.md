---
title: "Diamond Model"
date: 2024-06-23T20:15:39+02:00
draft: false
weight: 1
---

{{% notice info %}}
This public preview offers a glimpse of upcoming content. Please note that the content may be subject to change without prior notice as I continue to develop this site.
{{% /notice %}}

## About model

The Diamond Model of Intrusion Analysis is a framework used in cybersecurity to understand and analyze cyber intrusions. Developed by Sergio Caltagirone, Andrew Pendergast, and Christopher Betz in 2013, it deconstructs and examines attacks in a structured manner to provide a comprehensive understanding of threats. The model is built around four core elements, often visualized as the vertices of a diamond:

1. **Adversary**: This represents the individuals, groups, or organizations conducting the intrusion. Understanding the adversary involves identifying their motives, capabilities, and resources.

2. **Infrastructure**: This includes the tools, systems, and networks used by the adversary to carry out the attack. Infrastructure can encompass command and control servers, malware, exploited systems, and communication channels.

3. **Capability**: This vertex refers to the techniques, tactics, and procedures (TTPs) employed by the adversary. It includes the malware, exploits, and other methods used to breach systems and achieve their objectives.

4. **Victim**: The target of the intrusion, which could be individuals, organizations, or systems. Analyzing the victim helps in understanding why they were targeted and what the adversary aimed to achieve.

In addition to these four vertices, the Diamond Model incorporates several meta-features to provide a richer context:

- **Timestamp**: The time of the intrusion activity.
- **Phase**: The stage of the intrusion lifecycle (e.g., reconnaissance, exploitation, etc.).
- **Result**: The outcome of the adversary's actions (e.g., data exfiltration, system compromise).
- **Direction**: The flow of the attack, from adversary to victim.
- **Methodology**: The specific methods used by the adversary.

The Diamond Model emphasizes the relationships between these elements and encourages analysts to explore these connections to gain insights into the attack. This holistic approach helps in identifying patterns, attributing attacks to specific threat actors, and developing effective defensive strategies.

## Example

One of the best application of this model can be found over at [The DFIR Report](https://thedfirreport.com/) and in their various. For instance, the screenshot below has been obtained from their report [IcedID Brings ScreenConnect and CSharp Streamer to ALPHV Ransomware Deployment](https://thedfirreport.com/2024/06/10/icedid-brings-screenconnect-and-csharp-streamer-to-alphv-ransomware-deployment/#diamond-model). AS we see they have carefully noted down important information into the diamond model from their investigation. 

![Example Diamond Model from DFIR Report](/images/dfir-report-diamond-model.png)

## Practical application

## Resources

For more detailed information, you can explore the following resources:
- [Recorded Future](https://www.recordedfuture.com/blog/what-is-the-diamond-model-of-intrusion-analysis)
- [Threat Intelligence Lab](https://www.threatintelligencelab.com/understanding-the-diamond-model-of-intrusion-analysis/)
- [ThreatConnect](https://threatconnect.com/blog/importance-of-diamond-model-cyber-threat-intelligence/)

