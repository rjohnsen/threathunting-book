---
title: "Pyramid of Pain"
description: "A practical explanation of the Pyramid of Pain and how threat hunters can use it to prioritise indicators, detection logic and adversary disruption."
date: 2024-06-23T20:15:33+02:00
lastmod: 2026-07-10
draft: false
weight: 6
tags:
   - frameworks
   - threat hunting
   - Pyramid of Pain
   - detection engineering
keywords:
   - Pyramid of Pain
   - David Bianco
   - threat hunting
   - detection engineering
   - indicators of compromise
   - indicators of behaviour
   - IOC
   - TTP
   - MITRE ATT&CK
   - adversary disruption
----------------------

**Author:** *Roger C.B. Johnsen*

## Introduction

**The Pyramid of Pain is a model for thinking about how much difficulty we create for an adversary when we detect, block or disrupt different types of indicators.**

The model was created by David J. Bianco and is widely used in threat hunting, incident response, detection engineering and threat intelligence. It is simple, but it teaches an important lesson: not all indicators have the same defensive value.

Some indicators are easy for the adversary to change. A file hash can be changed by recompiling or slightly modifying a file. An IP address can be replaced. A domain can be abandoned. These indicators can still be useful, especially during an active investigation, but they usually create limited long-term pain for the adversary.

Other indicators are harder to change. Tooling, host artefacts, network artefacts and tactics, techniques and procedures are closer to how the adversary actually works. When defenders detect and disrupt those, the adversary may have to change behaviour, tooling, infrastructure, workflow or tradecraft.

The higher levels of the pyramid force the adversary to change how they operate, not only what they use.

That is the point of the pyramid.

It helps the analyst ask:

```text
Are we only blocking what the adversary used this time?
Or are we making it harder for the adversary to operate next time?
```

For threat hunters, that distinction matters. A hunt that only produces a list of hashes and IP addresses may still be useful, but it is usually not enough. A stronger hunt also looks for behaviours, artefacts and patterns that can be turned into more durable detection and response logic.

> The Pyramid of Pain reminds us that not all indicators hurt the adversary equally.
>
> -- Roger Johnsen

## What the Pyramid of Pain Is

The Pyramid of Pain is usually shown as a pyramid with the easiest indicators at the bottom and the most painful indicators at the top.

![Pyramid of Pain](/images/Pyramid-of-Pain-v2.png)

The model contains six levels:

| Level                              | Indicator type                                                      | Defensive value                                                              |
| ---------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Hash Values                        | Specific hashes of files or payloads.                               | Useful, but easy for the adversary to change.                                |
| IP Addresses                       | Specific IP addresses used by infrastructure.                       | Useful, but often temporary or replaceable.                                  |
| Domain Names                       | Domains used for phishing, payload delivery or command and control. | More durable than IPs, but still replaceable.                                |
| Network and Host Artefacts         | Observable patterns on systems or networks.                         | Harder to change because they reflect implementation details.                |
| Tools                              | Tools used by the adversary.                                        | More painful to lose if the adversary depends on them.                       |
| Tactics, Techniques and Procedures | How the adversary operates.                                         | Most painful because changing tradecraft is harder than changing indicators. |

The higher we move in the pyramid, the more we affect the adversary’s way of working.

That does not mean the lower levels are useless. Hashes, IPs and domains are often very useful during triage, containment and scoping. They are fast to search for, easy to share and can help identify known activity.

The problem appears when teams treat those low-level indicators as the whole defensive strategy.

A blocklist is not a hunting programme. A hash match is not behavioural understanding. An IP address does not explain the intrusion.

The Pyramid of Pain helps us keep that distinction visible.

## Hash Values

Hash values are specific cryptographic fingerprints of files.

Examples include:

* MD5
* SHA-1
* SHA-256

Hashes are useful when the exact file is known. If a malicious payload has already been identified, the hash can be searched across endpoints, used for containment, added to blocklists or shared with other teams.

That makes hashes valuable during incident response.

But they are also fragile.

