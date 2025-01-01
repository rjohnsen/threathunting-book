---
title: "Understanding Data"
date: 2024-09-15T14:10:39+02:00
weight: 1
draft: false
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Improved formatting and wording | 

## Introduction

**A threat hunter makes a living from understanding data. All kind of data. But what does it mean? Understanding data means grasping the context, meaning, structure, and insights within a dataset. It involves knowing where the data comes from and what it represents, such as financial transactions, network logs, or customer feedback. Recognizing different data types, like numbers, text, or timestamps, and understanding how to work with them is crucial. It's also about identifying patterns, trends, or anomalies in the data and assessing its quality to ensure it's complete, accurate, and consistent. Additionally, understanding data involves making sense of the relationships between different variables and how they connect to real-world phenomena, like detecting a potential security breach from a spike in network traffic. Finally, it’s about extracting useful insights, making inferences, and applying the right analytical methods to interpret the data effectively. In our field, this would mean understanding how to interpret security logs, threat intelligence feeds, and other data sources to spot threats, trends, and anomalies in network behavior.**

---

## Strategies for understanding data

### Searching

Before be start on the analytical tecniques, we need to discuss the human aspect of understanding data. I will start this section by talking about searching. Searching is bread and butter for every threat hunter out there. It's simple - we search in order to find. However, jumping into action without a plan or direction is a waste of time. I have seen many eager people jumping into action without a proper plan. They usually end up spending much time searching with no results, often having to revert back and start over again using a strategy. And .. yes, often they encounter a log that they've have not fully grasped yet. Or said in a tongue in cheek way:

> You don't simply query into Mordor --- Roger Johnsen

In order to prepare for any unknown situation (going into Mordor), I think a threat hunter should take advantage of the OODA loop. The OODA loop was developed by military strategist John Boyd. It stands for Observe, Orient, Decide, and Act. It can be effectively applied to the process of understanding data, helping us systematically navigate the log source(s). 

#### Observe

When faced with something "unknown" that we need to understand, we should really start observing "it". In this phase, the focus is on gathering information from the unknown situation. Here are some tips: 

| Step | Description |
| ---- | ----------- |
| **Identifying the Log Source** | Determine the type of logs (e.g., firewall, application, system) and where they are coming from. Understanding the source helps establish the context for analysis. |
| **Data Collection** | Extract the logs from the source, ensuring you have sufficient data for analysis. This might involve pulling in large volumes of log data for a specific timeframe. |
| **Initial Examination** | Look for any immediate indicators, such as errors, warnings, or unusual entries, that might require further investigation. |

#### Orient

Once you have gathered the data, the next step is to analyze and interpret / making sense of the information:

| Step | Description |
| ---- | ----------- |
| **Contextual Analysis** | Understand the environment from which the logs are derived. Familiarize yourself with normal behavior patterns and expected log entries. |
| **Baseline Comparison** | If available, compare the observed logs against known baselines to identify anomalies or deviations from normal behavior. |
| **Categorization** | Classify the log entries based on their types and severity to determine which areas may need deeper investigation. |

#### Decide

After analyzing the data, you will need to make decisions on the next steps:

| Step | Description |
| ---- | ----------- |
| **Identifying Priorities** | Determine which anomalies or events warrant immediate attention based on their potential impact. |
| **Formulating Hypotheses** | Based on your observations and analysis, develop hypotheses about potential security incidents or issues that may be present. |
| **Strategizing Next Steps** | Decide whether to conduct deeper investigations on specific entries, gather more logs, or look into other sources of data for correlation. |

#### Act

It is time to finally act. This means take action! 

| Step | Description |
| ---- | ----------- |
| **Conduct In-Depth Analysis** | Delve deeper into specific log entries or related logs to confirm or refute your hypotheses.|
| **Documentation** | Document findings, actions taken, and any correlations made during the investigation for future reference and reporting. |
| **Response Actions**| Depending on the outcome of your investigation, implement necessary response actions. This may involve alerting relevant teams, mitigating threats, or implementing changes to improve security posture. |
| **Feedback Loop** | After taking action, reflect on the process and results. Consider what was learned from the investigation to improve future OODA cycles, particularly in dealing with unknown log sources. |

