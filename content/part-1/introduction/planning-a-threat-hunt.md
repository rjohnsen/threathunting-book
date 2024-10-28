---
title: "Planning a Threat Hunt"
date: 2024-10-27T11:10:42+01:00
draft: false
weight: 4
---

| Revised Date | Comment |
| ------------ | ------- |
| 27.10.2024   | Added page | 

## Introduction

**In this chapter we will jump into action by planning a threat hunt. Hopefully you got a threat hunting program already initiated- Planning a threat hunt involves a structured approach to proactively search for signs of malicious activity within an environment. I would love to stress that structured part. A threat hunting program based on "happy-go-lucky" isn't worth much. Anyway, below is a step-by-step guide to planning a threat hunt, combining tables with explanatory text and highlighting where AI can assist. Yes - I have chosen to involve AI in my threat hunts. You will be amazed how much value it can give you.**

## Steps

We will now explore the key steps involved in planning an effective threat hunt and we are going to take basis in the following illustrated flowchart. From defining the scope and objectives to continuously improving our hunting methodologies, each step plays a critical role in enhancing our overall security posture. Let’s delve into these steps to understand how we can implement a successful threat hunting strategy.

{{<mermaid align="center">}}
graph TD
    A[Define the Scope and Objectives]
    B[Gather Intelligence and Research]
    C[Data Collection]
    D[Develop the Hunt Plan]
    E[Execute the Hunt]
    F[Analyze and Validate]
    G[Report and Remediate]
    H[Continuous Improvement]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
{{< /mermaid >}}

### 1. Define the Scope and Objectives

> The very first step is to clearly define the purpose and scope of the threat hunt, we can do so by focusing on certain aspects.

| **Aspect**             | **Description** |
|------------------------| --------------- |
| **Purpose**            | Identify the main goal of the threat hunt (examples: detect adversaries, investigate an incident, test defenses).  |
| **Scope**              | Specify which assets or systems to focus on (examples: endpoints, network, cloud environments).                    |
| **Hunting Hypothesis** | Formulate a hypothesis based on threat intelligence or known attack patterns (examples: PowerShell-based malware). |

#### Key Considerations

- **Data Sources**: Identify which logs and telemetry will be used in the hunt.
- **Focus**: Decide whether the hunt will be broad (entire network) or narrow (specific systems).
- **Duration**: Determine how long the hunt will last.

#### AI Assistance

{{% notice tip %}}
Use AI to generate potential hypotheses based on existing threat intelligence and past incidents. Great for inspiration, but please vet the output from AI in all cases! 
{{% /notice %}}

### 2. Gather Intelligence and Research

> Gather relevant intelligence to guide the threat hunt. 

| **Aspect**              | **Description** |
| ----------------------- | --------------- |
| **Threat Intelligence**  | Use external sources (examples: MITRE ATT&CK, threat feeds) and internal intelligence (examples: SOC reports) to guide the hunt. |
| **Known Attack Patterns**| Study common attack techniques relevant to your environment. |
| **Relevant IOCs**        | Prepare a list of known Indicators of Compromise (IP addresses, domains, file hashes, etc.). But don't let the IOC take the focus of the threat hunt. You are looking for TTPs since IOCs are often short lived and might not be present in your systems as they appear in the reports. |

#### Key Considerations

- Review recent vulnerabilities and attack campaigns that may affect your environment.
- Focus on ongoing threats, such as phishing or malware attacks targeting the network.

#### AI Assistance

{{% notice tip %}}
Leverage AI to analyze large volumes of threat data and extract key insights, helping you to prioritize.
{{% /notice %}}

### 3. Data Collection

> Identify and collect the necessary data for your hunt.

| **Aspect**             | **Description** |
| ---------------------- | --------------- |
| **Identify Data Sources** | Choose relevant data for hunting: EDR, network traffic, SIEM logs, DNS, or firewall logs. |
| **Baseline Understanding**| Establish what normal activity looks like within the environment to help detect anomalies. |

#### Key Considerations

- Ensure access to all necessary data sources.
- Check that the collected data is sufficient and of high quality.

#### AI Assistance 

{{% notice tip %}}
Use AI to automate the data collection process if possible, ensuring comprehensive coverage and reducing manual effort. Many SIEMs and SOARs utilizes AI to establish baselines and for user behavior analytics. Utilize such inbuilt functionality if present.
{{% /notice %}}

### 4. Develop the Hunt Plan

> Based on the gathered intelligence, develop a detailed hunt plan.

| **Aspect** | **Description** |
| ---------- | --------------- |
| **Hunting Techniques**  | Decide whether to use anomaly detection, signature-based detection, or behavioral analysis for the hunt.    |
| **Tools**              | Select tools like SIEM, EDR platforms, or custom scripts to help execute the hunt. |

#### Hunting Techniques

| **Technique**             | **Description**                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------|
| **Anomaly Detection**      | Look for deviations from baseline behavior (examples: , unusual file or network activity).|
| **Signature-Based Detection**| Use predefined IOCs (examples: , IPs, hashes) to identify known threats. It is more important to look at the context how the IOC is used, rather than looking at the IOC itself. IOC from a threat intel report might not exist in your log, but the context on how it was used may exist. |
| **Behavioral Analysis**    | Search for behaviors indicative of known attacks (examples: , credential dumping, lateral movement).|

