---
title: "SOP"
date: 2024-10-19T18:13:21+02:00
draft: false
weight: 3
---

| Revised Date | Comment |
| ------------ | ------- |
| 26.10.2024   | Page added | 

{{% notice info %}}  
This page serves as a template for manual Standard Operating Procedures (SOPs). Please modify as sees fit.
{{% /notice %}}

Template starts below the line

---

| Title             | Phishing Email |
| ----------------- |:-------------- |
| **ID**            | SOP-000001 |
| **Title**         | T1566.001: Spearphishing Attachment |
| **Description**   | Procedure for detecting, analyzing, mitigating, and responding to spearphishing emails containing malicious attachments. |
| **Author**         | [Author Name]                               |
| **Creation Date**  | [Date]                                      |
| **Last Updated**   | [Date of last update]                       |
| **ATT&amp;CK Tactic**  | [TA0001: Initial Access](https://attack.mitre.org/tactics/TA0001) |
| **ATT&amp;CK Technique**  | [T1566.001: Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001) |
| **Tags**          | phishing, spearphishing, malicious attachments |

## Objective

> Define the procedure for detecting, analyzing, mitigating, and responding to spearphishing emails containing malicious attachments, with a focus on preventing the execution of malicious payloads and minimizing the impact on the organization.

## Scope

> This SOP applies to SOC analysts, IT security teams, and any personnel responsible for email security, threat detection, and incident response.

## Procedure

### 1. Detection and Alerting

#### Email Gateway Configuration

- Ensure all inbound emails are scanned by a secure email gateway (SEG) with advanced threat protection (ATP) features.
- Enable attachment scanning with a focus on high-risk file types (e.g., `.exe`, `.zip`, `.docm`), commonly used in phishing attacks.
- Configure sandbox analysis to detonate suspicious attachments, detecting obfuscation techniques such as encoded scripts or embedded macros.

#### Threat Intelligence Integration

- Integrate threat intelligence feeds into the SEG to flag known malicious file hashes, domains, or IP addresses.
- Continuously update signature databases to detect spearphishing campaigns and common malware (e.g., Emotet, TrickBot).

#### User Reporting Mechanism

Set up internal tools (e.g., PhishAlarm) that allow users to report suspicious emails and attachments directly to the SOC for further analysis.

#### Behavioral Monitoring

Use endpoint detection and response (EDR) tools to monitor for suspicious activities associated with opening email attachments, such as unusual file downloads, process creation, or command execution via PowerShell or wscript.

### 2. Initial Analysis

#### Email Header Analysis

Investigate sender domains and IP addresses using threat intelligence databases, and verify email authentication mechanisms (SPF, DKIM, DMARC) to detect spoofed emails.

#### Attachment Static Analysis

- Extract and analyze attachment metadata (e.g., file hash, embedded macros).
- Use tools such as `oletools`, `YARA`, or `exiftool` to identify suspicious code or anomalies in the attachment (e.g., macros in Office files like `.doc` or `.xls`).
- Conduct a file hash lookup through repositories like VirusTotal to check if the file has been flagged as malicious.

#### Attachment Dynamic Analysis

- Use sandboxing solutions (e.g., Cuckoo Sandbox, FireEye AX) to detonate the attachment in a controlled environment. Observe:
  - Network activity (e.g., command-and-control communications)
  - File system modifications (e.g., dropping malware in startup folders)
  - Registry changes and process creation
  - Privilege escalation attempts
- Correlate indicators (e.g., IP addresses, domains, file hashes) with known spearphishing campaigns to identify potential threats.

#### Network Traffic Analysis

- Analyze communication attempts from the attachment (if executed) using network-based threat detection systems (e.g., Suricata, Zeek).
- Investigate DNS logs and URL requests associated with the email to identify connections to malicious domains.

### 3. Containment

#### Quarantine Email

If determined malicious, quarantine the spearphishing email across the organization using email gateway capabilities.

#### Isolate Affected Systems

If the attachment has been executed, isolate the compromised machine from the network to prevent further spread or data exfiltration.

#### Block Malicious Indicators

- Add identified malicious file hashes, IP addresses, and domains to the organization’s blocking lists (e.g., firewall, web proxy, EDR tools).
- Update signatures in intrusion detection/prevention systems (IDS/IPS) and antivirus solutions.

### 4. Eradication

#### Account Compromise

If user credentials were compromised through an attachment (e.g., via a fake login form), reset the affected user's password and enforce multi-factor authentication (MFA) if not already in place.

#### Malware Removal

Use EDR solutions to locate and remove any malicious scripts, processes, or malware that were deployed as a result of the attachment.

#### Artifact Cleanup

Manually or automatically remove malware artifacts, and check for persistence mechanisms such as scheduled tasks, registry run keys, or installed services linked to the spearphishing attachment.

### 5. Recovery

#### System Patching

Ensure all affected systems are updated with the latest security patches, especially for vulnerabilities exploited by spearphishing attachments (e.g., CVEs in Microsoft Office or Adobe products).

#### Backup Restoration

If necessary, restore affected systems from a clean backup, ensuring no malware remnants remain.

### 6. Reporting and Documentation

#### IOC Sharing

Document all indicators of compromise (IOCs) such as malicious file hashes, IP addresses, and domains. Share these IOCs with internal teams and external threat-sharing platforms (e.g., ISACs, VirusTotal).

#### Incident Report

Compile a full incident report detailing:
- The nature of the spearphishing email
- Analysis findings (e.g., sender, attachment details, IOCs)
- Affected users and actions taken (e.g., quarantine, credential resets)
- Lessons learned and recommendations for future prevention

### 7. Preventive Measures

#### Email Filtering Rules

Continuously refine email filtering rules, incorporating detections for advanced spearphishing tactics, such as encrypted or zipped attachments.

#### User Awareness Training

Conduct regular user training and phishing simulations to educate staff on recognizing and reporting spearphishing emails with malicious attachments.

#### Zero Trust Architecture

Implement a zero-trust architecture, where each attachment execution is treated as suspicious, and user access is restricted until verified.

#### Harden Endpoint Security

Enable endpoint security controls such as macro-blocking in Office files, disabling PowerShell for non-admin users, and limiting attachment execution permissions.