A small change to the file changes the hash. The adversary can recompile the malware, pack it differently, modify a resource, add junk data or use a different loader. The behaviour may remain similar, but the hash will no longer match.

Possible hunting questions:

| Question                                       | Why it matters                             |
| ---------------------------------------------- | ------------------------------------------ |
| Where else does this exact file exist?         | Helps scope known activity.                |
| When did the file first appear?                | Helps build timeline.                      |
| Which user or process wrote the file?          | Helps connect the file to activity.        |
| Did the file execute?                          | Separates presence from impact.            |
| Are there similar files with different hashes? | Moves the hunt above simple hash matching. |

Hashes are useful for finding what is already known.

They are weak when used as the only way to understand adversary activity.

## IP Addresses

IP addresses identify specific network locations used by infrastructure.

They may represent:

* command-and-control servers
* payload hosting
* phishing infrastructure
* proxy nodes
* VPN endpoints
* compromised systems
* cloud infrastructure
* scanning sources

IP addresses are useful because they are visible in firewall logs, proxy logs, DNS resolutions, NetFlow, EDR network telemetry and other network data.

During an investigation, an IP address can help answer important questions:

| Question                                                  | Why it matters                                        |
| --------------------------------------------------------- | ----------------------------------------------------- |
| Which hosts connected to this IP?                         | Helps identify potentially affected systems.          |
| When did communication occur?                             | Helps build timeline and sequence.                    |
| Which process initiated the connection?                   | Helps connect network activity to endpoint behaviour. |
| Was the IP rare in the environment?                       | Helps separate normal from unusual activity.          |
| Is the IP associated with known malicious infrastructure? | Adds threat intelligence context.                     |

But IP addresses are easy to replace. Attackers can move infrastructure, use cloud providers, rotate proxies or abuse compromised systems.

That does not make IPs useless. It means they should be treated as starting points, not final understanding.

A good hunt rarely stops at:

```text
This host connected to a bad IP.
```

A stronger hunt asks:

```text
What behaviour led to the connection, which process made it, which user was involved, and where else do we see similar communication patterns?
```

## Domain Names

Domain names are often more useful than raw IP addresses because they may remain stable while the underlying IP changes.

Domains may be used for:

* phishing
* payload delivery
* command and control
* credential harvesting
* staging
* tracking
* redirection
* impersonation

Domains can be searched in DNS logs, proxy logs, browser telemetry, email logs and threat intelligence platforms.

Possible hunting questions:

| Question                                                 | Why it matters                               |
| -------------------------------------------------------- | -------------------------------------------- |
| Which users or hosts resolved the domain?                | Helps scope exposure.                        |
| Was the domain clicked from email?                       | Connects delivery to user interaction.       |
| Did resolution lead to HTTP or HTTPS traffic?            | Helps distinguish lookup from communication. |
| Is the domain newly registered or rare?                  | Adds suspicion and context.                  |
| Are there related domains using similar naming patterns? | Helps pivot to infrastructure clusters.      |

Domains are more painful for adversaries than IP addresses because they may be embedded in malware, phishing campaigns, certificates, infrastructure configuration or user-facing lures.

Still, domains can be replaced.

That is why domain-based hunting should normally lead to deeper questions about infrastructure, delivery, user interaction, payloads and behaviour.

## Network and Host Artefacts

Network and host artefacts are more interesting because they describe how activity appears in the environment.

Examples of network artefacts include:

* unusual HTTP headers
* URI patterns
* beacon timing
* TLS certificate patterns
* JA3 or JA4-like fingerprints
* DNS query patterns
* user-agent strings
* uncommon protocols
* repeated connection intervals

Examples of host artefacts include:

* file paths
* registry keys
* scheduled task names
* service names
* mutexes
* process relationships
* command-line patterns
* unusual parent-child process chains
* persistence locations
* script structures

These artefacts are harder for the adversary to change than a hash or IP address because they often reflect how tooling is built or how the operation is executed.

For threat hunters, this is where the work becomes more useful.

