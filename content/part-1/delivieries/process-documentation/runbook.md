---
title: "Runbook"
date: 2024-10-19T18:13:15+02:00
draft: true
weight: 1
---

| Revised Date | Comment |
| ------------ | ------- |
| 19.10.2024   | Page added | 

{{% notice info %}}
This page serves as a template for runbooks. 
{{% /notice %}}

Here’s an example of a **Runbook** for responding to a **suspicious email** incident:

---

### **Runbook: Suspicious Email Investigation and Response**

#### **Objective**:
To provide SOC analysts with a step-by-step guide for investigating and responding to reports of suspicious emails, ensuring the security of the organization and preventing phishing, malware infection, or data breaches.

#### **Pre-requisites**:
- Access to the organization's email security gateway (e.g., Proofpoint, Mimecast)
- Access to endpoint detection and response (EDR) tools (e.g., CrowdStrike, Carbon Black)
- Knowledge of sandboxing systems (e.g., FireEye, Cuckoo)
- Incident tracking system (e.g., ServiceNow)

---

### **Step-by-Step Process**:

#### **1. User Report of Suspicious Email**
1. **Action**: User reports an email that appears suspicious via the SOC’s designated reporting mechanism (e.g., PhishAlarm, forwarding to SOC email).
2. **Verify Report**: Check if other users have reported the same email. Review email content for immediate red flags (e.g., misspellings, unexpected attachments, urgent requests for sensitive information).

#### **2. Initial Triage**
1. **Action**: Analyze the email header to determine its origin:
    - Use email tools to check the sender's domain and IP reputation.
    - Verify email authentication (SPF, DKIM, DMARC).
2. **Attachment/Link Analysis**: If the email contains attachments or links:
    - **Attachments**: Download and analyze using a sandbox environment.
    - **Links**: Use URL analysis tools (e.g., VirusTotal, URLscan) to assess if they are malicious or redirect to known phishing sites.
3. **Search for IOCs**: Correlate any known indicators of compromise (IOCs) like file hashes, domains, or IP addresses with threat intelligence platforms.

#### **3. Threat Containment (if malicious)**
1. **Quarantine Email**: 
   - If determined malicious, use the email security gateway to quarantine the email across the organization.
2. **Block Malicious Links/Attachments**: 
   - Update email and web filtering rules to block malicious domains or file hashes identified during analysis.
3. **Account Isolation**: 
   - If a user clicked a malicious link or opened the attachment, isolate their endpoint using EDR tools.

#### **4. Full Investigation**
1. **Log Review**: Check network and endpoint logs for any signs of compromise:
    - Monitor for unusual outbound traffic to suspicious IPs/domains.
    - Review process creation logs for any evidence of malware execution.
2. **System Scan**: Conduct a full scan of the affected endpoint for malware, using antivirus and EDR tools.
3. **Threat Hunt**: Use threat hunting tools to search for additional instances of the email or similar IOCs across the environment.

#### **5. Eradication and Remediation**
1. **Remove Malicious Content**: Delete malicious emails and any related malware from affected systems.
2. **Reset Credentials**: For compromised accounts, force a password reset and enable multi-factor authentication (MFA) if not already enabled.
3. **Patch Vulnerabilities**: Ensure any vulnerabilities exploited during the attack (e.g., unpatched email client software) are remediated.

#### **6. Recovery**
1. **Account Restoration**: Once the threat is neutralized, restore access to quarantined accounts.
2. **Endpoint Reimaging**: If the endpoint is severely compromised, reimage the system and restore from backup.
   
#### **7. Reporting and Documentation**
1. **Incident Report**: Document the incident thoroughly in the incident tracking system:
   - Nature of the email (e.g., phishing, malware)
   - Analysis findings (e.g., sender info, attachment details, IOCs)
   - Containment actions taken
   - Resolution steps
2. **Post-Incident Review**: Conduct a review meeting to evaluate response effectiveness and improve future procedures.

#### **8. Preventive Measures**

1. **Security Awareness Training**: Ensure users are regularly trained on identifying suspicious emails.
2. **Phishing Simulations**: Conduct periodic phishing tests to improve user awareness and response.
3. **Update Security Policies**: Modify email and network security policies to strengthen defenses based on lessons learned.

---

### **Additional Notes**:
- **Time-Sensitive**: Immediate containment actions (e.g., quarantining the email, isolating the affected system) should be prioritized.
- **Automation**: Where possible, integrate automation into the response process to reduce manual effort.

---

This runbook provides a comprehensive, step-by-step process to handle suspicious email incidents, ensuring effective and standardized actions within the SOC.