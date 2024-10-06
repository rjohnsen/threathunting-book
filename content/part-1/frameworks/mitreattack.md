---
title: "MITRE ATT&CK"
date: 2024-06-23T20:15:47+02:00
draft: false
weight: 4
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Improved formatting and wording | 

## Introduction

**MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) is a comprehensive framework that categorizes the actions and behaviors of cyber adversaries. It provides a detailed matrix of tactics and techniques that attackers use across the lifecycle of an intrusion, from initial access to the completion of their objectives. The framework is designed to help organizations improve their cybersecurity posture by understanding how attackers operate and identifying gaps in their defenses.**

## About Model

The MITRE ATT&CK framework consists of the following tactics:

|  | Tactic | Description |
| - | ------ | ----------- | 
| 1. | **Initial Access** | Techniques used to gain entry into a network. |
| 2. | **Execution** | Techniques used to run malicious code.| 
| 3. | **Persistence** | Techniques that ensure continuous access. |
| 4. | **Privilege Escalation** | Techniques to gain higher-level permissions. |
| 5. | **Defense Evasion** | Techniques to avoid detection. | 
| 6. | **Credential Access** | Techniques to steal credentials. | 
| 7. | **Discovery** | Techniques to gather information about the target environment. |
| 8. | **Lateral Movement** | Techniques to move through the network. |
| 9. | **Collection** | Techniques to gather data of interest. | 
| 10. | **Command and Control** | Techniques to maintain communication with compromised systems. |
| 11. | **Exfiltration** | Techniques to steal data from the target environment. |
| 12. | **Impact** | Techniques to disrupt operations or destroy data. |

### A note on TTP's

#### Tactics

Tactics are the overarching goals or objectives that adversaries aim to achieve during an attack. They represent the "why" behind an attack. Within the MITRE ATT&CK framework, tactics are organized as columns in the ATT&CK matrix. Each column represents a different tactical goal that adversaries might aim to achieve.

Examples (but not limited to):

* Initial Access
* Persistence
* Privilege Escalation
* Defense Evasion


#### Techniques

Techniques describe the "how" an adversary accomplishes a specific tactic. They are the methods used to achieve a particular objective or to execute a particular tactic. Techniques are listed under each tactic column and describe various ways to achieve the tactical goal. Each technique can have multiple sub-techniques that provide more specific instances or variations of the primary technique.

Examples (but not limited to):

* Spear Phishing,
* PowerShell
* Credential Dumping
* DLL Injection

### Procedures

Procedures are the specific, detailed steps or sequences of actions taken by adversaries to execute a technique. They provide the granular "how-to" details of an attack, often unique to a specific adversary or campaign. Procedures are often detailed in the descriptions of techniques and sub-techniques within the ATT&CK framework. They provide real-world examples of how adversaries have implemented these techniques in actual attacks.

Examples (but not limited to):

* Using a particular phishing email template
* Employing a specific script for credential dumping
* Exploiting a known vulnerability in a certain way, etc.

## Example

Consider a scenario where a manufacturing company is targeted by a cyberattack. Using the MITRE ATT&CK framework, the security team can map out the attack stages:

1. **Initial Access**: Attackers use spear-phishing emails to gain access.
2. **Execution**: Malicious scripts are executed via PowerShell.
3. **Persistence**: Attackers create new user accounts for ongoing access.
4. **Privilege Escalation**: They exploit a vulnerability to gain administrator rights.
5. **Defense Evasion**: Attackers disable antivirus software.
6. **Credential Access**: They dump password hashes from the Security Accounts Manager (SAM) database.
7. **Discovery**: Attackers scan the network to identify valuable systems.
8. **Lateral Movement**: Using remote desktop protocol (RDP), they move to a file server.
9. **Collection**: Sensitive design documents are gathered.
10. **Command and Control**: Compromised systems communicate with the attacker’s server via an encrypted channel.
11. **Exfiltration**: Sensitive documents are transferred out using HTTP.
12. **Impact**: Attackers deploy ransomware to disrupt manufacturing operations.

## Practical Application

In threat hunting, the MITRE ATT&CK framework can be used to identify and mitigate threats at each stage of an attack. Here’s how to apply it practically:

1. **Initial Access**:
   - Monitor for phishing attempts and unusual login activities.
   - Implement strong email filtering and multi-factor authentication.

2. **Execution**:
   - Use endpoint detection and response (EDR) solutions to identify suspicious script execution.
   - Restrict the use of scripting languages like PowerShell to authorized users.

3. **Persistence**:
   - Monitor for the creation of new user accounts and changes to startup scripts.
   - Implement application whitelisting and regular audits of user accounts.

4. **Privilege Escalation**:
   - Detect attempts to exploit vulnerabilities and escalate privileges.
   - Apply the principle of least privilege and conduct regular vulnerability assessments.

5. **Defense Evasion**:
   - Monitor for attempts to disable or bypass security tools.
   - Implement behavioral analysis tools to detect anomalies in security settings.

6. **Credential Access**:
   - Monitor for unauthorized access to credential stores and suspicious login patterns.
   - Use credential vaults and regularly change passwords.

7. **Discovery**:
   - Detect network scanning and enumeration activities.
   - Use network segmentation to limit the visibility of critical systems.

8. **Lateral Movement**:
   - Monitor for unusual authentication attempts and lateral movement techniques.
   - Implement network access controls and micro-segmentation.

9. **Collection**:
   - Detect large-scale data access and aggregation activities.
   - Implement data loss prevention (DLP) solutions to monitor data handling.

10. **Command and Control**:
    - Monitor for unusual outbound connections and encrypted traffic.
    - Use intrusion detection systems (IDS) and threat intelligence to identify C2 channels.

11. **Exfiltration**:
    - Monitor for large data transfers and unusual outbound traffic patterns.
    - Implement DLP solutions to prevent unauthorized data transfers.

12. **Impact**:
    - Detect signs of data destruction or encryption activities.
    - Implement robust backup and recovery procedures to mitigate the effects of ransomware.

## Resources

- **Books**:
  - "Threat Hunting with the MITRE ATT&CK Framework" by Bhushan Lakhe
  - "The MITRE ATT&CK Framework: A Strategy Guide for Cybersecurity Professionals" by Adam Gold

- **Websites**:
  - [MITRE ATT&CK](https://attack.mitre.org/)
  - [Center for Threat-Informed Defense](https://ctid.mitre-engenuity.org/)

- **Tools**:
  - ATT&CK Navigator for visualizing and planning defense strategies.
  - Threat intelligence platforms (e.g., ThreatConnect, Recorded Future)
  - EDR solutions (e.g., CrowdStrike Falcon, Carbon Black)
  - Network monitoring and analysis tools (e.g., Wireshark, Zeek)

By leveraging the MITRE ATT&CK framework, organizations can enhance their threat hunting capabilities, gain a detailed understanding of attacker tactics and techniques, and implement comprehensive defense strategies to protect against sophisticated cyber threats.