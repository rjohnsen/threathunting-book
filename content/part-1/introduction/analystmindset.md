---
title: "Analyst Mindset"
date: 2024-07-14T12:47:36+02:00
draft: false
weight: 2
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Improved formatting and wording | 

## Introduction

**Threat hunting can be considered an enhanced form of SOC analytics. While a SOC analyst typically responds reactively to alerts triggered by specific incidents, a threat hunter adopts a proactive approach. Instead of waiting for alerts, a threat hunter meticulously examines log data to identify anomalies and potential blind spots. This process involves formulating and testing hypotheses by correlating diverse sets of data, including both the raw logs and the statistics derived from them. Threat hunting is not just about following trails in logs; it is about understanding the underlying patterns within them.**

## Reflections

Becoming a proficient threat hunter requires a high degree of critical thinking. In this chapter, we will focus on the human perspective—specifically, the analyst—and highlight various methods and tools for approaching the role of a threat hunter.

Drawing from my experience as a mentor for new employees and in building SOC departments, I have guided many students and encountered numerous insightful questions about becoming and being an analyst. Reflecting on the questions I had when I first started as an analyst, I have formulated this definition:

> "Being an analyst is about discovering situations in a structured, repeatable manner that provides plausible answers."

By "repeatable," I mean using consistent methodologies to examine any findings along the way.

Today, we will explore thought patterns, models, and other valuable insights. Let's begin with a quote from one of Sun Microsystems' founders:

> "Many times, experts fail because they're experts in the past version of the world."

This quote underscores the necessity of staying at the forefront, continually adapting as the world rapidly changes. It perfectly reflects our everyday reality as analysts—what was true yesterday might not be true today. Each day presents a new set of challenges and opportunities. 

A threat hunter is essentially a SOC analyst on steroids, embodying this proactive and adaptive mindset. So, where do we begin our journey? Let's start by exploring the foundations of analytical thinking.

## Primer on analytical thinking

![The brain](/images/brain.png)

Thinking is an activity we engage in every day, all year round. But have you ever considered the different ways we think? These ways of thinking are known as thought patterns. Psychology has explored this extensively and identified numerous thought patterns. Here are some examples:

| Thinking Mode        | Description |
| -------------------- | ----------- |
| Sequential Thinking  | Organizing thoughts in a logical order. |
| Concretizing Thinking| Interpreting what we see, hear, feel, and experience. |
| Abstract Thinking    | Thinking beyond the concrete, considering broader concepts. |
| Holistic Thinking    | Considering the totality of a situation. |
| Lateral Thinking     | Thinking outside the box, making unconventional associations. |
| Critical Thinking    | Analyzing and evaluating information to form a well-founded opinion, often as a basis for action. |

There are many other ways of thinking, and we are not confined to one pattern at a time. Thought patterns often flow into each other because the brain is a complex machine, and the situations we encounter require a combination of thought patterns.

Understanding which thought patterns exist and when to use them can help you become more aware of your thinking process. This self-awareness allows you to correct your thinking and adjust your course as needed, which is an essential skill to have in your analytical toolbox.

Why am I emphasizing this at the beginning of our discussion? Because being an analyst involves a lot of thinking. When we engage in extensive thinking, we must be mindful of how we think to avoid contaminating our thought processes with biases and external influences. Here are some common factors that can affect our thinking:

| Factor                | Description |
| --------------------- | ----------- |
| Prejudice             | Allowing personal opinions to influence our thought process. |
| External Noise        | Being influenced by those around us and external information. |
| Insecurity            | Uncertainty about concepts can lead to vague analyses. |
| Confirmation Bias     | Being so convinced of a particular outcome that we overlook other possibilities. |
| Personal State        | Factors such as poor sleep or illness can affect our thinking. |

Many elements can impact our thinking, including the weather and family relationships. As analysts, being aware of how we think is crucial because our thinking is a primary tool. By understanding and mastering our thought processes, we can use thinking itself as an effective tool.

Most of you are likely familiar with analytical thinking, a thought pattern that defines us and serves as a vital tool. Let's define it:

> "Analytical thinking is the process of observing, researching, and developing critical insights from data or other information. It involves using these insights to gain knowledge, solve problems, or generate new ideas."

Analytical thinking involves several key steps:

1. Identifying a topic, problem, or issue.
2. Breaking down the topic, issue, or problem into manageable segments.
3. Obtaining relevant information from reliable sources.
4. Assessing cause, effect, and connections.
5. Developing solutions or enhancing understanding of the subject.
6. Testing solutions or new ideas based on what we have learned.
7. Reviewing which solutions worked and assessing our new knowledge.

Each of these steps can be broken down into specific methods. As we progress in our work, you will see how our internal procedures and routines align with these steps.

Understanding these thought patterns and processes is essential to grasping how and why the SOC operates the way it does.

