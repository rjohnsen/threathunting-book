+++
archetype = "page"
title = "Release Plan"
date = 2024-12-27T15:20:20+01:00
weight = 7
chapter = false
+++

| Revised Date | Comment |
| ------------ | ------- |
| 20.04.2025   | Page updated. | 

## Tentative Release Plan 2025

{{% notice info %}}
The following are the tentative release plan for articles and notes from my library, as well as an overview of general site improvements. These points are in no particular order and the forecasted content may differ from the actual released content.
{{% /notice %}}

### Site Changes

#### Overhaul the lab environment section

- [ ] Transition the lab's configuration and documentation to align with Podman as the container management tool. This should include clear instructions for installation, a comparison to Docker, and examples of how Podman can enhance security and flexibility in a SOC or threat-hunting lab setup. In addition, add support for LogStash for easier ingestion of adhoc logs.

### Planned topics

#### Threat hunting and where it fits in a SOC and/or DFIR setting

- [x] When to engage in threat hunting compared to alerts, triage, incident response, and digital forensics and incident response (DFIR). Explore how threat hunting fits into the broader incident response lifecycle. Provide examples of scenarios where proactive threat hunting can identify potential risks before they trigger alerts. Discuss its complementarity with triage and DFIR, emphasizing how hunting uncovers patterns that automated alerts may miss.  
- [x] Introduction to what needs to be in place before you can call your SOC and Threat Hunting efforts a success. 

#### Hunting as a broad discipline

- [ ] **Hunting risks, not just threats** Analyze the concept of shifting focus from individual threats to broader risks. Discuss how this approach aligns with enterprise risk management. Include practical examples, such as identifying systemic vulnerabilities or supply chain risks that could lead to future threats.  
- [ ] **Can a framework be created for hunting that encompasses threat, risk, policy, audit, pentest, and others?** Propose a conceptual framework that integrates these elements. Detail how each aspect—threat, risk, policy, audit, pentest—interacts within a holistic security strategy. Discuss the limitations of traditional SOC roles and advocate for more dynamic, proactive practices that break silos and enable a continuous feedback loop.  

### Deliverables

- [ ] **What exactly can we as threat hunters deliver to customers and stakeholders?** Detail the tangible and intangible value that threat hunters bring. Include specific deliverables such as actionable insights, enhanced detection capabilities, improved security postures, and compliance with industry standards.
- [ ] **Reports** Discuss the importance of tailored reports that meet the needs of different stakeholders, from executives to technical teams. Provide examples of report structures for threat hunting findings and their impact on business decisions. Explore what kinds of reports are feasible, such as real-time dashboards, post-hunt summaries, and strategic recommendations.  
- [ ] **Sigma** Explore how Sigma rules can standardize threat detection across multiple SIEM platforms. Provide examples of creating, modifying, and deploying Sigma rules to address emerging threats effectively.  
- [ ] **Yara** Elaborate on using Yara rules for malware identification. Include best practices for rule creation, testing, and deploying them in active threat-hunting scenarios.  
- [ ] **Baselines** Detail the role of baselines in distinguishing normal behavior from anomalies. Provide strategies for creating baselines tailored to specific environments and tools to automate their maintenance.  
- [ ] **Audits** Discuss the value of audits in maintaining the integrity of threat-hunting processes. Explore how regular audits of tools, methodologies, and outcomes can uncover gaps and drive improvements.  
- [x] **Nothing. Because it happens**: Acknowledge that sometimes hunts yield no actionable findings. Frame this as a positive outcome, emphasizing that the absence of threats can confirm security measures are working as intended. Discuss how to document and communicate these outcomes.  

### Frameworks

- [ ] **Extend the Framework Section to Include Other Relevant Frameworks** Expand this section to cover frameworks like PEAK, Magma Framework, ATC React, Tahiti, Cyborg Framework, and Mitre CWE. Include a brief overview of each framework, highlighting its relevance to threat hunting. Discuss how these frameworks can be integrated into daily practices and how they complement each other in building a comprehensive hunting strategy.  
  - [x] PEAK
  - [x] Tahiti
  - [x] Magma Framework
  - [ ] ATC React
  - [ ] Cyborg Framewwork
  - [ ] Mitre CWE

### Planning

- [x] **Planning hunts, staffing, etc.**  Provide a structured approach to planning threat-hunting operations. Include steps such as setting objectives, defining hypotheses, allocating resources, and scheduling activities. Address how to estimate staffing needs based on the scale and complexity of hunts and how to balance workloads across a team.  
- [x] **Who can conduct hunts? What kind of individuals do we need?** Discuss the ideal skill set and mindset for threat hunters. Emphasize curiosity, analytical thinking, and technical expertise. Highlight how diverse backgrounds (e.g., system admins, developers, and data scientists) can enrich a hunting team. Outline formal qualifications, such as certifications (e.g., GCIH, GCFA, OSCP), alongside informal ones like hands-on experience with hunting tools or strong problem-solving skills. Discuss how to evaluate these qualifications in real-world scenarios.  

### Miscellaneous

- [ ] **Wabi Sabi** Discuss the concept of Wabi Sabi in the context of cybersecurity, focusing on finding beauty in imperfection. Explore how this philosophy could apply to threat hunting, such as valuing iterative improvement over perfection in tools and processes.  
- [ ] **Automation in threat hunts** Delve into the balance between manual and automated techniques in threat hunting. Use examples to argue for a hybrid approach, where automation handles routine tasks and manual analysis tackles complex patterns. Provide suggestions for integrating automation without losing the human element.  