By applying the OODA loop to searching in an unknown log source or situation, we can maintain a structured approach that enhances their efficiency and effectiveness in identifying potential security incidents. Oh - you might have to restart the OODA loop to fully grasp something. That's OK! The OOOA loop is designed for just that!

### Clustering Data

We'll leave the human aspect for now. But before we do that, keep in mind to always carry the OODA loop with you. It sure is handy if you apply it right. Carrying on with some statistical approach to understanding data: 

> **Data clustering** in the context of SOC (Security Operations Center) and threat hunting refers to the process of manually grouping similar types of security events, logs, or incidents based on shared characteristics or patterns. The goal is to identify related activities, isolate abnormal behavior, and detect potential threats more efficiently.

#### Key Concepts in SOC Clustering

| Concept | Description |
| -- | -- | 
| **Event Grouping** |Analysts manually sort and categorize similar security events (e.g., login attempts, network traffic spikes) to identify trends or anomalies. |
| **Log Analysis** | Clustering helps to organize log entries from firewalls, IDS/IPS, or endpoint security tools, grouping them based on similar IP addresses, timestamps, or activity types. |
| **Pattern Recognition** | By clustering events that share common characteristics, analysts can detect potential attacks or lateral movement within the network. |
| **Reducing Noise** | Grouping redundant or benign events can reduce alert fatigue, allowing analysts to focus on clusters that stand out and require deeper investigation |

#### Examples of Manual Clustering

| Example | Description |
| -- | -- | 
| **IP or Domain Grouping** | Grouping traffic or alerts related to specific IP addresses, domains, or subnets to investigate potentially malicious communication. |
| **Time-Based Clustering** | Clustering incidents or events occurring within the same time window to look for coordinated attack patterns (e.g., a brute-force attempt followed by privilege escalation). | 
| **User Behavior Clustering** | Grouping activities by users, especially when investigating insider threats or compromised accounts, to determine if there's any unusual behavior. |
   
In SOC and threat hunting, clustering helps analysts efficiently manage large volumes of data and prioritize their investigations, making it easier to spot indicators of compromise or suspicious activity.
    
### Grouping Data

> **Grouping data** in the context of threat hunting refers to the process of taking multiple unique artifacts and identifying when multiple instances of them appear together based on specific criteria. This technique is essential for efficiently analyzing security data and identifying potential threats or anomalies.

#### Key Aspects of Grouping Data in Threat Hunting

| Aspect | Description |
| -- | -- | 
| **Definition and Purpose**: | Grouping involves categorizing unique artifacts (e.g., IP addresses, user accounts, event types) to see when multiple items of interest appear together. This can help analysts detect patterns indicative of malicious behavior. |
| **Explicit Input** | Unlike clustering, where the algorithm determines groupings based on similarities without predefined categories, grouping uses an explicit set of items that are already known to be of interest. Analysts define the items they want to track and analyze. |
| **Identifying Tools and TTPs** | If a particular group of artifacts appears out of place or unusual, it may represent a tool or TTP (Tactics, Techniques, and Procedures) that an attacker is using. This can be critical for identifying ongoing attacks or breaches. |
| **Criteria for Grouping** | An important aspect of grouping is determining the specific criteria for identifying related instances. This might include: _Time periods_ (grouping events that occurred within a certain timeframe). _Event types_ (Grouping related events, such as failed login attempts followed by a successful login). _Source or destination_ (Grouping based on common source or destination IP addresses.) |
| **Hunting for Related Instances** | Grouping works best when analysts are hunting for multiple, related instances of unique artifacts. For example, if multiple failed login attempts from the same user account occur within a short time frame, this may warrant further investigation. |
| **Anomaly Detection** | By observing groups of artifacts that deviate from the norm, analysts can more easily identify potential threats. For instance, if several user accounts are accessed from unusual locations at the same time, it may indicate a coordinated attack. |
| **Enhanced Reporting and Visualization** | Grouping data allows for better reporting and visualization. Security analysts can create dashboards that showcase grouped metrics, making it easier to communicate findings and respond effectively. |

