---
title: "How to Start a Threat Hunting Program"
date: 2024-10-26T16:40:47+02:00
draft: false
weight: 5
---

| Revised Date | Comment |
| ------------ | ------- |
| 26.10.2024   | Added page | 

## Introduction

**One question I often encounter when discussing threat hunting is, "How do we start a threat hunting program at our company?" This is a crucial question, and I’d like to share my perspective on it. The approach to launching a threat hunting program can vary significantly between organizations. Some companies have matured to the point where they recognize the importance of proactive security measures, while others may still be reactive in their security posture.**

**Starting a threat hunting program requires a structured methodology to proactively identify potential threats that have evaded existing security defenses. Below are some key steps and tips I want to share with you to help you initiate your threat hunting efforts.**

---

### Assess Your Current Security Posture

Begin by evaluating your organization’s current security measures. Understand your existing tools, processes, and team capabilities. This assessment will help identify gaps and opportunities for improvement.

### Define Objectives and Goals
Establish clear, measurable objectives for your threat hunting program. Here are some strategies to effectively define these objectives:

| Objectives | Description |
| ---------- | ----------- | 
| Identify Key Threats | Conduct a thorough risk assessment to pinpoint specific threats relevant to your organization. This analysis should consider historical security incidents, current threat intelligence, and industry-specific vulnerabilities. Understanding these elements will help you focus your hunting efforts where they are needed most. |
| Set SMART Objectives | Use the SMART criteria — Specific, Measurable, Achievable, Relevant, and Time-bound—to define your goals. For instance, instead of setting a vague goal like “improve threat detection,” specify a goal such as “detect and respond to 90% of identified threats within 48 hours over the next year.” By establishing clear metrics, you can better evaluate the success of your threat hunting activities. |
| Prioritize Threats |Once you've identified potential threats, prioritize them based on their potential impact and the likelihood of occurrence. Focus on the most critical threats first, allowing you to allocate resources effectively. For example, if your industry is facing an uptick in ransomware attacks, you should concentrate your hunting efforts on detecting and mitigating those specific threats. |
| Align with Business Objectives | Ensure your threat hunting objectives align with your organization’s broader business goals. This alignment helps secure support from leadership and reinforces the value of your efforts. For example, if your organization is focused on achieving regulatory compliance, your threat hunting objectives should also include monitoring for compliance-related risks. |
| Develop Key Performance Indicators (KPIs) | Establish KPIs to measure the effectiveness of your threat hunting initiatives. Useful KPIs could include: **Time to Detection (TTD)** which is the speed at which your team can identify potential threats. **Threat Containment Rate** which is the percentage of threats that are contained effectively once identified. **Incident Reduction** is about tracking the decrease in security incidents directly attributed to proactive hunting efforts. _Keep in mind, threat huning will improve the KPIs for you SOC (if you have one), so you better take that in consideration as well_|
|  Communication and Reporting | Clearly communicate the defined objectives and goals to your threat hunting team and relevant stakeholders. Ensuring that everyone understands the objectives fosters collaboration and accountability. Consider creating regular reports to update stakeholders on your progress and demonstrate the impact of your hunting efforts. But also, keep on talking about threat hunting to anyone in a clear and non technical way - peak some interest! |
| Review and Adapt | Regularly review your objectives and KPIs to ensure they remain relevant. The threat landscape is constantly evolving, and your objectives should adapt accordingly. Gathering feedback from your team can provide insights into necessary adjustments, fostering a culture of continuous improvement. |
| Incorporate Feedback Loops | Implement feedback mechanisms to learn from each threat hunting iteration. Analyzing outcomes will help you understand what strategies worked well and where improvements are needed, allowing you to refine your objectives and enhance your hunting program over time. Please also involve stakeholders and people who is interested in threat hunting to listen in (but not necessarily speak) |

### Gather the Right Tools and Resources

Equip your threat hunting team with the necessary tools, including security information and event management (SIEM) systems, endpoint detection and response (EDR) solutions, and threat intelligence platforms. Familiarity with these tools is essential for effective threat detection and analysis. 

> When I started out in threat hunting, I only had an outdated installation of QRadar and some random logs. From there, I built up a list of what I needed to have in place. However, at that time, the company I worked for wasn't willing to invest much. So, I resorted to creating a bunch of Python scripts to aid me in my threat hunting efforts. The scripts ranged from pulling threat intelligence from various online sources to correlating alerts against other alerts on the tenants in QRadar. I wouldn’t say this was the best way to do things, but hey, that’s how I started out.