Instead of asking only whether a known bad indicator appeared, the hunter can ask whether a pattern of behaviour exists.

For example:

| Low-level indicator   | Higher-value artefact question                                                 |
| --------------------- | ------------------------------------------------------------------------------ |
| Known malicious hash  | Are there similar files with the same path, naming pattern or execution chain? |
| Known C2 IP           | Are there hosts with similar beacon timing or HTTP patterns?                   |
| Known phishing domain | Are there similar domains, redirects or email lure patterns?                   |
| Known malware sample  | Are there similar persistence artefacts or process behaviours?                 |

Artefacts are valuable because they help the defender move from indicator matching to behavioural detection.

They also create better detection engineering opportunities.

A single IP block may stop one connection. A detection for rare Office-spawned PowerShell followed by outbound network activity may catch a broader pattern.

## Tools

Tools are the software, scripts, frameworks or utilities used by the adversary.

These may include:

* malware families
* credential dumping tools
* remote access tools
* offensive security frameworks
* web shells
* loaders
* scripts
* living-off-the-land binaries
* administrative tools used in suspicious ways

Detecting tools can cause more pain because the adversary may depend on them. If a tool is reliably detected, the adversary may need to modify it, replace it, change execution methods or alter the operation.

But tool detection has a trap. A tool name is not enough.

For example, detecting “Mimikatz” by filename is weaker than detecting credential dumping behaviour. Detecting a known tool hash is weaker than detecting suspicious access to LSASS. Detecting a known PsExec binary is weaker than understanding remote service creation, lateral authentication and command execution patterns.

In many cases, what looks like tool detection is really artefact detection. A filename, hash, default path or static signature may identify one version of a tool, but it does not necessarily detect the behaviour that makes the tool useful to the adversary. Real tool-level pain begins when the defender understands and detects the way the tool is used.

The useful question is not only:

```text
Did we detect the tool?
```

The better question is:

```text
Which behaviours make this tool useful to the adversary, and can we detect those behaviours even if the tool changes?
```

This is where tool-focused detection starts to move toward TTP-focused detection.

## Tactics, Techniques and Procedures

Tactics, techniques and procedures sit at the top of the pyramid because they describe how the adversary operates.

This is the hardest level for the adversary to change. A hash can be changed quickly. An IP can be replaced. A domain can be abandoned. A tool can be modified or swapped. But changing tradecraft is harder. It may require new training, new tooling, new infrastructure, new testing and a different way of running the operation.

Examples of TTP-focused hunting include:

* detecting credential dumping behaviour rather than a specific credential dumping tool
* detecting suspicious lateral movement patterns rather than a single remote access binary
* detecting unusual data staging before exfiltration rather than one known upload destination
* detecting identity abuse patterns rather than one known malicious IP
* detecting persistence behaviour rather than one scheduled task name
* detecting discovery activity rather than one specific command

This is where MITRE ATT&CK becomes useful. ATT&CK gives us a vocabulary for describing techniques. The Pyramid of Pain helps us understand why detecting technique-level behaviour is often more valuable than only detecting low-level indicators.

That does not mean every hunt must start at the top of the pyramid. In practice, many hunts begin with a low-level indicator and move upward.

* A hash may lead to a file path.
* A file path may lead to a process chain.
* A process chain may lead to a technique.
* A technique may lead to a detection or hunt that is useful long after the original hash is gone.

That movement up the pyramid is the practical value.

> The goal is not to ignore low-level indicators. The goal is to use them as starting points and move toward behaviour.
>
> -- Roger Johnsen

## Using the Pyramid During Threat Hunting

The Pyramid of Pain helps threat hunters decide how much value a finding may have.

A simple hunting flow looks like this:

```text
Indicator → Context → Behaviour → Detection opportunity → Adversary disruption
```

For example, a hunt may begin with a known malicious domain from threat intelligence.

A weak hunt asks:

```text
Who connected to this domain?
```

That is a valid question, but it is incomplete.

A stronger hunt asks:

```text
Who connected to this domain, what process made the connection, what happened before and after, are there similar domains, and do we see the same behaviour with other infrastructure?
```

