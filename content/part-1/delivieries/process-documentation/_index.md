+++
title = "Process Documentation"
weight = 1
chapter = false
+++

| Revised Date | Comment |
| ------------ | ------- |
| 20.10.2024   | Page added | 

**In cybersecurity operations, structured process documentation is vital for guiding incident responses and ensuring operational consistency. Threat hunters play a crucial role in creating and refining these documentation entities by leveraging their expertise in identifying emerging threats and attack vectors.**

**Examples of structured process documentation:**

- **Playbooks: Strategic guides shaped by threat hunters that outline detection, analysis, and containment strategies.**
- **Runbooks: Detailed, step-by-step instructions informed by threat hunters, ensuring effective incident triage and threat detection.**
- **SOPs: Standardized procedures that evolve with threat hunters’ insights to address current challenges in the threat landscape.**

**This chapter illustrates how threat hunters are essential contributors to these documentation entities, enhancing the SOC's ability to respond effectively to the dynamic nature of cyber threats. Structured process documentation is one of the most important contributions that threat hunters make, as it drives the SOC forward.**

## Documentation Entities

In the introduction to this chapter, I mentioned three kinds of documents; these will be the focus of this chapter. Below is a brief explanation of each.

### Playbooks
Playbooks are comprehensive, strategic guides designed to outline workflows and response strategies for various incidents. They focus on the broader response to incidents, covering stages such as detection, analysis, containment, eradication, and recovery. Playbooks often incorporate decision-making frameworks, define roles and responsibilities, and outline communication protocols, ensuring that the entire SOC (Security Operations Center) team operates cohesively during incident responses.

### Runbooks
Runbooks are operationally focused, providing step-by-step instructions for completing specific tasks within the SOC. They guide analysts through routine or predictable actions, such as incident triage, system reboots, or threat hunting. Unlike Playbooks, Runbooks focus on precise, repeatable tasks, often including commands or scripts to automate parts of the process. They ensure consistent execution of tasks by SOC personnel.

### Standard Operating Procedures (SOPs)
SOPs are formal documents that define standardized processes for various operations within the SOC. They serve as overarching guidelines that ensure consistency and compliance with organizational policies. SOPs cover routine activities like incident response, system monitoring, and report generation, promoting efficiency and accountability throughout the organization.

### Visualization

Playbooks, Runbooks, and SOPs are complementary tools that serve different purposes within the SOC. Playbooks offer high-level, strategic guidance for incident response; Runbooks provide detailed, task-specific instructions; and SOPs ensure that all procedures align with organizational standards.

Overall, the following graph visually demonstrates how these documentation types interrelate, showing that Playbooks, Runbooks, and SOPs fall under the umbrella of Operational Documentation, while providing distinct yet complementary functions to enhance SOC effectiveness.

{{<mermaid align="center">}}
graph TD
    A[Operational Documentation] 
    B[Playbooks]
    C[Runbooks]
    D[Standard Operating Procedures - SOPs]

    A --> B
    A --> C
    A --> D

    B --> E[Incident Response Strategies]
    B --> F[Decision-Making Frameworks]
    
    C --> G[Step-by-Step Instructions]
    C --> H[Task Automation]

    D --> I[Routine Operations]
    D --> J[Compliance Protocols]
{{< /mermaid >}}

The graph above introduces several entities that have not been previously discussed:

- **Playbooks**: These may include decision-making frameworks, which serve as tools to aid in making informed decisions. However, this topic is not the focus of this chapter.
  
- **Runbooks**: Runbooks can be fully automated through scripts, SOAR workflows, and other automation methods. This chapter will concentrate on the manual, day-to-day operations within a SOC.
  
- **Standard Operating Procedures (SOPs)**: SOPs are essential for ensuring compliance and standardization in processes. It is implicit that SOPs guarantee tasks are performed (mostly) consistently each time.

### Hierarchical Relationship Overview

The relationship between **Playbooks**, **Runbooks**, and **SOPs** can be described as follows:

| Entity | Description | 
| ------ | ----------- | 
| SOPs | Provide high-level, standardized guidance, outlining the required processes for maintaining consistency across teams and operations. |
| Playbooks | Are derived from SOPs but focus on responding to specific situations, offering a more flexible and adaptive approach to dynamic incidents, such as cybersecurity threats. |
| Runbooks | Implement the procedures from both SOPs and Playbooks in a detailed and structured manner, offering exact steps to perform operational tasks. |

The following graph showcases the relationship between multiple Playbooks and their associated SOPs, as well as their sequential order within the Runbook framework:

{{<mermaid align="center">}}
flowchart TB
   subgraph Runbook
     subgraph Playbook1[Playbook 1]
        SOP1[SOP 1]
        SOP2[SOP 2] 
     end

     subgraph Playbook2[Playbook 2]
        SOP3[SOP 3]
        SOP4[SOP 4] 
     end
  
     subgraph PlaybookN[Playbook N+1]
        SOP5[SOP N+1] 
     end
   end

   Playbook1-->Runbook
   Playbook2-->Runbook
   PlaybookN-->Runbook
{{< /mermaid >}}

| Entity | Description | 
| ------ | ----------- | 
| Runbook | The top-level container holding various Playbooks. A Runbook is a detailed execution guide for a variety of tasks, referencing specific Playbooks for specialized instructions. |
| Playbooks | Each Playbook (e.g., Playbook 1, Playbook 2, Playbook N+1) is associated with specific scenarios or incidents. Each Playbook includes a set of SOPs that guide the processes in a structured way. |
| SOPs | Each Playbook references one or more SOPs, providing the detailed steps necessary to carry out routine tasks relevant to the Playbook's focus. |
| Sequential Flow | The arrows connecting Playbooks (e.g., Playbook 1 → Playbook 2 → Playbook N+1) indicate that these Playbooks might be used in a particular sequence or build on one another as the incident progresses. |

This structure reflects how Playbooks and SOPs integrate within Runbooks, creating an organized and efficient approach to security operations.

## Examples

In the subsequent subchapters, I will present one example of each of these documents. Feel free to use these as inspiration for your own work. I have based the examples on various sources, mostly the ATC RE&CT Framework, as well as custom templates I have developed over the years.

## Resources

* [ATC React](https://atc-project.github.io/atc-react/)
* [The RE&CT Framework](https://github.com/atc-project/atc-react)
* [How To Create A Runbook For SOC: Practical Guide](https://www.neumetric.com/journal/how-to-create-a-runbook-for-soc-practical-guide/)
* [Runbook vs. Playbook, SOP, User Guide: How Do They Compare?](https://scribehow.com/library/runbook-vs-playbook)
* [What is a Runbook?](https://www.pagerduty.com/resources/learn/what-is-a-runbook/)
* [Security Operations Center (SOC): Data Guardians](https://medium.com/@aasthathakker/security-operations-center-soc-data-guardians-8f961264f824)
* [Runbooks vs Playbooks: A Guide to Understanding Operational Documentation](https://medium.com/@squadcast/runbooks-vs-playbooks-a-guide-to-understanding-operational-documentation-d111027b7761)