## Thinking is verbing

Let's delve deeper into the steps of analytical thinking and identify a common thread: action verbs. Thinking itself is an action, something we actively do. 

By examining the steps outlined previously, we can extract essential action verbs that are crucial for our analytical work:

* Identify
* Break down
* Gather
* Assess
* Develop solutions
* Promote understanding
* Test
* Review
* Communicate

These verbs represent the core actions that form a comprehensive methodology—a structured approach to analysis.

Additionally, it's important to consider another critical aspect: dissemination. We must effectively communicate the results of our analytical processes to others. This ability to convey our findings clearly and accurately is essential for successful analysis.

## Think this, this, and this way

Analytical thinking is crucial, but it doesn't exist in isolation—it must be complemented by other thought models. At SOC, we integrate various thought patterns into our work. While we rely on analytical thinking, it is essential to combine it with critical thinking.

We need to analyze and assess information about a case to form a well-founded and accurate opinion, guiding our actions effectively. Running an analysis from start to finish without critically evaluating the process can lead to overlooking important details. This approach lacks quality, as it ignores potential blind spots.

Two key skills are central here: argumentation and strategic source assessment.

| Skill | Definition |
| ---- | ---------- |
| Argumentation | The ability to take a position on a case based on rationally valid reasons supported by evidence. This involves assessing the validity of claims and considering possible counter-arguments. |
| Strategic Source Assessment | The ability to use information from various channels to interpret their content. For example, when reading a text online, consider the type of publication, the publisher, and the publication date. Assess whether the sources have credibility and authority to be trusted. |

Often, we encounter situations with data gaps that must be filled with plausible theories. This requires creativity and finding connections that aren't always obvious—what we call "lateral thinking."

Lateral thinking involves solving problems using an indirect and creative approach, through reasoning that is not immediately obvious. Sometimes, we must hypothesize and test whether the evidence fits this hypothesis rather than allowing the evidence alone to shape our hypothesis. This approach is about thinking outside the box.

JP Guilford, known for his task of drawing four lines through a matrix of nine dots without lifting the pen, researched this extensively. However, Edward de Bono's work on different thought patterns and their combination to solve tasks is particularly notable. In 1967, de Bono introduced the concept of "lateral thinking," which I interpret as follows:

> "Creative thinking is the ability to come up with new and innovative ideas. Imagination plays a big role in this process. One of its greatest qualities is that it knows no boundaries. If we let it. Imagination is the search engine of your brain. You fill your brain with information from various sources. You then use your imagination to draw connections between the information. If you free the boundaries and let your imagination run wild, it's amazing what ideas emerge."

While using your imagination is essential, we must balance it with critical thinking to ensure it doesn't lead us astray. Think of it as "freedom under responsibility." Creative ideas must be plausible and defensible.

To illustrate these thought patterns, I have prepared a task based on classic lateral thinking exercises. This task demonstrates that we don't always have concrete information to rely on, similar to analyzing a log file. Be prepared to combine analytical, critical, and creative thinking to solve it.

## Analyze this task

Here I want you to use the thought models we have talked about to solve this task:

> It's almost midnight. It's quite a storm outside. There is a storm. A man sits inside. He is watching TV. The TV signals come and go. The man gets bored. He screws off the light and goes to bed. The man wakes up the following morning and reads in the newspaper: 40 men have died last night right outside his door.

What has happened and what is the man's occupation?

### Answer 

Lighthouse keeper. He has turned off the light in the lighthouse, which has caused a ship to go down.

### Important clues

How on earth did we come to this conlusion? Let me try to explain by important hints from the text:

| Clue | Comment |
| ---- | ------- |
| Midnight | Night – you use the light at night to see |
| Storm | Storm belongs to the sea |
| A man is sitting inside | Is he inside because it's night or stormy? |
| He is watching TV | Not really that interesting |
| The TV signals come and go | Alludes to the weather being reasonably bad |
| Turn off the light | Why is the light mentioned at all? |
| He is going to bed | Action that can tell us something about this being an action with cover |
| Reading in the newspaper | The newspaper brings news |
| Died | What happened |
| Outside the door | Why are so many dying outside his door? |
| Man | Male-dominated profession |

With this task, we leave the brain a little.

## Words of wisdom

We've discussed the mental model and recognized that managing and understanding thought patterns is a valuable tool. While we've focused significantly on our own thinking processes, these thought patterns can also offer insights into how an attacker thinks. Understanding and applying these patterns can significantly enhance our analytical capabilities.

To delve deeper into this concept, I've gathered some quotes that can further guide us in our daily work as analysts:

> "Every contact leaves a trace."

Dr. Edmond Locard (1877–1966), a French criminologist and pioneer of forensic science, became known as the "Sherlock Holmes of France." He formulated the basic principle of forensic science, known as Locard's exchange principle. This principle is fundamental to our work as threat hunters—we focus on finding those traces of contact.