An analyst might group data from a SIEM based on:
  - **Time Period**: Identifying all login attempts within a specific hour that come from unusual geographic locations.
  - **User Accounts**: Tracking all accounts that experience multiple failed login attempts in a short timeframe.
  - **Event Types**: Grouping alerts from different systems (e.g., firewall and intrusion detection system) that indicate a potential breach attempt.
  - **Other**: By other means than depicted here that fits the scenario.

Grouping data is a vital technique in threat hunting that enhances the ability to analyze security events, detect anomalies, and prioritize responses. By identifying when multiple unique artifacts appear together based on specific criteria, security analysts can uncover patterns that may indicate malicious activity. This method is particularly useful for tracking related instances and understanding potential threats within an organization’s environment.

### Stack Counting

> **Stack counting** in the context of threat hunting refers to the practice of systematically accumulating and organizing counts of various security-related events or indicators over time. This method allows analysts to identify patterns, anomalies, and potential threats within a network or system by visually stacking counts of different types of events or alerts. Here’s a breakdown of its significance and usage in threat hunting:

#### Key Aspects of Stack Counting in Threat Hunting

| Aspect | Description |
| -- | -- | 
| **Data Aggregation** | Stack counting involves aggregating data from various sources (e.g., logs, alerts, user activities) to create a comprehensive view of events occurring within an environment. |
| **Visualization** | By using visual representations (like bar charts or histograms), stack counting helps security analysts quickly identify trends, spikes, or unusual patterns in data. This visualization makes it easier to spot anomalies that could indicate malicious activity. |
| **Comparison of Event Types** | Analysts can compare the counts of different types of events (e.g., successful logins, failed login attempts, firewall alerts) to understand normal versus abnormal behavior. For instance, a sudden increase in failed logins could indicate a brute force attack. |
| **Temporal Analysis** | Stack counting can be used to track event counts over time, enabling analysts to see how certain types of events change, which may correlate with specific activities or incidents. |
| **Incident Response** | Identifying anomalies through stack counting allows for quicker incident response. For example, if there is an unexpected increase in traffic from a specific IP address, analysts can investigate further for potential malicious intent. || 
| **Prioritization** | By identifying the most frequently occurring threats or events, security teams can prioritize their investigations and resources towards the most critical issues. |
| **Baseline Establishment** | Establishing a baseline of normal activity using stack counting can help in detecting deviations that may indicate security incidents. |

An analyst might use stack counting to visualize the number of alerts generated by various detection rules in a security information and event management (SIEM) system over a week. By stacking the counts, the analyst can easily identify which alerts are most common, which might require deeper investigation, and which may represent a coordinated attack.

An another example I have found extremely handy is just to count occurrence of "things". For instance, count browser user-agents to find most used user-agents and from there find outliers, like so: 

{{<mermaid align="center">}}
xychart-beta
    title "User-Agents"
    x-axis [Chrome, Firefox, Edge, Opera, "Py Requests", "Py Urllib", Scrapy, "curl/wget"]
    y-axis "Occurrence" 0 --> 200
    bar [150, 90, 80, 60, 30, 20, 15, 10, 5]
{{< /mermaid >}}

Some may find counting using tables easier, like so:

| User-Agent Type      | Count |
|-----------------------|-------|
| Chrome                | 150   |
| Firefox               | 90    |
| Safari                | 80    |
| Edge                  | 60    |
| Opera                 | 30    |
| Python Requests       | 20    |
| Python urllib         | 15    |
| Scrapy                | 10    |
| curl/wget             | 5     |
| **Total Legitimate**  | **410** |
| **Total Malicious**   | **50**  |

In summary, stack counting is a valuable technique in threat hunting that enhances visibility into potential security incidents, aiding in the identification, prioritization, and response to threats. By the way, grouping is extremely good fit for dashboards in your SIEM/SOAR!

### Baselining

Baselining can be viewed as the noble art of finding and documenting what is considered legitimate behavior or usage within a network or system. This intricate process begins with the thorough observation and analysis of various activities across the digital landscape, enabling us to identify the patterns and behaviors that define normalcy for the organization. By collecting and analyzing data from logs, user activities, and network traffic, we can establish a clear understanding of typical operational behavior.

