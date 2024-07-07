---
title: "Unified Kill Chain"
date: 2024-07-07T11:39:29+02:00
draft: false
weight: 3
---

{{% notice info %}}
This public preview offers a glimpse of upcoming content. Please note that the content may be subject to change without prior notice as I continue to develop this site.
{{% /notice %}}

## About Model

The Unified Kill Chain (UKC) is a comprehensive cybersecurity framework that details the lifecycle of cyberattacks. It integrates aspects of various models, including the Cyber Kill Chain and MITRE ATT&CK, into a unified structure. The UKC categorizes the attack lifecycle into three main phases: **In, Through, and Out**.

### Why "In, Through, Out"?
This division helps organizations systematically understand and address each phase of an attack:
- **In**: How attackers gain initial access.
- **Through**: How they move within the network.
- **Out**: How they achieve their objectives and exit.

### Phases

Notice that each phase is built of multiple steps. For instance, the steps in the "IN" phase is 

* Reconnaissance
* Resource development
* Delivery
* Social Engineering
* Exploitation
* Persistence
* Defense evasion
* Command and control

Bear in mind, in an attack, an adversary doesn't have to fulfill or pace through each step. Some steps can and will be skipped depending on the TTP's of the adversary. Speaking of TTP's, if you look closely, each step in each phase can (with some exceptions outlined in the design PDF) be mapped with the Mitre Att&ck Framework. This means, as I use the model, whenever I do threat hunting I use this model as a notebook (along with the Diamond Model). By carefully mapping out whatever the adversay are trying to do up against the Mitre Att&ck framework, I can place which phase attack is in. Paired with the Diamond Model I have a great system for basing my reports and further investigation on.

Anyway, let's have a closer look at each phases: 

1. **In**:

The "in" phase is about:

   - Preparation
   - Initial Compromise
   - Establish Foothold

![Unified Kill chain In Phase 1](/images/ukc1.png)

2. **Through**:

The "through" phase is about:

   - Escalate Privileges
   - Internal Reconnaissance
   - Move Laterally

![Unified Kill chain In Phase 2](/images/ukc2.png)

3. **Out**:

The "out" phase is about:

   - Maintain Presence
   - Complete Mission
   - Exfiltration & Impact

![Unified Kill chain In Phase 3](/images/ukc1.png)

## Example

Consider a cyberattack on a financial institution using the UKC framework:

1. **In**:
   - **Preparation**: Attackers gather information on employees.
   - **Initial Compromise**: Phishing email delivers malware.
   - **Establish Foothold**: Malware installs a backdoor.

2. **Through**:
   - **Escalate Privileges**: Attackers exploit vulnerabilities for admin rights.
   - **Internal Reconnaissance**: Network scans identify critical systems.
   - **Move Laterally**: Attackers use stolen credentials to access other systems.

3. **Out**:
   - **Maintain Presence**: Persistent backdoors ensure ongoing access.
   - **Complete Mission**: Attackers exfiltrate sensitive financial data.
   - **Exfiltration & Impact**: Data is transferred out, and ransomware is deployed.

## Practical Application

### In
- **Detection**: Monitor for reconnaissance and phishing attempts.
- **Response**: Conduct security awareness training and implement email filtering.

### Through
- **Detection**: Identify privilege escalation and lateral movement.
- **Response**: Enforce least privilege and use network segmentation.

### Out
- **Detection**: Monitor for data exfiltration and ransomware activity.
- **Response**: Deploy data loss prevention tools and maintain regular backups.

## Why It Is Better Than Lockheed Martin's Cyber Kill Chain

The Unified Kill Chain is considered more effective than Lockheed Martin's Cyber Kill Chain for several reasons:

1. **Comprehensiveness**: The UKC encompasses a broader range of attack techniques and stages, covering the entire attack lifecycle in more detail.
2. **Integration**: It integrates insights from multiple models, including MITRE ATT&CK, providing a more holistic view.
3. **Flexibility**: The division into "In, Through, Out" allows for more targeted and specific defensive measures at each phase.
4. **Depth**: It addresses more advanced and sophisticated attack techniques, enabling organizations to defend against a wider array of threats.

By leveraging the Unified Kill Chain, organizations can systematically enhance their threat detection and response capabilities, improving overall security posture and reducing the likelihood of successful attacks.

## Resources

- **Books**:
  - "The Unified Kill Chain: A Comprehensive Guide to Cyber Defense" by Paul Pols

- **Websites**:
  - [Unified Kill Chain Documentation](https://www.unifiedkillchain.com/assets/The-Unified-Kill-Chain.pdf)
  - [MITRE ATT&CK Framework](https://attack.mitre.org/)

- **Tools**:
  - Threat intelligence platforms
  - Endpoint detection and response (EDR) solutions
  - Network monitoring tools
  - Security information and event management (SIEM) systems
