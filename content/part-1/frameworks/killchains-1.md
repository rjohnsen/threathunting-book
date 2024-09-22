---
title: "Lockheed Martin Killchain"
date: 2024-06-22T10:17:04+02:00
draft: false
weight: 2
---

## About Model

Lockheed Martin's Cyber Kill Chain is a security framework designed to help organizations understand and counteract cyber threats. The model outlines the stages of a cyberattack, from initial reconnaissance to the final objective. By identifying and interrupting these stages, organizations can improve their threat detection and response capabilities.

The Cyber Kill Chain consists of seven stages:

1. **Reconnaissance**: Attackers gather information about the target.
2. **Weaponization**: Attackers create a deliverable payload.
3. **Delivery**: The payload is transmitted to the target.
4. **Exploitation**: The payload is executed on the target system.
5. **Installation**: Malware or backdoors are installed.
6. **Command and Control (C2)**: Attackers establish communication with the compromised system.
7. **Actions on Objectives**: Attackers achieve their goals, such as data exfiltration or system damage.

This is the official depiction of the model from [Lockheed Martin](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html):

![Lockheed Martin Cyber Killchain](/images/THE-CYBER-KILL-CHAIN-body.png.pc-adaptive.1280.medium.png)

## Example

Consider a hypothetical scenario where a financial institution is targeted by a cyberattack. Using the Cyber Kill Chain model, the institution's security team can map out the attack as follows:

1. **Reconnaissance**: Attackers scan the institution’s public-facing websites and social media for vulnerabilities.
2. **Weaponization**: They craft a phishing email containing a malicious PDF.
3. **Delivery**: The phishing email is sent to an employee.
4. **Exploitation**: The employee opens the PDF, triggering a vulnerability in the PDF reader.
5. **Installation**: A remote access Trojan (RAT) is installed on the employee’s computer.
6. **Command and Control**: The RAT connects back to the attacker’s server.
7. **Actions on Objectives**: Attackers move laterally through the network to access sensitive financial data.

## Practical Application

In threat hunting, the Cyber Kill Chain can be used to proactively search for indicators of compromise (IOCs) and anomalous activities within each stage of the attack process. Here’s how:

1. **Reconnaissance**:
   - Monitor for unusual scanning activities and attempts to access public information.
   - Use threat intelligence feeds to identify potential attacker IP addresses.

2. **Weaponization**:
   - Analyze email attachments and web downloads for malware signatures.
   - Use sandbox environments to detonate suspicious files.

3. **Delivery**:
   - Implement email filtering and web gateway protections to block malicious content.
   - Educate employees on phishing tactics to reduce successful delivery rates.

4. **Exploitation**:
   - Patch vulnerabilities promptly to prevent exploitation.
   - Use endpoint detection and response (EDR) tools to identify exploitation attempts.

5. **Installation**:
   - Monitor for changes in system files and registry entries.
   - Use application whitelisting to prevent unauthorized software installations.

6. **Command and Control**:
   - Monitor network traffic for unusual outbound connections.
   - Use intrusion detection systems (IDS) to identify known C2 patterns.

7. **Actions on Objectives**:
   - Monitor for unusual data access and exfiltration activities.
   - Implement data loss prevention (DLP) solutions to protect sensitive information.

## Resources

- **Books**:
  - "The Cyber Kill Chain: Threat Hunting Across the Attack Lifecycle" by Steven Stasiukonis
  - "The Hacker Playbook 3: Practical Guide To Penetration Testing" by Peter Kim

- **Websites**:
  - [Lockheed Martin Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html)
  - [MITRE ATT&CK Framework](https://attack.mitre.org/)

- **Tools**:
  - Threat intelligence platforms (e.g., ThreatConnect, Recorded Future)
  - EDR solutions (e.g., CrowdStrike Falcon, Carbon Black)
  - Network monitoring tools (e.g., Wireshark, Zeek)

By leveraging the Cyber Kill Chain model, organizations can enhance their threat hunting processes, improve their security posture, and effectively mitigate cyber threats.

