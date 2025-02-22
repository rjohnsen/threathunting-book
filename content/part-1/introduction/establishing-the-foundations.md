---
title: "Establishing the Foundation"
date: 2025-02-22T17:17:54+01:00
draft: true
weight: 2
---

| Revised Date | Comment |
| ------------ | ------- |
| 22.02.2025  | Added page | TEST

**In today's cybersecurity landscape, many organizations rush to establish Security Operations Centers (SOC) and threat hunting capabilities without fully understanding the necessary groundwork. It's common to see businesses trying to jump directly into these advanced activities, only to find their efforts taking a solid nosedive since they lack the foundational components their efforts depend on. I've seen many lackluster implementations of SOC services – all bearing the same trademark: the lack of supporting layers.**

**The reality is that neither SOC nor threat hunting can function effectively without the layers beneath them. For a SOC to detect and respond to incidents, it needs a solid foundation of logs, data sources, and an asset inventory. Similarly, threat hunters need a comprehensive view of the organization's systems, network traffic, and endpoint activities to identify subtle indicators of compromise (IOCs). Without understanding what assets are at risk and having the logs to monitor them, both teams are left without the tools and context needed to perform their jobs effectively. In this topic, we are taking a dive into these layers.**

---

## Incident Response Hierarchy of Needs

