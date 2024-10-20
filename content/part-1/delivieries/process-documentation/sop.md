---
title: "SOP"
date: 2024-10-19T18:13:21+02:00
draft: true
weight: 3
---

| Revised Date | Comment |
| ------------ | ------- |
| 19.10.2024   | Page added | 

{{% notice info %}}
This page serves as a template for manual Standard Operating Procedure (SOP). 
{{% /notice %}}

| Title             | Phishing email |
| ----------------- |:-------------- |
| **ID**            | SOP-000001 |
| **Title**         | T1566.001: Spearphishing Attachment |
| **Description**   | Response playbook for Phishing Email case   |
| **Author**        | Roger Johnsen |
| **Creation Date** | 19.10.2024 |
| **ATT&amp;CK Tactic**  | [TA0001: Initial Access](https://attack.mitre.org/tactics/TA0001) |
| **ATT&amp;CK Technique**  | [T1566.001: Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001) |
| **Tags**          | phishing |

## Objective
To define the procedure for detecting, analyzing, mitigating, and responding to spearphishing emails containing malicious attachments, preventing execution of malicious payloads.

## Scope
This SOP applies to SOC analysts, IT security teams, and any personnel responsible for email security, threat detection, and incident response.

## Procedure

### 1. Detection and Alerting

#### Email Gateway Configuration

- Ensure all email is scanned by a secure email gateway (SEG) with advanced threat protection (ATP) features.
- Enable attachment scanning for known malicious file types (e.g., `.exe`, `.zip`, `.docm`), focusing on file extensions commonly used in phishing attacks.
- Use sandbox analysis to detonate suspicious attachments, detecting obfuscation techniques such as encoded scripts or macros.
  
#### Threat Intelligence Integration

- Integrate threat intelligence feeds into the SEG to flag known malicious file hashes, domains, or IP addresses.
- Continuously update the signature database for common spearphishing tools or payloads like trojans (e.g., Emotet, TrickBot).

#### User Reporting Mechanism

Set up internal tools (e.g., PhishAlarm) that allow users to report suspicious emails and attachments directly to the SOC for analysis.
  
#### Behavioral Monitoring

Use endpoint detection and response (EDR) tools to monitor for suspicious activity triggered by opening email attachments, such as unusual file downloads, process creation, or command execution via PowerShell or wscript.

### 2. Initial Analysis

#### Email Header Analysis

Investigate sender domains and IP addresses using threat intelligence databases and verify email authentication mechanisms (SPF/DKIM/DMARC).
  
#### Attachment Static Analysis

- Extract and analyze the attachment’s file metadata (e.g., file hash, embedded macro scripts).
- Use tools such as `oletools`, `YARA`, or `exiftool` to identify embedded macros, suspicious code, or anomalies in Office documents (e.g., `.doc`, `.xls`).
- Perform a file hash lookup through repositories like VirusTotal to check if the file has previously been flagged as malicious.

#### Attachment Dynamic Analysis
  - Use sandboxing solutions (e.g., Cuckoo Sandbox or FireEye AX) to detonate the attachment in an isolated environment, observing for:
    - Network activity (e.g., C2 communication)
    - File system changes (e.g., dropping malicious files in startup folders)
    - Registry modifications
    - Process creation or privilege escalation attempts
  - Use indicators from the sandbox (IP addresses, domains, file hashes) to correlate with known spearphishing campaigns.

#### Network Traffic Analysis

- Investigate any communication attempts made by the attachment (if executed) using network-based threat detection systems (e.g., Suricata, Zeek).
- Check DNS logs and URL requests associated with the email to identify any connection to malicious domains.

### 3. Containment

#### Quarantine Email

If deemed malicious, immediately quarantine the spearphishing email across the organization using the email gateway’s capabilities.
  
#### Isolate Affected Systems

If the attachment has been executed, isolate the affected machine from the network to prevent lateral movement or data exfiltration.
  
#### Block Malicious Indicators

- Add identified malicious file hashes, IPs, and domains to the organization’s blocking lists (firewall, web proxy, endpoint protection tools).
- Update signatures on intrusion detection/prevention systems (IDS/IPS) and antivirus systems.

### 4. Eradication

#### Account Compromise

If user credentials are suspected of being phished through an attachment (e.g., using a fake login form), reset the affected user’s credentials and force MFA enrollment, if not already in place.
  
#### Script/Process Removal

Use EDR solutions to hunt and remove any malicious scripts or processes spawned by the attachment.
  
#### Clean Up Artifacts

Manually or automatically remove any malware dropped onto the system, and check for persistence mechanisms such as scheduled tasks, registry run keys, or services installed by the spearphishing attachment.

### 5. Recovery

#### Patch Systems 

Ensure all systems are up-to-date with security patches, especially vulnerabilities exploited by spearphishing attachments (e.g., CVE-based exploits in Microsoft Office or Adobe).
  
#### Restore from Backup

If necessary, restore affected systems from backups, ensuring that no malware artifacts remain.

### 6. Reporting and Documentation

#### IOC Sharing

Document all IOCs (Indicators of Compromise) such as malicious file hashes, IP addresses, and domains. Share these indicators with internal teams and external threat-sharing platforms (e.g., ISACs, VirusTotal).
  
#### Incident Report

Compile a full incident report including details of the email, attachment, analysis results, affected users, actions taken, and lessons learned. This report should be used for future training and prevention.

### 7. Preventive Measures

#### Email Filtering Rules

Continuously refine email filtering rules, adding checks for advanced spearphishing tactics such as encrypted attachments or zipped files.
  
#### User Awareness

Conduct periodic training and phishing simulations, educating staff on recognizing spearphishing attachments and reporting them.
  
#### Zero Trust Architecture

Implement a zero-trust architecture where each execution of an attachment is treated as suspicious, and access is limited until verified.

#### Harden Endpoint Security

Enable security controls such as macro-blocking in Office files, disabling PowerShell for non-admin users, and limiting attachment execution rights on end-user machines.