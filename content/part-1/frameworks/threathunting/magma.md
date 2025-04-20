---
title: "Magma"
date: 2025-04-13T10:45:06+02:00
draft: false
hidden: false
pweight: 9
tags:
    - framework
summary: ""
---

| Revised | Author         | Comment        |
| ------- | -------------- | -------------- |
| 20.04.2025 | Roger Johnsen  | Article rewritten  |

## Introduction

**In proactive cybersecurity, effective threat hunting demands more than just technical acumen; it requires a structured approach to documenting, organizing, and learning from each investigation. MaGMa (Management, Growth, Metrics & assessment), developed to support the TaHiTI (Targeted Hunting integrating Threat Intelligence) framework, provides this essential structure, transforming individual hunts into a collective body of knowledge that drives continuous improvement and proactive threat mitigation.**

---

## Why MaGMa?

MaGMa addresses critical challenges in operationalizing threat hunting programs, such as inconsistent documentation, difficulty in tracking progress and outcomes, and the absence of a centralized repository for lessons learned. By providing a structured framework for documenting every stage of a threat hunt, MaGMa ensures that valuable insights are captured consistently and readily accessible for analysis and future hunting endeavors.

MaGMa serves as a framework enabler, ensuring the organization of hunting outcomes. It enables security teams to identify recurring patterns, refine detection strategies, and measure the effectiveness of their hunting efforts, transforming isolated activities into a cohesive knowledge base. The TaHiTI whitepaper emphasizes the need for well-defined threat hunting processes, and MaGMa operationalizes this by providing a clear, repeatable structure.

Furthermore, MaGMa functions as a guide throughout the threat hunting lifecycle, aligning seamlessly with methodologies like TaHiTI. It prompts hunters to consider key aspects of each hunt, ensuring critical information capture and adherence to a defined, repeatable pattern. This guidance is valuable for onboarding new hunters and ensuring team consistency.

As highlighted in the TaHiTI whitepaper, the iterative nature of threat hunting requires a tool that can manage the complexity of investigations. MaGMa simplifies this process by providing a centralized location for planning, execution, and analysis.

## Core Functionalities of MaGMa

The core functionalities of MaGMa for threat hunting include:

| Functionality | Description |
| ------------- | ----------- |
| Structured Hunt Documentation: | Predefined templates and fields for recording crucial information about each hunt, such as the objective, scope, hypotheses, data sources, queries used, findings, and conclusions. This structured documentation aligns with the TaHiTI principle of maintaining clear records for future reference and analysis. |
| Hypothesis Management | Allowing hunters to articulate and track hypotheses being tested, along with their rationale. As emphasized in the TaHiTI framework, hypothesis-driven hunting is essential for focusing efforts and validating assumptions. |
| Evidence and Artifact Tracking | Enabling the systematic logging and linking of relevant data, logs, and artifacts discovered during the investigation. |
| Collaboration and Knowledge Sharing | Facilitating team collaboration by providing a centralized platform for accessing and contributing to hunt records. The TaHiTI framework highlights the importance of collaboration and knowledge sharing within the threat hunting team. |
| Outcome and Recommendation Management | Providing a dedicated space to document hunt outcomes, including identified threats, false positives, and recommendations for improving security controls or detection logic. |
| Reporting and Analysis | Offering capabilities to generate reports on hunting activities, track key metrics (e.g., number of hunts, findings per hunt), and analyze trends in hunting outcomes. |
| Integration with Threat Intelligence | Integrating with threat intelligence platforms to enrich hunt records with relevant contextual information, enabling hunters to prioritize and contextualize their investigations. |

## MaGMa in Practice: A Practical Example

**Scenario**

> Your threat intelligence team has identified a new phishing campaign targeting employees, potentially leading to credential harvesting. You decide to conduct a threat hunt to proactively identify any successful compromises.

**Leveraging MaGMa:**

1.  **Initiating the Hunt (in MaGMa):** Create a new hunt record in MaGMa, defining the objective ("Identify potential credential harvesting resulting from recent phishing campaign") and the scope (email logs, endpoint authentication logs, web proxy logs).
2.  **Formulating Hypotheses (in MaGMa):** Document specific hypotheses, such as "Look for unusual login attempts from non-standard locations following receipt of the phishing emails" and "Identify any successful execution of suspicious attachments from the phishing emails."
3.  **Executing the Hunt & Documenting (in MaGMa):** As you execute queries and analyze data, record findings directly within MaGMa, including queries used, anomalies identified, and evidence collected. This step aligns with TaHiTI's emphasis on documenting all actions taken during the hunt.
4.  **Collaboration (in MaGMa):** If collaboration is needed, MaGMa provides a centralized platform for sharing information and discussing potential leads.
5.  **Documenting Outcomes and Recommendations (in MaGMa):** Meticulously document details of any confirmed compromises or suspicious activity, along with recommendations for remediation, such as password resets or blocking malicious domains.
6.  **Knowledge Retention (in MaGMa):** The complete record of the hunt, including intelligence, hypotheses, queries, findings, and recommendations, is stored in MaGMa, serving as a valuable resource for future hunts and team learning.

## Hunt Program Maturity with MaGMa

Adopting a tool like MaGMa represents a significant step toward building a mature and effective threat hunting program. As organizations mature, they progress from ad-hoc hunting activities to a systematic, proactive approach that integrates threat intelligence and leverages structured methodologies. MaGMa facilitates this transition by providing the necessary framework and tools for managing and documenting the entire threat hunting lifecycle.

## References

Got it! Here’s the table you requested in the format you’ve provided, with example links:

| Resource | Description |
|---|---|
| [MaGMa: a framework and tool for use case management](https://www.betaalvereniging.nl/wp-content/uploads/FI-ISAC-use-case-framework-verkorte-versie.pdf) | Whitepaper |
| [MaGMa](https://www.betaalvereniging.nl/en/safety/magma/) | Introduction |
| [ATT&CK Use Cases with MaGMa!](https://medium.com/adarma-tech-blog/att-ck-use-cases-with-magma-3a5c83775d86) | ATT&CK Use Cases with MaGMa! |
| [SOC-CMM MaGMa UCF - Tools for effective Cyber Defense](https://www.first.org/resources/papers/amsterdam2019/2019.04.03-SOC-CMM-MaGMa-FIRST.pdf) | Presentation   |

