---
title: "Pyramid of Pain"
date: 2024-06-23T20:15:33+02:00
draft: false
weight: 6
tags:
    - framework
summary: ""
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Improved formatting and wording | 

## Introduction

**The Pyramid of Pain is a conceptual model created by David J. Bianco that illustrates the relative difficulty and impact of detecting and responding to different types of indicators used in threat hunting and incident response. The model helps cybersecurity professionals understand the value of various types of indicators and how effectively they can disrupt adversary activities.**

---

## About Model

The Pyramid of Pain consists of six levels:

![Pyramid of pain](/images/Pyramid-of-Pain-v2.png)

Each level representing a type of indicator and the associated pain (difficulty) it causes to the adversary when detected and mitigated:

1. **Hash Values**: Specific file hashes (e.g., MD5, SHA-1).
2. **IP Addresses**: Specific IP addresses used by adversaries.
3. **Domain Names**: Specific domain names used by adversaries.
4. **Network/Host Artifacts**: Specific characteristics of network traffic or host-based artifacts.
5. **Tools**: Specific tools used by adversaries.
6. **Tactics, Techniques, and Procedures (TTPs)**: The adversary’s methods and strategies.

The higher the indicator on the pyramid, the more pain it causes the adversary when detected and mitigated, as it forces them to change their tactics, tools, or procedures, which is more challenging and time-consuming than merely changing a hash or IP address.

## Example

Consider a scenario where a company detects a cyberattack using the Pyramid of Pain:

1. **Hash Values**: The company identifies and blocks a known malicious file by its hash.
2. **IP Addresses**: The attacker's command and control (C2) server IP address is detected and blocked.
3. **Domain Names**: The domain name used for phishing emails is identified and blocked.
4. **Network/Host Artifacts**: Unusual patterns in network traffic are detected, such as specific HTTP headers used by malware.
5. **Tools**: The attacker is using the Mimikatz tool for credential dumping, which is detected and blocked.
6. **TTPs**: The attacker uses specific techniques for lateral movement (e.g., Pass-the-Hash), which are identified and mitigated by enhancing detection and response strategies.

## Practical Application

In threat hunting, the Pyramid of Pain can be used to prioritize detection and response efforts based on the impact on the adversary:

1. **Hash Values**:
   - **Detection**: Use file integrity monitoring and antivirus software to detect malicious hashes.
   - **Response**: Block detected hashes and share them with threat intelligence communities.

2. **IP Addresses**:
   - **Detection**: Monitor network traffic for connections to known malicious IP addresses.
   - **Response**: Block detected IP addresses at the firewall and update network blacklists.

3. **Domain Names**:
   - **Detection**: Use DNS filtering and threat intelligence feeds to identify malicious domains.
   - **Response**: Block detected domains and update DNS blacklists.

4. **Network/Host Artifacts**:
   - **Detection**: Analyze network traffic and host logs for unusual patterns or artifacts.
   - **Response**: Implement detection rules and alerts for identified artifacts.

5. **Tools**:
   - **Detection**: Monitor for known tools and their signatures (e.g., Mimikatz).
   - **Response**: Block execution of detected tools and enhance endpoint security measures.

6. **TTPs**:
   - **Detection**: Use the MITRE ATT&CK framework to identify and detect adversary techniques.
   - **Response**: Develop and implement detection and mitigation strategies for identified TTPs.

### Example Scenario

Suppose an organization detects an ongoing attack. By leveraging the Pyramid of Pain, they can disrupt the adversary at various levels:

1. **Hash Values**: They detect a malicious file hash and block it.
2. **IP Addresses**: They identify the C2 server's IP address and block it.
3. **Domain Names**: They block the phishing domain used by the attacker.
4. **Network/Host Artifacts**: They detect unusual network traffic patterns indicative of the attack and alert the security team.
5. **Tools**: They identify and block the use of Mimikatz by monitoring for its signatures.
6. **TTPs**: They enhance their detection and response capabilities to mitigate lateral movement techniques used by the attacker.

## Resources

- **Books**:
  - "The Threat Hunter Playbook: A Practical Guide to Hunt and Detect Advanced Threats" by J. H. Rose
  - "Threat Hunting with the MITRE ATT&CK Framework" by Bhushan Lakhe

- **Websites**:
  - [Pyramid of Pain by David J. Bianco](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html)
  - [MITRE ATT&CK Framework](https://attack.mitre.org/)

- **Tools**:
  - Threat intelligence platforms (e.g., ThreatConnect, Recorded Future)
  - Endpoint detection and response (EDR) solutions (e.g., CrowdStrike Falcon, Carbon Black)
  - Network monitoring and analysis tools (e.g., Wireshark, Zeek)

By applying the Pyramid of Pain model in threat hunting, organizations can prioritize their efforts to cause maximum disruption to adversaries, making it more challenging and costly for them to achieve their objectives.
