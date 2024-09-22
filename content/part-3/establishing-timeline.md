---
title: "Establishing Timeline"
date: 2024-09-15T14:26:43+02:00
weight: 2
draft: false
---

Over the years, I’ve trained many students to become SOC analysts, and eventually, Threat Hunters. This is a challenging field, especially when working with young and inexperienced individuals. One of the biggest hurdles I’ve observed is their difficulty in grasping certain concepts, such as timelines. Many view a timeline as simply a linear progression of events—a straightforward sequence from point A to point B. In this chapter, I aim to demystify that notion and provide a deeper understanding of how timelines function in threat hunting.

In threat hunting, a timeline is more than just a chronological list of events. It’s a detailed reconstruction that helps investigators understand how a security incident unfolded. Timelines involve the collection, analysis, and correlation of logs, alerts, and system activity to piece together the actions of a threat actor or malicious process. These can include everything from user actions and system changes to network activity and file executions.

The purpose of a timeline is to provide a clear, structured view of what happened, when it happened, and how the threat actor moved through, say, the network. This clarity is essential for identifying key indicators of compromise (IOCs), determining the scope of an attack, and developing an effective response.

## Key components of a threat hunting timeline 

Key components of a threat hunting timeline may include:

| # | Phase | Description | 
| -- | ---- | ----------- |
| 1. | _Initial Compromise_ | The point at which the attacker first gains access to the network, such as through phishing emails, exploiting vulnerabilities, or unauthorized logins. |
| 2. | _Lateral Movement_ | Evidence of how the attacker moves through the network, using methods like credential dumping, remote execution, or exploiting trusted relationships between systems. |
| 3. | _Persistence_  | Actions taken by the attacker to maintain ongoing access to the network, such as installing backdoors, creating new accounts, or modifying system files. |
| 4. | _Data Exfiltration or Impact_ | Signs that sensitive data has been stolen, encrypted, or destroyed, which is crucial for assessing the damage. |
| 5. | _Indicators of Compromise (IOCs)_ | Specific data points, such as file hashes, IP addresses, or domain names, that help identify and track similar malicious activities. |

The reason I say "may include" is because you may base your timeline on other phases from Mitre ATT&CK or other cyber kill chains. This will be dependent on the case investigated. Anyhow, by analyzing these events in a timeline, threat hunters can uncover vulnerabilities, learn from past attacks, and proactively detect future threats.

## An example of a timeline

The following example illustrates a cyberattack from start to finish, as investigated by an experienced threat hunter. The investigation was driven by the following hypothesis:

> _Outbound network traffic suggests sensitive data may be exfiltrated to an external location, possibly indicating a breach._

With this hypothesis in mind, the threat hunter began constructing a timeline by analyzing logs, alerts, and data from various monitoring tools. The key steps in this process included:

| Step | Description |
| ---- | ----------- |
| _Log Analysis_ | Reviewing system, application, and network logs to identify unusual behavior, such as lateral movement, unauthorized account creation, or privilege escalation. |
| _File Integrity Monitoring_ | Checking for suspicious file modifications, the presence of backdoors, or unauthorized changes to system configurations. |
| _Network Traffic Monitoring_ | Tracking anomalous outbound connections, particularly those to suspicious external IP addresses, to verify potential data exfiltration. |

The goal of the investigation was to map out the attacker's actions in detail, reconstruct the entire timeline, and assess the full extent of the breach, including which systems were compromised. This work resulted in the following timeline (exact date omitted):

{{<mermaid align="center">}}
sequenceDiagram
    participant Attacker
    participant Victim
    participant Network
    participant Sensitive System

    %% Initial Compromise
    Attacker->>Victim: Send Phishing Email [09:00]
    Victim->>Attacker: Credentials Harvested [09:15]
    Attacker->>Network: Exploit Vulnerability [09:30]
    Network->>Attacker: Access Granted [09:35]

    %% Lateral Movement
    Attacker->>Network: Explore Network [09:45]
    Attacker->>Network: Steal Credentials [10:00]
    Attacker->>Sensitive System: Move Laterally to Sensitive Systems [10:15]
    Sensitive System->>Attacker: Access Sensitive Data [10:30]

    %% Persistence
    Attacker->>Sensitive System: Install Backdoor [11:00]
    Attacker->>Sensitive System: Create New Accounts [11:15]
    Attacker->>Sensitive System: Escalate Privileges [11:30]

    %% Data Exfiltration
    Attacker->>Sensitive System: Exfiltrate Data [12:00]
    Attacker->>Network: Cover Tracks [12:30]
    Attacker->>Network: Exit Network [12:45]
 {{< /mermaid >}}

{{<mermaid align="center">}}
 timeline
    title History of Social Media Platform
    2002 : LinkedIn
    2004 : Facebook : Google
    2005 : Youtube
    2006 : Twitter
{{< /mermaid >}}

{{<mermaid align="center">}}
info
{{< /mermaid >}}

From this timeline we see that the threat hunter with basis in the hypothesis discovered traces of data exfiltration at 12:00 on that given day. From there the threat hunter followed traces both back and forth in time to uncover the entire scope.

## The role of timelines in reports

Timelines are essential in threat-hunting reports for multiple reasons. They provide a clear, chronological sequence of events that helps investigators understand how a security incident unfolded. This clarity is critical for reconstructing the attack, identifying patterns, and grasping the progression of the threat actor's activities.

By mapping out events, timelines allow threat hunters to pinpoint significant actions taken by the attacker, such as the initial compromise, lateral movement, persistence methods, and data exfiltration. This detailed view aids in identifying key indicators of compromise (IOCs) and understanding the overall scope of the attack.

Timelines are also instrumental in detecting anomalies or deviations from normal behavior. By comparing the timeline of an attack with typical activity patterns, investigators can more easily spot unusual or malicious activities.

Additionally, a well-structured timeline enhances coordination among team members and improves communication with stakeholders, including management. It provides a coherent and easily understandable overview of the incident. Understanding the timeline of an attack is crucial for assessing its impact and scope, which in turn helps in developing an effective response plan and implementing remediation strategies to mitigate damage and prevent future incidents.

For post-incident analysis, timelines are invaluable. They enable organizations to review how the attack unfolded, identify weaknesses in their defenses, and strengthen their security posture based on lessons learned from past incidents.

Timelines also play a crucial role in compliance with legal and regulatory requirements. They offer a detailed record of events that can be used for forensic investigations, legal proceedings, or audits.

Furthermore, timelines assist in identifying the root cause of an incident by showing how and when different factors contributed to the breach. This understanding is vital for addressing underlying vulnerabilities and preventing similar attacks in the future.

Overall, timelines significantly enhance the effectiveness of threat-hunting efforts by providing a structured view of the incident, which is essential for thorough investigation, effective response, and continuous improvement.

Additionally, timelines are crucial for training and awareness. For instance, in our example, the threat hunter discovered that a simple phishing email triggered the attack. By tracing the timeline, we could see how that initial email led to a chain of events resulting in data loss for the customer, illustrating the importance of understanding the full scope of an attack for both prevention and education.