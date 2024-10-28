---
title: "Sitrep"
date: 2024-10-13T09:09:44+02:00
draft: false
weight: 5
---

| Revised Date | Comment |
| ------------ | ------- |
| 13.10.2024   | Page added | 

## Introduction

**In threat hunting, Security Operations Center (SOC) operations, and incident management, effective communication is essential. In this chapter we are going to look into a great tool called a SITREP - also known as The Situation Report. The SITREP serves as a vital tool to ensure that all stakeholders—team members, management, and external partners—are kept informed about ongoing situations. By providing structured, concise updates on the current status, actions taken, and next steps, SITREPs help teams make informed decisions and coordinate responses effectively.**

## What 

A SITREP (Situation Report) is a concise, structured update on the current status of an ongoing situation. Originally developed for military use, SITREPs are now employed across various fields, including cybersecurity. In threat hunting, Security Operations Centers (SOCs), and incident management, SITREPs play a critical role in keeping all stakeholders informed about important developments.

In any command center, three key activities are common: inputs, processes, and outputs. The inbound aspect involves communication, typically in the form of intelligence or field reports. This is where SITREPs (detailing the current situation) and PROGREPs (progress reports tracking movement toward a set goal) provide essential insights.

- SITREPs convey what is currently happening in the field, allowing for real-time awareness of ongoing situations.
- PROGREPs, on the other hand, focus on the progress made toward specific objectives or goals. They provide updates on milestones achieved, tasks completed, and any challenges encountered along the way. This helps teams assess whether they are on track or need to adjust their strategies.

I’d like to discuss PROGREPs here to clarify the distinctions between these two types of reports. Throughout my career in Security Operations Centers (SOCs), I’ve observed many analysts confuse SITREPs with PROGREPs. Often, instead of providing a clear SITREP, they focus too heavily on PROGREPS, which can lead to misunderstandings and incomplete communication. To keep it simple, most of the time management only wants to know what is going on, like _"We have currently situation XYZ on our hands"_. However, the next question from management is PROGREPs though, so keep in mind this thumb of rule:

> SITREP first, then follow up with PROGREPS.

Anyhow, together, these reports feed information from the field back to the command element, helping align actions, facilitate decision-making, and ensure a coordinated response across teams. 

## How

The best way to understand SITREPs is to consider them as essential tools in operational communication. Here are several key aspects that illustrate their importance:

| Aspect | Comment |
| ------ | ------- | 
| A Manuscript of Information | SITREPs serve as a guideline for what information needs to be communicated, ensuring clarity and focus. |
| Formal Texts for Action | They are formal documents designed to inform stakeholders about the current status of an operation, enabling informed decision-making for further actions. |
| Verbal and Textual Formats | While SITREPs can be delivered verbally, they should always be backed up by a written version. This dual approach enhances reliability and serves as a reference. |
| Hand-Off Documentation | SITREPs are meant to be handed over to other team members or stakeholders, ensuring continuity in communication and operations. |
| Timeline Documentation | They provide valuable documentation over time, creating a record of what has transpired during a particular incident or operation. |
| Timeliness is Key | Given the fast-paced nature of incidents, SITREPs should be quick to write. Time is of the essence in crisis situations, and concise reports facilitate rapid responses. |

By keeping these elements in mind, teams can utilize SITREPs effectively to maintain situational awareness and enhance collaboration during critical incidents. In the following examples I have included SITREPS I frequently use in my line of work. Please consider these as templates you can change as sees fit. After all, there exist no golden template and most SITREPS are unique for the situation they are used in.


### Examples SITREP

{{% notice info %}}
Please take inspiration of the following examples and modify to your needs!
{{% /notice %}}

#### Threat Hunting

This case is unique, as it also serves to define the focus of the current hunt. I, personally, make it a point to fill out the details before beginning, but I leave the "Next Steps" section blank. When someone requests the SITREP, I ensure that I update this section to reflect my next steps. This approach allows for clarity and keeps everyone informed about the direction of the investigation as it progresses. 

By documenting the initial details and updating the next steps upon request, I maintain a clear communication channel with my team and ensure that everyone is aligned on our objectives. This practice is crucial for effective collaboration in threat hunting. 

| **Field**             | **Details**                                                   |
|-----------------------|---------------------------------------------------------------|
| **Date**              | October 13, 2024                                              |
| **Threat Hunt Team**  | Team Corgi                                                    |
| **Target**            | MITRE T1566 (Phishing)                                        |
| **Status**            | Ongoing                                                       |
| **Key Findings**      | Unusual email activity, malicious URLs, spearphishing attempts |
| **Recommendations**   | Strengthen email filtering, user awareness training           |
| **Next Steps**        | Analyze attachments, develop YARA rules                       |

