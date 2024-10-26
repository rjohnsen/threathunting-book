---
title: "Playbook"
date: 2024-10-19T18:13:00+02:00
draft: false
weight: 2
---

| Revised Date | Comment |
| ------------ | ------- |
| 25.10.2024   | Page added | 

{{% notice info %}}
This page serves as a template for manual playbooks. Please modify as sees fit.
{{% /notice %}}

Template starts below the line

---

| Title             | Phishing email |
| ----------------- |:-------------- |
| **ID**                | PB-000001 |
| **Description**       | Response playbook for Phishing Email case |
| **Author**         | [Author Name]                               |
| **Creation Date**  | [Date]                                      |
| **Last Updated**   | [Date of last update]                       |
| **Severity**          | Medium |
| **[TLP](https://www.cisa.gov/news-events/news/traffic-light-protocol-tlp-definitions-and-usage)** | AMBER |
| **[PAP](https://cert.ssi.gouv.fr/csirt/sharing-policy/)** | WHITE |
| **[ATT&CK Tactic](https://attack.mitre.org/tactics)** | [TA0001: Initial Access](https://attack.mitre.org/tactics/TA0001) |
| **[ATT&CK Technique](https://attack.mitre.org/techniques)** | [T1566.001: Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001), [T1566.002: Spearphishing Link](https://attack.mitre.org/techniques/T1566/002) |
| **Tags**           | phishing, phishing response, email threats, incident management |

## Purpose

> To provide a structured response to phishing incidents, ensuring swift action and proper documentation.

## Overview

> This playbook outlines the steps to take when a phishing incident is identified.

## Steps

{{% notice tip %}}
For this Playbook to be useful, you should for each step include links to relevant Standard Operating Procedures (SOPs). Examples are provided by the end of every sub-step here ("Link to SOP"), but please change these to suit your organizational setup.
{{% /notice %}}

### 1. Identification

- Confirm if the reported email is indeed a phishing attempt. [Link to SOP]().
- Verify the sender's address and any suspicious links. [Link to SOP]().

### 2. Containment

- Notify the affected user not to click on any links or provide personal information. [Link to SOP]().
- Block the sender’s email address in the email security gateway. [Link to SOP]().

### 3. Analysis

- Analyze the phishing email for indicators of compromise (IOCs). [Link to SOP]().
- Identify any potentially impacted accounts or systems. [Link to SOP]().

### 4. Eradication

- Remove the phishing email from all affected users' inboxes. [Link to SOP]().
- Reset passwords for any accounts that may have been compromised. [Link to SOP]().

### 5. User Education

- Conduct a training session for all employees on recognizing phishing attempts. [Link to SOP]().
- Share examples of the phishing email with the team. [Link to SOP]().

### 6. Reporting

- Document the incident in the incident management system. [Link to SOP]().
- Report the phishing attempt to relevant authorities (e.g., anti-phishing organizations). [Link to SOP]().

### 7. Review

- Conduct a post-incident review to evaluate the response process. [Link to SOP]().
- Update training materials and security awareness programs based on lessons learned. [Link to SOP]().
