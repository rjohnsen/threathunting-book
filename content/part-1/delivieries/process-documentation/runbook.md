---
title: "Runbook"
date: 2024-10-19T18:13:15+02:00
draft: false
weight: 1
---

| Revised Date | Comment |
| ------------ | ------- |
| 25.10.2024   | Page added | 

{{% notice info %}} 
This page serves as a template for runbooks. Please modify as sees fit.
{{% /notice %}}

Template starts below the line

---

| **Title**          | Suspicious Email Investigation and Response |
| ------------------ |:------------------------------------------- |
| **Runbook ID**     | RB-000001                                   |
| **Description**    | Step-by-step guide for investigating and responding to suspicious email reports |
| **Author**         | [Author Name]                               |
| **Creation Date**  | [Date]                                      |
| **Last Updated**   | [Date of last update]                       |
| **Severity**       | Medium                                      |
| **ATT&amp;CK Tactic**   | [TA0001: Initial Access](https://attack.mitre.org/tactics/TA0001) |
| **ATT&amp;CK Technique**| [T1566.001: Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001) |
| **Tags**           | Phishing, Email Security, Threat Detection  |

#### Objective

> To provide SOC analysts with a step-by-step guide for investigating and responding to reports of suspicious emails, ensuring the security of the organization and preventing phishing, malware infection, or data breaches.

#### Pre-requisites

- Access to the organization's email security gateway (e.g., Proofpoint, Mimecast)
- Access to endpoint detection and response (EDR) tools (e.g., CrowdStrike, Carbon Black)
- Knowledge of sandboxing systems (e.g., FireEye, Cuckoo)
- Access to the incident tracking system (e.g., ServiceNow)

### Step-by-Step Process

#### 1. User Report of Suspicious Email

1. **Action**: User reports an email that appears suspicious via the SOC’s designated reporting mechanism (e.g., PhishAlarm, forwarding to SOC email).
2. **Verify Report**: Check if other users have reported the same email. Review the email content for immediate red flags (e.g., misspellings, unexpected attachments, urgent requests for sensitive information).

#### 2. Initial Triage

1. **Action**: Analyze the email header to determine its origin:
    - Use email tools to check the sender's domain and IP reputation (e.g., MXToolbox).
    - Verify email authentication (SPF, DKIM, DMARC) results to determine the legitimacy of the sender.
2. **Attachment/Link Analysis**: If the email contains attachments or links:
    - **Attachments**: Download and analyze in a sandbox environment to detect malicious behavior.
    - **Links**: Use URL analysis tools (e.g., VirusTotal, URLscan) to assess whether they are malicious or redirect to known phishing sites.
3. **Search for IOCs**: Cross-reference any known indicators of compromise (IOCs) like file hashes, domains, or IP addresses with threat intelligence platforms.

#### 3. Threat Containment (if malicious)

1. **Quarantine Email**: 
   - If confirmed malicious, use the email security gateway to quarantine the email across the organization.
2. **Block Malicious Links/Attachments**: 
   - Update email and web filtering rules to block malicious domains or file hashes identified during analysis.
3. **Account Isolation**: 
   - If a user interacted with malicious content (clicked a link or opened an attachment), isolate their endpoint using EDR tools.

#### 4. Full Investigation

1. **Log Review**: Examine network and endpoint logs for any signs of compromise:
    - Monitor for unusual outbound traffic to suspicious IPs/domains.
    - Review process creation and command-line execution logs for evidence of malware activity.
2. **System Scan**: Run a full antivirus and EDR scan of the affected endpoint to detect and remove malware.
3. **Threat Hunt**: Utilize threat hunting tools to search for additional instances of the email or related IOCs across the network.

#### 5. Eradication and Remediation

1. **Remove Malicious Content**: Delete malicious emails and remove any related malware from affected systems.
2. **Reset Credentials**: For compromised accounts, initiate a password reset and ensure multi-factor authentication (MFA) is enabled if not already enforced.
3. **Patch Vulnerabilities**: Remediate any vulnerabilities exploited during the attack (e.g., unpatched email clients or operating systems).

#### 6. Recovery

1. **Account Restoration**: Once the threat is fully mitigated, restore access to quarantined accounts.
2. **Endpoint Reimaging**: If the endpoint is severely compromised, reimage the system and restore data from a secure backup.

#### 7. Reporting and Documentation

1. **Incident Report**: Document the incident thoroughly in the incident tracking system, including:
   - The nature of the email (e.g., phishing, malware)
   - Findings from the analysis (e.g., sender details, attachment information, IOCs)
   - Containment and eradication actions taken
   - Recovery and remediation steps
2. **Post-Incident Review**: Conduct a review meeting to evaluate the effectiveness of the response process and identify areas for improvement.

#### 8. Preventive Measures

1. **Security Awareness Training**: Continuously educate users on identifying and reporting suspicious emails.
2. **Phishing Simulations**: Run regular phishing simulations to test user awareness and enhance their ability to respond.
3. **Update Security Policies**: Adjust email and network security policies to strengthen defenses based on the lessons learned from the incident.

### Additional Notes

- **Time-Sensitive Actions**: Immediate containment actions (e.g., quarantining emails, isolating affected systems) should be prioritized to prevent further spread.
- **Automation Integration**: Where feasible, integrate automation into response workflows to streamline tasks like email analysis, IOCs correlation, and account isolation.