This turns one domain into a path for understanding behaviour.

The Pyramid of Pain helps the hunter avoid getting stuck at the lowest available indicator. It encourages movement from simple matches toward patterns that may create lasting defensive value.

## Practical Example: From Hash to Behaviour

Consider a simplified investigation where an organisation receives a malicious file hash from threat intelligence.

At the bottom of the pyramid, the team searches for the hash.

| Step             | Finding                                                                         |
| ---------------- | ------------------------------------------------------------------------------- |
| Hash             | The file exists on one endpoint.                                                |
| IP address       | The endpoint contacted an external IP after execution.                          |
| Domain           | DNS logs show a related domain shortly before the connection.                   |
| Host artefact    | The file executed from a suspicious user-writable path.                         |
| Network artefact | The host made repeated outbound HTTPS connections at regular intervals.         |
| Tool             | The behaviour resembles a known loader or remote access tool.                   |
| TTP              | The activity suggests execution, persistence and command-and-control behaviour. |

The hash was useful. It gave the team a starting point. If the investigation had stopped at the hash, the behaviour may have remained undetected on other hosts using different files, infrastructure or tooling. But the real value came from moving upward.

The team can now ask better questions:

* Do other hosts show the same execution path?
* Do other hosts show the same beacon timing?
* Do other files use similar naming or staging patterns?
* Did the same user receive a suspicious email?
* Did the activity create persistence?
* Can the behaviour be turned into detection logic?

This is how a low-level indicator becomes a useful hunt.

## Detection Engineering and the Pyramid

The Pyramid of Pain is also useful for detection engineering.

Detection logic based only on low-level indicators is often brittle. It may work today and fail tomorrow when the adversary changes the hash, rotates infrastructure or modifies the payload.

Detection logic based on behaviour tends to be more durable, but it is also harder to build well.

For example:

| Detection level | Example                                                   | Strength                                            |
| --------------- | --------------------------------------------------------- | --------------------------------------------------- |
| Hash            | Alert on known malicious SHA-256                          | Fast and precise, but fragile.                      |
| IP              | Alert on connection to known C2 IP                        | Useful for known infrastructure, but easy to evade. |
| Domain          | Alert on known malicious domain                           | Useful for known campaigns, but replaceable.        |
| Artefact        | Alert on suspicious path, process chain or beacon pattern | More durable and behaviour-oriented.                |
| Tool            | Alert on tool behaviour or execution pattern              | Stronger if not limited to filename or hash.        |
| TTP             | Alert on technique-level behaviour                        | More durable, but requires context and tuning.      |

This does not mean every detection should be complex. Simple detections are still useful. The question is whether the detection is honest about what it covers. A hash alert should not be described as broad malware coverage. An IP block should not be described as command-and-control detection. A tool signature should not be mistaken for understanding the technique.

The pyramid helps detection engineers explain what kind of detection they have built and how much pain it may create for the adversary.

## Pyramid of Pain and ATT&CK

The Pyramid of Pain and MITRE ATT&CK work well together, but they answer different questions.

| Framework       | Main value                                                                        |
| --------------- | --------------------------------------------------------------------------------- |
| Pyramid of Pain | Helps prioritise indicators by how much disruption they create for the adversary. |
| MITRE ATT&CK    | Provides vocabulary for describing adversary behaviour and techniques.            |

ATT&CK helps describe the technique. The Pyramid of Pain helps decide whether the detection or hunt is focused on something fragile or something durable.

For example:

| Observation                                                    | ATT&CK angle                           | Pyramid angle                          |
| -------------------------------------------------------------- | -------------------------------------- | -------------------------------------- |
| Known malware hash                                             | May relate to Execution or Persistence | Hash value: low pain                   |
| C2 IP address                                                  | Command and Control                    | IP address: low pain                   |
| Suspicious domain                                              | Command and Control or Delivery        | Domain name: moderate pain             |
| Office spawning PowerShell                                     | Execution                              | Host artefact / behaviour: higher pain |
| LSASS access pattern                                           | Credential Access                      | Tool or TTP: higher pain               |
| Repeated internal discovery followed by lateral authentication | Discovery / Lateral Movement           | TTP: high pain                         |

