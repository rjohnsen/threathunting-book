---
title: "SOP"
date: 2024-10-19T18:13:21+02:00
draft: false
weight: 3
---

| Revised Date | Comment |
| ------------ | ------- |
| 22.02.2025   | Improved formatting and revised wording | 

{{% notice info %}}  
This page serves as a template for manual Standard Operating Procedures (SOPs). Please modify as sees fit.
{{% /notice %}}

Template starts below the line. Example of applied template follows template below.

---

| Title             | [SOP Title] |
| ----------------- |:-------------- |
| **ID**            | [Unique Document ID, e.g., SOP-000001] |
| **Description**   | [Brief summary of the SOP’s purpose] |
| **Author**        | [Author Name or Team] |
| **Creation Date** | [Date] |
| **Last Updated**  | [Date of last revision] |
| **Associated Frameworks** | [Relevant security frameworks, e.g., NIST, MITRE ATT&CK] |
| **Tags**          | [Keywords related to the SOP] |

---

## 1. Objective

> [Clearly define the goal of this SOP. What is it intended to achieve?]

---

## 2. Scope

> [Specify who this SOP applies to, such as SOC analysts, IT security teams, system administrators, etc.]

> [Define the systems, processes, and environments covered by this SOP.]

---

## 3. Roles and Responsibilities

| Role | Responsibilities |
|------|----------------|
| [Role Name] | [Responsibilities related to this SOP] |
| [Role Name] | [Responsibilities related to this SOP] |

---

## 4. Prerequisites and Dependencies

- [List any tools, access permissions, or setup required before executing this SOP.]
- [Mention dependencies on other SOPs, policies, or external teams.]

---

## 5. Procedure

### **Step 1: Initial Actions**
- [Define the first actions to take when executing this procedure.]
- [Include automated alerts, monitoring tools, or initial response steps.]

### **Step 2: Data Collection & Analysis**
- [Describe data sources and analysis methods.]
- [Specify logs, threat intelligence feeds, or forensic tools used.]

### **Step 3: Containment & Mitigation**
- [Outline how to prevent further impact (e.g., isolating affected systems, blocking malicious indicators).]

### **Step 4: Eradication**
- [Detail steps to remove threats or resolve the issue permanently.]

### **Step 5: Recovery & Validation**
- [Define actions to restore normal operations and ensure the issue is fully resolved.]

### **Step 6: Reporting & Documentation**
- [Specify how findings should be documented and reported.]
- [List required details for an incident report or post-mortem review.]

### **Step 7: Preventive Measures & Continuous Improvement**
- [Outline best practices to reduce the likelihood of future occurrences.]
- [Specify updates to security policies, user training, or automation improvements.]

---

## 6. Escalation & Exception Handling

- [Define escalation procedures if the standard response is insufficient.]
- [Specify exception handling for cases where the SOP may not fully apply.]

---

## 7. References

> [Provide links to relevant policies, standards, frameworks, or related SOPs.]

---

## 8. Change Log

| Date | Change Description | Updated By |
|------|-------------------|------------|
| [Date] | [Summary of change] | [Author] |
| [Date] | [Summary of change] | [Author] |

--- 

## Example

| Title             | Detection and Response to Spearphishing Attachments |
| ----------------- |:--------------------------------------------------- |
| **ID**            | SOP-2024-001 |
| **Description**   | Procedure for identifying, analyzing, mitigating, and responding to spearphishing emails containing malicious attachments. |
| **Author**        | Security Operations Center (SOC) Team |
| **Creation Date** | 2024-10-05 |
| **Last Updated**  | 2024-10-05 |
| **Associated Frameworks** | [MITRE ATT&CK: [T1566.001 - Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001)] |
| **Tags**          | phishing, spearphishing, malicious attachments, email security |

---

## 1. Objective

> This SOP defines the steps required to detect, analyze, mitigate, and respond to spearphishing emails containing malicious attachments. The primary goal is to prevent the execution of malicious payloads and minimize the risk to the organization.

---

## 2. Scope

> This SOP applies to SOC analysts, IT security teams, and any personnel responsible for email security, threat detection, and incident response.  
> It covers spearphishing attempts that use malicious attachments to deliver malware or steal credentials.

---

## 3. Roles and Responsibilities

| Role | Responsibilities |
|------|----------------|
| SOC Analysts | Monitor for and investigate suspected spearphishing emails. |
| IT Security Team | Manage email security policies and implement mitigations. |
| Incident Response Team | Contain and remediate compromised systems. |
| End Users | Report suspicious emails and avoid interacting with unverified attachments. |

---

## 4. Prerequisites and Dependencies