If every action leaves traces, is there always a connection to be found? Sir Arthur Conan Doyle thought so in one of his books:

> "So all life is a great chain, the nature of which is known whenever we are shown a link of it."

Sir Arthur Conan Doyle, a British author and physician, created the character Sherlock Holmes in 1887. His stories are milestones in crime fiction and have significantly influenced modern police investigations. Doyle’s quote builds on Locard's principle, emphasizing the interconnectedness of all actions. If traces and connections exist, what can they reveal to us?

> "Res ipsa loquitur."

This Latin phrase means "The matter speaks for itself." It's an important principle reminding us to let evidence tell its own story without imposing our biases. This can be challenging, especially when we receive colored narratives from customers or other sources. While the interpretation of this principle can vary, its core idea is to let the facts speak for themselves.

These quotes illustrate a connected philosophy of analysis. However, one critical element remains: data. Our task involves sifting through vast amounts of data to find clues. We must be prepared to dissect, scrutinize, and interpret data to uncover the information we seek.

## The departure to technical information

In the data we process, we uncover numerous fascinating insights and narratives. Three key concepts you should be familiar with are IOA, IOB, and IOC. Understanding thought patterns will be particularly useful when hunting for these indicators.

### IOA: Indicator of Attack

- **Definition:** IOA refers to signs that something is about to happen (precursor) or is happening currently.
- **Description:** These are circumstances that hint at an attack but cannot be fully specified. Think of it as an indication that something suspicious is occurring.
- **Detection:** IOA-based detection focuses on an attacker's behavior, regardless of whether the attack is known or unknown. This method aims to uncover unknown or evolving exploits and attacks before they penetrate your defenses.

IOAs are events that can reveal an active attack before Indicators of Compromise (IOC) become visible. For example:
- A sudden increase in traffic to a website
- Unexpectedly high activity on a file server at odd hours
- Web servers communicating with internal hosts
- Internal hosts communicating with each other for the first time
- A laptop exhibiting unusual behavior
- Port scanning within the network
- Increased PowerShell activity on hosts
- Spike in traffic to online storage services like Dropbox or Box, which the company may not typically use

IOA detection requires near real-time monitoring and can be effectively combined with machine learning and IOB. Vigilance in monitoring logs and statistical dashboards is crucial.

### IOB: Indicator of Behavior

- **Definition:** IOB refers to the behavioral patterns of users, devices, or accounts and deviations from these established patterns.
- **Description:** This involves understanding what constitutes normal behavior and identifying deviations from this baseline. The concept of "baselining" is crucial, as it involves measuring typical behavior to detect anomalies.

Consider the following example:

Imagine a financial analyst named Jennifer who regularly accesses financial data from the company’s internal systems. Her typical behavior involves:
- Logging in during business hours (9 AM to 5 PM).
- Accessing specific financial reports.
- Using her work computer from her office network.

Now, let’s say Jennifer suddenly starts accessing financial data from a different location late at night and downloads unusually large amounts of sensitive information. These deviations from her usual pattern are notable and warrant further investigation.

**In more technical terms:**
- **Permanent Patterns:** Regular access to specific files or systems during standard working hours.
- **Sporadic Patterns:** Occasional access to systems outside normal hours for urgent tasks.
- **One-Time Events:** A significant data download or access to unusual files which is not part of her typical activity.

By establishing a baseline of Jennifers usual behavior, we can more easily spot deviations. For instance:
- **Regular Behavior:** Accessing financial data from her office computer during business hours.
- **Deviation:** Accessing data from an unfamiliar IP address late at night and downloading a large volume of files.

This deviation could indicate potential security concerns such as unauthorized access or data exfiltration. By monitoring such deviations, we can identify and investigate anomalies that might signal a security threat.

Mapping IOBs and understanding behavioral deviations allow us to effectively assess whether an anomaly is benign or indicative of a serious threat, ultimately enhancing our security posture.

### IOC: Indicator of Compromise

- **Definition:** IOC refers to signs that a compromise has occurred.
- **Description:** These are traces left by criminals at the crime scene, which we analyze to investigate the incident.

In our field, IOCs can include:
- Headers
- IP addresses
- Techniques
- Network artifacts
- Tools
- Domain names
- Registry keys
- Any mysterious behavior or deviation from the norm

Working with IOCs is a significant part of our daily routine. We gather IOCs from various sources, including external vulnerability registers, email lists, news reports, and our own analysis of alarms and network traffic. You might be familiar with OpenCTI, a tool that assists in this process.

Remember, IOCs are indications of events that have already happened, based on evidence found by you or others. Understanding and effectively utilizing IOAs, IOBs, and IOCs are essential for robust threat detection and response in our security operations.