This article is based on [Swannman's Incident Response Hierarchy of Needs](https://github.com/swannman/ircapabilities), because it depicts in an eminent way the layers needed for success in SOC and Threat Hunting. His model offers a structured approach to building an effective incident response program, with a clear emphasis on the layers that must be established before advanced functions like SOC or threat hunting can thrive. The pyramid consists of 10 layers, each building on the one below it. Coupled with this pyramid is Swannman's *Plateaus Model*, which tracks an organization's progression through different levels of maturity in its security operations. Together, these models provide a roadmap for building and evolving a comprehensive incident response strategy.

![Hierarchy](/images/hierarchy.png)

### Understanding the Incident Response Hierarchy of Needs

Swannman's pyramid is a visual framework that helps organizations understand how to progressively improve their security posture. The layers represent different capabilities, from basic foundational needs at the bottom to more sophisticated, proactive strategies at the top. *Threat hunting occupies the upper part of the pyramid, meaning that it is a highly specialized function that depends on having the right tools and processes in place lower down.*

### The 10 Layers of Swannman's Pyramid

Each layer in Swannman's model plays a crucial role in an organization's incident response and detection capabilities. Here's how threat hunting fits into the broader structure:

| # | Layer | Description |
| --- | -----------|-----------------|
| 1 | Asset Inventory | The base of the pyramid is an accurate inventory of assets. Organizations must first identify and classify their assets, including hardware, software, and sensitive data. Understanding what needs protection is critical before any detection or response strategies can be implemented. |
| 2 | Telemetry | Telemetry encompasses the collection of system, network, and endpoint data that is essential for security monitoring. This layer ensures that organizations have the necessary data to identify potential threats and observe normal and abnormal activities. |
| 3 | Detection | Detection focuses on the tools and mechanisms used to identify threats. This could include intrusion detection systems (IDS), SIEM solutions, and other automated detection tools that generate alerts for suspicious activity. This layer is vital for providing visibility into potential security incidents. |
| 4 | Triage | Triage involves the process of analyzing and prioritizing detected incidents to determine their severity and potential impact. Security teams assess the alerts and focus on the most critical incidents that need immediate attention. Efficient triage is crucial for avoiding alert fatigue and ensuring a quick response to genuine threats. |
| 5 | Threats | At this stage, organizations actively monitor for specific threat actors, tactics, techniques, and procedures (TTPs). This layer uses threat intelligence to identify known and emerging threats, enabling security teams to stay ahead of adversaries. |
| 6 | Behaviors | The "Behaviors" layer involves analyzing anomalous or suspicious behaviors that could indicate a security incident. Rather than relying solely on signature-based detection, organizations focus on identifying abnormal activity patterns and deviations from normal behavior. |
| 7 | Hunt  | Threat hunting is a proactive approach to cybersecurity, where security teams actively search for hidden threats across networks, endpoints, and data sources. Rather than waiting for alerts to be triggered, threat hunters dig deeper into the data to uncover signs of compromise that may have evaded traditional detection systems. |
| 8 | Track | Tracking involves continuously monitoring and following the actions of adversaries once they have been identified. Organizations maintain visibility over the threat’s movement and evolution, providing the context needed to anticipate the adversary’s next steps. |
| 9 | Act | The "Act" layer involves taking appropriate actions in response to identified threats. This could include containment, remediation, and recovery efforts. At this stage, security teams take decisive steps to neutralize the threat and prevent further damage. |
| 10 | Collaboration | The final layer emphasizes the importance of collaboration with trusted partners, such as industry peers, government agencies, and threat intelligence groups. By working together, organizations can share valuable insights and intelligence to disrupt adversary campaigns and improve defenses on a broader scale. |

### A Complement to the Hierarchy of Needs

Swannman's plateaus model is designed to illustrate how organizations progress through different levels of maturity in their security operations. While the pyramid defines the foundational needs at each level, the plateaus model represents the milestones or stages of maturity that an organization reaches as it enhances its detection and incident response capabilities. Sure, we've all seen various maturity models for SOC over the years, but this one albeit old is easy to grasp.

![Plateaus](/images/plateaus.png)

The plateaus model divides an organization's journey into distinct stages of growth, reflecting how they improve their ability to handle incidents and detect threats. These plateaus are not strictly linear, meaning organizations may need to circle back and reinforce previous stages as they evolve their strategies. However, they provide a clear framework for understanding the continuous progress toward a mature, adaptive security posture.

## Extending the plateau thought

While the pyramid describes the necessary components for building a solid security foundation, the plateaus model tracks an organization's progression through different levels of maturity. Each plateau represents a point where the organization achieves a significant improvement in its capabilities. In the following the table I have taken the liberty to explain the model from my view and stance:

| # | Plateau | Description |
| --- | ------- | ------------| 
| 1 | Basic Detection and Awareness | This plateau focuses on establishing visibility into the organization's systems, networks, and assets. Really, eastablishing the bare minimum. This aligns with the Asset Inventory and Telemetry layers in the pyramid. |
| 2 | Incident Response and Initial Triage | As organizations gain detection capability, they should start formalizing incident response plans, including triage and containment. This corresponds with the Triage and Incident Response Capabilities layers. |
| 3 | Advanced Detection and Threat Intelligence Integration | At this stage, organizations starts integrating threat intelligence feeds and advanced detection systems, such as SIEM and EDR. This plateau reflects the Detection and Threats layers. |
| 4 | Proactive Threat Hunting and Vulnerability Management | Organizations starts actively hunting for threats and improving their vulnerability management. This aligns with the 'Hunt and Behaviors layers in the pyramid. |
| 5 | Full Operational Maturity | The final plateau represents a fully mature security posture, where all elements of the pyramid work together seamlessly, enabling the organization to effectively detect, respond to, and prevent sophisticated threats. This corresponds with the Collaboration and Track layers. |

### Conclusion
    
From my stance, Swannman's model is a good and straight forward depiction on what companies and organziations should focus on when establishing their security efforts. The pyramid provides a foundational framework for building detection and response strategies, while the plateaus model tracks the evolution of an organization's maturity. Threat hunting, as one of the final stages, relies on having a solid base of detection and incident response practices already in place.

Together, these models show that cybersecurity is a layered, progressive process - starting from basic security hygiene and moving toward sophisticated, proactive defenses like threat hunting. By thinking in terms of these two models, organizations can ensure that their security program grows in a structured, manageable way and is always prepared for the next wave of threats.

Perhaps the most critical takeaway is this: this model serves as a strategic framework to prevent your incident management, SOC, and Threat Hunting efforts from devolving into a chaotic, ineffective state, where you’re forced to rebuild from the ground up. By following a structured progression, you significantly reduce the risk of missteps, ensuring that each phase of your security operations is built on solid foundations. Not only does this lead to a more resilient and adaptive defense posture, but it also creates an environment where your employees can thrive, as they work within a clear, well-supported framework that fosters growth and success.