- Access to security tools: Email Security Gateway (Proofpoint), SIEM (OpenSearch), Endpoint Detection & Response (CrowdStrike Falcon), Threat Intelligence feeds (VirusTotal, MISP).  
- Threat intelligence sources: Internal reports, external feeds (VirusTotal, URLhaus, AbuseIPDB).  
- Sandbox environment: Cuckoo Sandbox for safe detonation of suspicious attachments.  
- Logging and alerting configuration: Ensure logs from email gateways, endpoints, and network traffic analysis tools are ingested into OpenSearch.

---

## 5. Procedure

### **Step 1: Detection and Initial Alerting**

**Email Gateway Monitoring**
- Ensure inbound emails pass through a Secure Email Gateway (Proofpoint) with advanced threat protection.
- Configure the SEG to flag emails containing high-risk attachment types (`.exe`, `.zip`, `.docm`, `.js`).
- Enable sandbox analysis for automated detonation of suspicious attachments.

**Threat Intelligence Integration**
- Correlate incoming email attachments with known malicious hashes.
- Enrich alerts using external intelligence sources (VirusTotal, AbuseIPDB, URLhaus).
- Identify patterns from ongoing phishing campaigns.

**User Reporting**
- Ensure users can report suspicious emails via a report phishing button in Outlook.
- Automate SOC alerts when multiple reports of the same email occur.

---

### **Step 2: Initial Analysis**

**Email Header Analysis**
- Verify sender domain, SPF, DKIM, and DMARC** records.
- Identify anomalies in email routing paths.

**Attachment Static Analysis**
- Extract file hashes and check against threat intelligence databases.
- Analyze embedded macros, scripts, or encoded payloads.
- Use tools such as `oletools`, `exiftool`, and `YARA` for pattern detection.

**Attachment Dynamic Analysis**
- Detonate the file in Cuckoo Sandbox.
- Observe:
  - Network activity (e.g., C2 beaconing)
  - File system modifications
  - Process injection or privilege escalation attempts

**Network Traffic Analysis**
- Check DNS logs and URL requests generated by the attachment.
- Flag connections to known phishing infrastructure.

---

### **Step 3: Containment and Mitigation**

**Email Quarantine**
- If confirmed malicious, quarantine the email across all user inboxes.
- Notify affected users and relevant teams.

**Affected System Isolation**
- If the attachment was opened and malware executed, isolate the machine.
- Collect forensic evidence before remediation.

**Blocking Malicious Indicators**
- Add malicious file hashes, domains, and IPs** to block lists (firewall, proxy, EDR).
- Update IDS/IPS signatures for future detections.

---

### **Step 4: Eradication**

**Credential Reset**
- If the phishing attempt aimed at stealing credentials, enforce a password reset.
- Require multi-factor authentication (MFA).

**Malware Removal**
- Use CrowdStrike Falcon to locate and remove malware.
- Investigate persistence mechanisms (registry keys, scheduled tasks).

**Artifact Cleanup**
- Manually remove phishing-related artifacts.
- Monitor for secondary infections.

---

### **Step 5: Recovery & Validation**

**System Hardening**
- Patch email security solutions and endpoint protections.
- Implement stricter attachment filtering policies.

**Backup Restoration**
- If data was compromised, restore from a clean backup.

---

### **Step 6: Reporting & Documentation**

**IOC Sharing**
- Document Indicators of Compromise (IOCs) such as:
  - Malicious file hashes (`d41d8cd98f00b204e9800998ecf8427e`)
  - Phishing URLs (`hxxp://malicious-example.com/login`)
  - C2 server IPs (`192.168.1.123`)
- Share findings with internal teams and external threat-sharing platforms (MISP, VirusTotal).

**Incident Report**
- Create a detailed report including:
  - Summary of the spearphishing attempt
  - Analysis findings** (sender, attachment details, behavior)
  - Affected users and actions taken
  - Lessons learned and recommendations

---

### **Step 7: Preventive Measures & Continuous Improvement**

**Email Security Enhancements**
- Enforce stricter email filtering rules for attachments.
- Implement DMARC quarantine/reject policies.

**User Awareness Training**
- Conduct regular phishing simulations.
- Educate employees on phishing red flags.

**Automation & Zero Trust**
- Deploy automated phishing detection playbooks in OpenSearch.
- Enforce zero-trust execution policies for email attachments.

---

## 6. Escalation & Exception Handling

- Escalate to Incident Response Team if a confirmed breach occurs.
- If false positives occur frequently, refine detection rules in SEG and SIEM.

---

## 7. References

- [MITRE ATT&CK T1566.001 - Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001)
- [NIST 800-61 - Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
- [CISA Phishing Awareness](https://www.cisa.gov/resources-tools/resources/phishing-awareness)

---

## 8. Change Log

| Date | Change Description | Updated By |
|------|-------------------|------------|
| 2024-10-05 | Initial version | SOC Team |
| 2025-10-05 | Added sandboxing details | SOC Lead |