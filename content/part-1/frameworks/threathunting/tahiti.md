---
title: "TaHiTI"
date: 2025-04-13T09:39:04+02:00
draft: false
hidden: false
weight: 8
tags:
    - framework
summary: ""
---

|Revised Date | Author | Comment |
| ----------- | ------ | ------- |
| 20.04.2025  | Roger Johnsen | Article rewritten |

**In modern cybersecurity, proactive threat hunting is essential. As attackers bypass automated defenses, structured threat hunting frameworks have emerged. One such framework is TaHiTI (Targeted Hunting integrating Threat Intelligence), developed by the Dutch financial sector (FI-ISAC). TaHiTI offers a structured, intelligence-driven approach, transforming ad hoc investigations into repeatable hunts. It refines hypotheses, identifies detection gaps, and integrates findings into security measures, emphasizing the use of threat intelligence for targeted and efficient hunts.**

---

## Why TaHiTI?

The core tenet of the TaHiTI framework lies in its emphasis on the *integration of threat hunting with threat intelligence*. This synergy ensures a focused and risk-driven approach to identifying potential threats within an organization's environment. Threat intelligence serves as a vital input, providing crucial context and enrichment throughout the hunting process, thereby guiding the direction and scope of investigations.

TaHiTI was born out of a collaborative effort within the Dutch financial institutes information sharing community (FI-ISAC). This collective endeavor aimed to address the need for a common language and a standardized methodology for threat hunting across the sector. By fostering a shared understanding, TaHiTI facilitates better communication, knowledge sharing, and ultimately, more effective threat detection capabilities within and among participating organizations.

Furthermore, the development of the "MaGMa for threat hunting" tool underscores the practical application of the TaHiTI methodology. This tool plays a significant role in streamlining the threat hunting process by providing a structured platform for documenting findings, organizing the outcomes of hunting investigations, and iteratively improving the overall threat hunting program.

Unlike frameworks that might solely focus on technical execution, TaHiTI implicitly acknowledges the importance of shared understanding and collaborative effort in building a robust threat hunting capability. By emphasizing the integration of intelligence and providing tooling for structured documentation, TaHiTI aims to elevate threat hunting from ad-hoc activities to a more mature and impactful security function.

## Core Principles of the TaHiTI Framework

The TaHiTI Threat Hunting Framework is built upon the fundamental principle of integrating threat intelligence directly into the threat hunting process. This integration manifests in several key aspects:

| Principle | Description |
| --------- | ----------- |
| Intelligence-Driven Focus | Threat intelligence acts as the primary driver for initiating and guiding hunting activities. By leveraging insights into known threat actors, their tactics, techniques, and procedures (TTPs), and emerging threats, hunters can focus their efforts on the most relevant areas of the environment. |
| Contextual Enrichment | Throughout the hunting process, threat intelligence is continuously used to enrich findings, providing deeper context and understanding of observed activities. This helps in differentiating malicious behavior from benign anomalies. |
| Risk-Based Prioritization | By aligning hunting efforts with credible threat intelligence, organizations can prioritize their resources and focus on investigating threats that pose the highest risk to their specific environment and assets. |
| Collaborative Foundation | The framework's origin within a collaborative community highlights the importance of information sharing and collective learning in enhancing threat detection capabilities. |

## Supporting Tool: MaGMa for Threat Hunting

To facilitate the practical implementation of the TaHiTI methodology, the "MaGMa for threat hunting" tool was developed. This tool serves several key purposes:

| Purpose | Description |
| ------- | ----------- |
| Structured Documentation | MaGMa provides a structured approach to documenting all aspects of a threat hunting investigation, from the initial hypothesis to the final conclusions and recommendations. This ensures consistency and facilitates knowledge retention. |
| Outcome Organization | The tool helps in organizing the findings and outcomes of hunting activities in a clear and coherent manner. This allows for easy analysis, reporting, and sharing of valuable insights. |
| Process Guidance | MaGMa acts as a guide throughout the threat hunting lifecycle, helping teams follow the TaHiTI methodology and ensuring that all critical steps are considered. |
| Iterative Improvement | By providing a platform for documenting and analyzing past hunts, MaGMa supports the continuous improvement of the threat hunting process over time. Lessons learned from previous investigations can be incorporated into future hunts, making the overall program more effective. |

## TaHiTI in Practice: A Conceptual Example

**Scenario**

> Recent threat intelligence indicates a rise in ransomware attacks targeting organizations within the financial sector, with a specific TTP involving lateral movement via compromised service accounts.

**Applying TaHiTI:**

1.  **Threat Intelligence Integration:** The threat intelligence about ransomware targeting the financial sector and the specific TTP of lateral movement via compromised service accounts becomes the driving force for a threat hunt.
2.  **Focused Hypothesis:** Based on the intelligence, a hypothesis is formulated: "Are there any signs of unusual lateral movement originating from service accounts within our network?"
3.  **Targeted Hunting:** Hunters utilize network traffic logs, authentication logs, and endpoint detection and response (EDR) data to investigate activity associated with service accounts, specifically looking for indicators of lateral movement techniques mentioned in the threat intelligence.
4.  **Contextual Analysis (using MaGMa):** Any suspicious activity identified is documented within MaGMa. Threat intelligence is used to enrich these findings, providing context about known attack patterns and indicators associated with the identified TTP.
5.  **Outcome and Action (documented in MaGMa):** If evidence of suspicious lateral movement is found, the findings are documented in MaGMa, along with recommendations for containment, remediation, and the development of new detection rules to prevent similar attacks in the future. This information can also be shared (anonymized as appropriate) with other members of the FI-ISAC to enhance collective defense.

## Conclusion

The TaHiTI Threat Hunting Framework offers a valuable and collaborative approach to proactive threat detection, particularly demonstrated by its adoption within the Dutch financial sector. By placing threat intelligence at the forefront of the hunting process and providing practical tooling like MaGMa, TaHiTI empowers organizations to move beyond reactive security measures and actively seek out potential threats within their environments. The framework's emphasis on shared understanding and structured documentation fosters a more mature and effective threat hunting capability, ultimately contributing to a stronger and more resilient security posture.

---

## References

| Resource | Description |
|---|---|
| [DEF-TaHiTI-Threat-Hunting-Methodology.pdf](https://www.betaalvereniging.nl/wp-content/uploads/DEF-TaHiTI-Threat-Hunting-Methodology.pdf) | The official document outlining the TaHiTI Threat Hunting Methodology. |
| [FI-ISAC (Dutch financial institutes information sharing community)](https://www.betaalvereniging.nl/en/payments-in-the-netherlands/security/fi-isac/) | Information about the Dutch financial sector's information sharing community. |
| [MaGMa for threat hunting](https://www.betaalvereniging.nl/wp-content/uploads/FI-ISAC-use-case-framework-verkorte-versie.pdf) | A tool developed to support the documentation and structuring of threat hunting activities within the TaHiTI framework (specific public documentation may vary). |