### Build a Skilled Team

Assemble a team of analysts (if possible) with diverse skill sets, including knowledge of networking, malware analysis, and incident response. Continuous training and development are vital to keep the team updated on the latest threat landscapes and hunting techniques.

> I have a stance that threat hunting is like pair programming. You simply need at least two persons to have a good discussion on and validation of things.

### Leverage Threat Intelligence

Integrate threat intelligence into your hunting process. This intelligence can provide insights into current threats, trends, and known indicators of compromise (IOCs), enhancing your ability to detect and respond to potential threats.

> Threat intelligence is more than following the news closely. It is important, but when it comes to threat intelligence you need technical data and a good way to query that data. 

### Develop Hypotheses for Investigation

Create hypotheses based on observed anomalies or emerging threats that you should investigate.

Examples of hypothesis might be: 

* If there is a sudden increase in outbound traffic to an uncommon IP address during non-business hours, it may indicate an attempt to exfiltrate sensitive data.
* If a user account experiences a significant number of failed login attempts from different geographic locations within a short time frame, it may indicate an ongoing brute force attack.
* If a system has newly installed software that is not approved by the IT department, it may indicate the presence of malware or an unwanted application.
* If there is a spike in changes to administrative privileges across multiple accounts within a short period, it may indicate that an account has been compromised.
* If a user logs in from multiple geographical locations within a brief time frame that is inconsistent with their normal behavior, it may suggest that the account has been compromised.

> We put the hypothesis to the test in our threat-hunting efforts, trying to prove them right, wrong, or something in between. The reason I say "in between" is that sometimes our hunts yield nothing. Have we succeeded or not? Well, even if we get no results from the current hunt, we may still achieve results if we revisit the hunt later or create detection rules based on it. 

### Conduct Proactive Hunting Activities

Execute your hypotheses by analyzing data, investigating anomalies. Use your tools to examine logs and network traffic for deeper insights into potential threats.

> Keep in mind that looking for IOCs isn't the same as hunting. If we search for one specific IP associated with an APT and can't find it, does that mean the APT isn't present, or does it mean that the APT has switched to another IP? Look for Tactics, Techniques, and Procedures instead

### Document and Share Findings

Maintain detailed documentation of your findings and share them with relevant stakeholders. This practice not only helps improve future hunting efforts but also contributes to the organization’s overall security knowledge base. 

> The best tools a threat hunter can have is: a brain, pen and paper. Let your brainwork down on paper (or in Notepad etc), this will make reporting easier! 

### Iterate and Improve

Continuously refine your threat hunting processes based on lessons learned, emerging threats, and team feedback. Regular reviews and updates will help you adapt to the evolving threat landscape.

> Imagine you improve yourself every time you take a drive ... The world would be so much better if everyone did that

### Engage with the Threat Hunting Community

To circle back to my introduction. Participate in forums, conferences, and collaborative platforms to engage with the broader threat hunting community. Sharing experiences and learning from others can provide valuable insights and keep you informed of the latest trends and techniques.

By following these steps, I think you can lay a solid foundation for a threat hunting program that enhances your organization’s security posture and resilience against evolving cyber threats. Remember, threat hunting is not a one-time effort but an ongoing process that requires dedication and adaptability.

### Resources

- [MITRE ATT&CK](https://attack.mitre.org/)
- [Threat Hunting: A Practical Guide](https://www.cisecurity.org/white-papers/threat-hunting-a-practical-guide/)
- [SANS Threat Hunting Resources](https://www.sans.org/white-papers/34847/)
- [Threat Hunting with Splunk](https://www.splunk.com/en_us/blog/security/threat-hunting-using-splunk.html)
- [Verizon Data Breach Investigations Report](https://enterprise.verizon.com/resources/reports/dbir/)
- [The Threat Hunting Process: A Practical Guide](https://www.cybintsolutions.com/threat-hunting-process-guide/)
- [Incident Response and Threat Hunting](https://www.cyber.gov.au/acsc/view-all-content/publications/incident-response-threat-hunting)
- [Hunt for Threats: The Art and Science of Threat Hunting](https://www.csoonline.com/article/3569268/hunt-for-threats-the-art-and-science-of-threat-hunting.html)
- [Threat Hunting: Analyzing Security Events](https://www.microsoft.com/security/blog/2020/06/16/threat-hunting-analyzing-security-events/)
- [Open Threat Hunting Framework](https://www.openthreat.hunting)