Once this baseline of legitimate behavior is documented, it serves as a crucial reference point for detecting anomalies. Any deviations from this established norm can raise red flags, indicating potential security threats such as unauthorized access, malicious activities, or system misconfigurations. However, baselining is not merely about creating a static snapshot; it requires continuous adaptation and refinement as the organizational environment evolves. Factors such as changes in user roles, the introduction of new technologies, or variations in operational demands can all influence what is deemed "normal."

Moreover, understanding the context surrounding user behavior is essential for effective baselining. For example, an unusual spike in network activity during a corporate event might be legitimate, while a similar spike during off-hours could warrant further investigation. By maintaining an up-to-date and contextualized baseline, threat hunters can enhance their ability to identify genuine threats while minimizing false positives, ultimately fostering a proactive security posture that protects the organization's critical assets.

HOWEVER, baselining is a kind of unicorn — it is rare, extremely rare, to find someone who has fully baselined and documented a system or network. Instead, what I have found are various attempts at dashboards trying to explain what’s going on. I have chosen to call such dashboards baselining. They don’t tell the entire truth but offer a glimpse of the truth, and that’s better than nothing.

### General tips and tricks

Over the years I have amassed some thoughts regarding interpreting logs and data. Here's a few thoughts from me on understading data: 

| Aspect | Description |
| -- | -- | 
| Volume | Always look for volume. The size of data can give an indication of a situation. Example: large data transfers could mean exfiltration. | 
| Count| Counting is basics. Simply knowing the count of things can point in a direction. Example: Many failed logins may suggest brute force. |
| Min | Hackers loves being stealthy. Take a look at what occurs seldom or the tiniest size of something. Example: Tiny network packets might signal suspicious activity. | 
| Max | Maximum values can indicate interesting behavior. Example: Many file access entries in log could mean unauthorized access. |
| Sum | The sum of it all. Example: high login totals after hours may hint at insider threats. |
| Percentage | Percentage can be used to compare between systems, or depict a baseling - or outliers. Example: A spike in failed logins can signal an attack. |
| Combine them all | All of these tips can be successfully combined to form a narrative - keep that in mind when you twist and turn data to understand it |

### Resources

* [Cyber Threat Hunting Techniques & Methodologies](https://heimdalsecurity.com/blog/threat-hunting-techniques/)
* [Threat Hunting Metrics: The Good, The Bad and The Ugly](https://kostas-ts.medium.com/threat-hunting-metrics-the-good-the-bad-and-the-ugly-d662907379b2)
* [Threat Hunting Techniques, Checklist, Examples, Execution, Metrics](https://proinf.com/threat-hunting-techniques-checklist-examples-process-exection-metrics)
* [Proactive Threat Hunting Tools & Techniques](https://www.stickmancyber.com/cybersecurity-blog/proactive-threat-hunting-tools-techniques)
* [Check the Stats, Your Threat Hunting is Probably Broken](https://www.activecountermeasures.com/check-the-stats-your-threat-hunting-is-probably-broken/)
* [Threat Hunting Demystified](https://medium.com/@linda.milvi/threat-hunting-demystified-d8d9cec5fd7b)
* [Jitter Plots: Solving Overlapping Data in Scatter Plots](https://www.editverse.com/jitter-plots-solving-the-overlapping-data-problem-in-scatter-plots/)
* [Jittering with R](https://www.bridgetext.com/jittering-with-r)
* [Using scatterplots to find details in reports](https://www.sqlbi.com/articles/using-scatterplots-to-find-details-in-reports/)
* [Outlier!!! But Why???](https://towardsdatascience.com/outlier-but-why-b26c30c9ab78)
* [What is cluster analysis?](https://www.spotfire.com/glossary/what-is-cluster-analysis#:~:text=Cluster%20analysis%20is%20a%20data,is%20an%20unsupervised%20learning%20method.)
* [Cluster analysis](https://en.wikipedia.org/wiki/Cluster_analysis)
* [How Grouping Analysis works](https://pro.arcgis.com/en/pro-app/latest/tool-reference/spatial-statistics/how-grouping-analysis-works.htm)
* [Grouping Data in Data Science](https://towardsdatascience.com/grouping-data-in-data-science-be7387870c4d)
* [Baseline Hunting with the PEAK Framework](https://www.splunk.com/en_us/blog/security/peak-baseline-hunting.html)