### SOC

In a Security Operations Center (SOC), it is essential to promptly inform the SOC manager (or the equivalent authority) whenever an alert registers a criticality above a predefined threshold. I have found this template to be invaluable for escalating cases effectively. It provides a clear structure for communicating the necessary information and ensuring that appropriate actions are taken swiftly.

Using this template not only enhances the efficiency of our communication but also ensures that critical incidents receive the attention they require. Effective escalation is vital in managing security threats, allowing for informed decision-making and timely interventions.

| **Field**             | **Details**                                                   |
|-----------------------|---------------------------------------------------------------|
| **Criticality**       | Pri 1.                                                        |
| **Date/Time**         | October 13, 2024, 10:00 AM                                    |
| **Analyst**           | Albus Corgi                                                   |
| **Incident**          | Suspicious login attempts (Brute Force)                       |
| **Key Observations**  | Multiple failed logins from IP 192.168.1.5                    |
| **Actions Taken**     | Blocked IP, initiated password reset                          |
| **Next Steps**        | Monitor logins, review affected accounts                      |
| **Status**            | Under Control                                                 |

#### Incident Management

This template provides a structured overview of an incident, detailing the current status, actions taken, and next steps for resolution. Maintaining clear and precise communication during such critical events is vital for ensuring effective incident management and minimizing potential risks to our stakeholders.

| **Field**             | **Details**                                                   |
|-----------------------|---------------------------------------------------------------|
| **Incident Name**     | Data Breach - Customer Records Exfiltration                   |
| **Date/Time**         | October 13, 2024, 08:30 AM                                    |
| **Incident Commander**| Albus Corgi                                                   |
| **Status**            | Ongoing Investigation                                         |
| **Affected Systems**  | CRM database, API endpoints                                   |
| **Details**           | Unauthorized access, exfiltration of customer PII confirmed   |
| **Actions Taken**     | Shut down affected systems, notified legal teams              |
| **Next Steps**        | Forensics, implement controls                                 |

#### Incident Call-in

This template is commonly used when a caller contacts the SOC hotline. If a caller is met by an unprepared receiver who struggles to ask the right questions, it reflects a significant failure on our part. Effective communication during these initial interactions is critical, as it sets the tone for the entire incident response process.

It is essential for the receiver to be well-prepared to engage with callers, as this facilitates the gathering of accurate information and a swift response. A smooth and professional interaction can significantly impact our ability to resolve issues efficiently.

Throughout my time in the SOC, I have developed several versions of this template and various methods for recording information. It is important that this template is easily accessible to the receiving personnel, whether it is printed on paper or available as a template on a wiki page. It must be within reach under any circumstance to ensure that we can respond effectively and without delay.

| **Field**             | **Details**                                                   |
|-----------------------|---------------------------------------------------------------|
| **Caller Name**       | [Name of person reporting]                                    |
| **Contact Details**   | [Contact details for caller, phone number, address etc]       |
| **Date/Time**         | [When call was received]                                      |
| **Reported Issue**    | [Brief description of the incident]                           |
| **Incident Location** | [Physical or network location]                                |
| **Affected Systems**  | [Critical systems affected]                                   |
| **Details**           | Description of unusual activity, time, actions taken          |
| **Immediate Actions** | Escalated to [team/role], Incident ticket opened: [number]    |
| **Next Steps**        | Investigate logs, notify stakeholders                         |

## Resources

- [Merriam-Webster: SITREP Definition](https://www.merriam-webster.com/dictionary/sitrep)  
- [Heimdal Security: Incident Response Plan Best Practices](https://heimdalsecurity.com/blog/incident-response-plan/)  
- [ProjectManagement.com: SITREP – A Necessary Tool in Project Management](https://www.projectmanagement.com/deliverables/5314/SITREP)  
- [Ready.gov: Situation Reports for Emergency Response](https://www.ready.gov/business/implementation/crisis)  
- [Wikipedia: Situation Report](https://en.wikipedia.org/wiki/Situation_report)  
- [CARE Emergency Toolkit - Situation Reports (SITREPS)](https://www.careemergencytoolkit.org/meal/42-information-management/4-situation-reports-sitreps/)
- [Persimmon Group - SITREP Template (PDF)](https://thepersimmongroup.com/wp-content/uploads/2022/05/SITREP-Template.pdf)
- [Wiktionary - Sitrep Definition](https://en.wiktionary.org/wiki/sitrep)
- [Persimmon Group - Situation Report (SITREP) Template](https://thepersimmongroup.com/situation-report-sitrep-template/)