#### Key Considerations

- Decide if the hunt will focus on real-time data or historical data.
- Prepare any custom scripts, tools, or automation needed to enhance the hunt.

#### AI Assistance

{{% notice tip %}}
AI can help generate automated scripts for data queries or create detection rules based on patterns identified in the data. As always, AI is a helping tool - not a silver bullet. Please pay attention and review the output.
{{% /notice %}}

### 5. Execute the Hunt

> Begin the hunt by following the plan developed.

| **Aspect** | **Description** |
| ---------- | --------------- |
| **Search and Investigate**   | Execute searches based on your hypothesis. Look for anomalies or suspicious activity.|
| **Document Findings**        | Record suspicious activities and log details such as timestamps and affected systems.|

#### Key Considerations

- Identify any signs of unauthorized access or malicious activity.
- Ensure that findings are correlated across different data sources.

#### AI Assistance

{{% notice tip %}}
Utilize AI to analyze results in real-time, helping to identify patterns that may not be immediately visible to human analysts.
{{% /notice %}}

### 6. Analyze and Validate

> After collecting data, analyze and validate your findings.

| **Aspect** | **Description** |
| ---------- | --------------- |
| **Correlate Data**          | Combine findings from different data sources to confirm or rule out an actual threat. |
| **Filter False Positives**  | Ensure anomalies are not caused by benign activity (examples: , system updates, administrative tasks). |

#### Key Considerations

- Determine whether the observed activity can be traced to a legitimate cause or is part of an attack.
- Refine the hypothesis or pivot to another area based on findings.

#### AI Assistance

{{% notice tip %}}
Employ AI to automate correlation tasks and suggest possible explanations for detected anomalies.
{{% /notice %}}

### 7. Report and Remediate

> Summarize the outcomes of the threat hunt.

| **Aspect** | **Description** |
| ---------- | --------------- |
| **Document Results**      | Summarize findings, including detected incidents, anomalies, and overall results of the threat hunt.    |
| **Alert and Respond**     | Escalate real threats to the SOC or incident response team for remediation. Include actionable details. |

#### Key Considerations

- If real threats are found, outline the systems affected and type of threat.
- Conduct a post-hunt review to gather insights and improve future hunts.

#### AI Assistance

{{% notice tip %}}
Use AI to generate reports that highlight key findings and actionable insights in a concise manner.
{{% /notice %}}

### 8. Continuous Improvement

> Use the insights gained from the hunt for future enhancements.

| **Aspect** | **Description** |
| ---------- | --------------- |
| **Update Detection Rules**  | Use insights from the hunt to improve detection capabilities (examples: , SIEM rules, IOCs, playbooks).    |
| **Feedback Loop**           | Refine threat hunting methodologies, tools, and strategies based on findings and evolving threats.   |

#### Key Considerations

- Document any new attack methods discovered during the hunt.
- Identify gaps in the environment that need to be addressed.

#### AI Assistance

{{% notice tip %}}
AI can facilitate continuous learning by analyzing past hunts and suggesting adjustments to processes and tools based on results.
{{% /notice %}}

### Threat Hunt Example

To illustrate the threat hunting process in a short and concise manner:

| Term | Description |
| ---- | ----------- |
| **Hypothesis** | Attackers may have compromised internal systems using spear-phishing and are using PowerShell scripts for remote access. |
| **Data Sources** | PowerShell logs, EDR telemetry, and DNS logs. |
| **Hunt Method** | Look for PowerShell command invocations, outbound connections to suspicious domains, and abnormal DNS traffic. |
| **Outcome** | Identified multiple anomalous PowerShell executions tied to unauthorized remote access attempts. Incident escalated to incident response team.  |

## Resources

1. [Threat Hunting: The Basics](https://www.cisecurity.org/white-papers/threat-hunting-the-basics/)
2. [MITRE ATT&CK Framework](https://attack.mitre.org/)
3. [MITRE ATT&CK Overview](https://attack.mitre.org/overview/)
4. [CISecurity Threat Intelligence](https://www.cisecurity.org/controls/threat-intelligence/)
5. [SANS Data Collection Best Practices](https://www.sans.org/white-papers/38366/)
6. [Varonis Security Data Sources](https://www.varonis.com/blog/security-data-sources)
7. [SANS Hunting Techniques](https://www.sans.org/white-papers/38475/)
8. [Behavioral Detection and Threat Hunting](https://www.ibm.com/security/data-breach/threat-hunting)
9. [Executing Threat Hunts](https://www.csoonline.com/article/3572794/how-to-execute-a-successful-threat-hunt.html)
10. [Threat Hunting Methodologies](https://www.redcanary.com/threat-hunting-methodology/)
11. [Analysis Techniques for Threat Hunting](https://www.sans.org/white-papers/38721/)
12. [Validating Threat Hunting Findings](https://www.sans.org/white-papers/39208/)
13. [Forensic Reporting](https://www.forensicfocus.com/articles/how-to-prepare-a-forensic-report/)
14. [NIST Incident Response Guide](https://www.nist.gov/publications/guide-cybersecurity-incident)