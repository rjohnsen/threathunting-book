---
title: "Playbook"
date: 2024-10-19T18:13:00+02:00
draft: true
weight: 2
---

| Revised Date | Comment |
| ------------ | ------- |
| 19.10.2024   | Page added | 

{{% notice info %}}
This page serves as a template for manual playbooks. 
{{% /notice %}}

| Title             | Phishing email |
| ----------------- |:-------------- |
| ID            | PB-000001 |
| Description   | Response playbook for Phishing Email case   |
| Author        | Roger Johnsen |
| Creation Date | 19.10.2024 |
| Severity      | Medium |
| [TLP](https://www.cisa.gov/news-events/news/traffic-light-protocol-tlp-definitions-and-usage) | AMBER |
| [PAP](https://cert.ssi.gouv.fr/csirt/sharing-policy/)           | WHITE |
| ATT&amp;CK Tactic  | [TA0001: Initial Access](https://attack.mitre.org/tactics/TA0001) |
| ATT&amp;CK Technique  | [T1566.001: Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001), [T1566.002: Spearphishing Link](https://attack.mitre.org/techniques/T1566/002) |
| Tags          | phishing |

 ## Purpose
 To provide a structured response to phishing incidents, ensuring swift action and proper documentation.
 
 ## Overview
 This playbook outlines the steps to take when a phishing incident is identified.
 
 ## Steps
 
 ### 1. Identification 
   - Confirm if the reported email is indeed a phishing attempt.
   - Verify the sender's address and any suspicious links.

 ### 2. Containment
   - Notify the affected user not to click on any links or provide personal information.
   - Block the sender’s email address in the email security gateway.

 ### 3. Analysis
   - Analyze the phishing email for indicators of compromise (IOCs).
   - Identify any potentially impacted accounts or systems.

 ### 4. Eradication
   - Remove the phishing email from all affected users' inboxes.
   - Reset passwords for any accounts that may have been compromised.

 ### 5. User Education
   - Conduct a training session for all employees on recognizing phishing attempts.
   - Share examples of the phishing email with the team.

 ### 6. Reporting
   - Document the incident in the incident management system.
   - Report the phishing attempt to relevant authorities (e.g., anti-phishing organizations).

 ### 7. Review
   - Conduct a post-incident review to evaluate the response process.
   - Update training materials and security awareness programs based on lessons learned.