## In short 

### Summarizing IOA, IOB, and IOC

To encapsulate the key concepts of IOA, IOB, and IOC, we can outline their primary distinctions as follows:

| **Type** | **Description** |
|----------|-----------------|
| **IOA**  | Indicators of Attack (IOA) deal with signs that an attack is occurring or is about to occur. They signal potential threats based on suspicious activities or precursors. |
| **IOB**  | Indicators of Behavior (IOB) focus on behavioral patterns and deviations from established norms. They help identify anomalies by comparing current actions against a baseline of normal activity. |
| **IOC**  | Indicators of Compromise (IOC) are the tangible traces left behind by attackers. These include specific artifacts like IP addresses, domain names, and file hashes that indicate a system has been compromised. |

**Understanding the Differences:**

To clarify the distinctions between IOA, IOB, and IOC:

- **IOA (Indicator of Attack)** provides early warning signs that an attack might be in progress or imminent. For instance, a sudden surge in traffic to a website could be a potential IOA.

- **IOB (Indicator of Behavior)** focuses on identifying deviations from normal behavior patterns. By establishing a baseline of typical activities, we can detect unusual behavior that might indicate malicious activity.

- **IOC (Indicator of Compromise)** pertains to tangible evidence left by attackers. This could include specific IP addresses, domain names, or other artifacts that confirm a system has been compromised. When discussing IOCs and threat hunting, it's important to clarify that IOCs in this context refer to those that we, as threat hunters, discover ourselves. The essence of threat hunting goes beyond merely validating IOCs provided by others; it involves proactively identifying and uncovering new IOCs through our own investigative efforts. Effective threat hunting is about detecting and addressing potential threats independently, rather than relying solely on pre-existing indicators handed to us.

**Integrating IOA, IOB, and IOC:**

We can derive IOCs from IOAs. For instance, consider a sudden increase in traffic to a website:
- **IOC Extraction:** 
  - The IP address from which the traffic originates can be treated as an IOC.
  - Examine the IP’s ASN (Autonomous System Number) and geolocation.
  - Analyze the nature of the traffic (e.g., unusual requests or patterns).
  - Check for any previous reports of suspicious activity associated with this IP.

**Combining IOA and IOB:**
- **Context Building:** IOA helps us identify that something is happening, while IOB provides context by comparing current activities to historical patterns. This combination allows us to assess whether the detected activity is typical or unusual.

**Practical Application:**

By integrating these concepts, we achieve a comprehensive analysis:
- **IOA** identifies potential threats.
- **IOC** provides specific indicators to investigate.
- **IOB** helps determine whether the observed activity deviates from established norms.

With this approach, we build context and enhance our analysis, allowing us to address and respond to potential threats more effectively.

**Action and Reaction:**

As we delve deeper into IOCs, remember Newton's Third Law of Motion:

> "For every action, there is an equal and opposite reaction."

In the context of cybersecurity, this principle underscores the importance of understanding the implications of each action we take. Our response to detected indicators must be measured and well-considered to ensure effective mitigation and protection.

### Integrating Psychology and Methodology in Analysis

The psychology and methodology underpinning an analyst's mindset are both extensive and complex, yet profoundly intriguing. Our aim has been to identify models that align with the spirit of effective analysis and translate these models into actionable practices. By synthesizing the concepts discussed today, we can enhance our analytical capabilities and be better prepared for the challenges we face.

As Skinner aptly remarks:

> "Being wrong isn't always wrong—it might be the best thing we could do given the situation."

This insight highlights the value of acknowledging errors and iterating on our processes. Embracing the possibility of failure and the need to refine our approach can be a powerful advantage. This adaptability is a key strength of the OODA loop (Observe, Orient, Decide, Act)—a framework that encourages us to continually reassess and restart our analysis with newly acquired information. We will delve into OODA later on in this book. 

By integrating these psychological insights and methodologies, we not only improve our analytical effectiveness but also cultivate a mindset that embraces flexibility and growth.

## Resources

* [What is a threat hunter?](https://www.offsec.com/cybersecurity-roles/threat-hunter/#:~:text=A%20threat%20hunter%20must%20have,effective%20communication%20skills%20are%20crucial.)
* [Your Next Move: Threat Hunter](https://www.comptia.org/blog/your-next-move-threat-hunter)
* [SOC ANALYST - Threat hunter](https://www.infosecinstitute.com/resources/soc-analyst/threat-hunter/)
* [How to Become a Cyber Threat Hunter: A Guide to Level Up Your Security Team](https://www.snapattack.com/become-a-threat-hunter/)
* [Threat Hunting Series: What Makes a Good Threat Hunter](https://kostas-ts.medium.com/threat-hunting-series-what-makes-a-good-threat-hunter-e2b1d0d07e8c)