Together, the two models help the hunter ask a better question:

```text
Are we only naming the technique, or are we detecting it in a way that meaningfully disrupts the adversary?
```

## Where the Pyramid Fits With Other Frameworks

The Pyramid of Pain fits naturally with the other frameworks in this section.

| Framework                  | Main value                                                                         |
| -------------------------- | ---------------------------------------------------------------------------------- |
| Lockheed Martin Kill Chain | Describes intrusion progression and disruption opportunities.                      |
| Unified Kill Chain         | Describes broader adversary progression and operational phases.                    |
| MITRE ATT&CK               | Provides behavioural vocabulary for adversary techniques.                          |
| Diamond Model              | Structures relationships between adversary, infrastructure, capability and victim. |
| OODA Loop                  | Structures decision-making under uncertainty.                                      |
| Pyramid of Pain            | Helps prioritise indicators and detections by adversary disruption.                |

Each framework answers a different question.

The Kill Chain models help us ask where the intrusion is progressing. ATT&CK helps us describe what the adversary is doing. The Diamond Model helps us connect the adversary, infrastructure, capability and victim. OODA helps us think about the analyst’s decision process. The Pyramid of Pain helps us ask how much our detection or response actually hurts the adversary.

That makes the pyramid especially useful when converting hunt findings into detection engineering work.

## What Usually Goes Wrong

Several mistakes are common when teams use the Pyramid of Pain.

| Problem                                    | Why it hurts                                                                                                        |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| Dismissing low-level indicators completely | Hashes, IPs and domains are still useful for triage, scoping and containment.                                       |
| Stopping at low-level indicators           | The team blocks what was used once but does not learn the behaviour.                                                |
| Mistaking IOC volume for maturity          | A team may have many feeds, blocklists and indicators but still remain focused on the lowest levels of the pyramid. |
| Treating all detections as equal           | A hash match and a TTP-focused detection do not create the same adversary pain.                                     |
| Calling tool detection TTP detection       | Detecting a filename or hash is not the same as detecting tradecraft.                                               |
| Ignoring context                           | An artefact only matters when it is interpreted with surrounding evidence.                                          |
| Overbuilding complex detections            | Higher-level detections can become noisy if they lack context and tuning.                                           |
| Failing to feed detection engineering      | The hunt finds useful behaviour but never turns it into durable security work.                                      |

The pyramid should not make the analyst arrogant about low-level indicators. It should make the analyst curious about what those indicators reveal.

## Working Position for This Book

For this book, the Pyramid of Pain is best treated as a prioritisation model for threat hunters and detection engineers.

It helps explain why some indicators are useful but fragile, while others are harder to detect but more disruptive to the adversary.

The practical workflow is:

```text
Start with the indicator you have.
Understand the context.
Move toward behaviour.
Create detection or response value.
```

This is where the model becomes useful.

The point is not to ignore hashes, IPs or domains. The point is to avoid stopping there.

A good hunt may start with a low-level indicator, but it should try to end with better understanding.

> Use low-level indicators to start the investigation. Use behaviour to make the defence stronger.
>
> -- Roger Johnsen

## Resources

* [Pyramid of Pain by David J. Bianco](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html)
* [MITRE ATT&CK](https://attack.mitre.org/)
* [Threat Hunting with the MITRE ATT&CK Framework](https://www.packtpub.com/en-us/product/threat-hunting-with-the-mitre-attck-framework-9781804614260)
* [The Threat Hunter Playbook](https://threathunterplaybook.com/)

## Revision

| Revised Date | Comment                                                                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-10   | Major rewrite. Reframed the article as a practical guide for using the Pyramid of Pain to prioritise indicators, detection logic and adversary disruption. |
| 2024-10-06   | Improved formatting and wording                                                                                                                            |
| 2024-06-23   | Added page                                                                                                                                                 |
