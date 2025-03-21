---
title: "Sysmon"
date: 2025-03-21T20:10:23+01:00
draft: false
hidden: false
weight: 5
tags:
    - cheatsheet
    - sysmon
    - events
    - windows
summary: ""
---

|Revised Date | Author | Comment |
| ----------- | ------ | ------- |
| 21.03.2025  | Roger Johnsen | Article added |

## Introduction

**Sysmon event IDs are a powerful tool for threat hunters, offering detailed insights into system activities that help detect malicious behavior and abnormal patterns. By capturing critical events like process creation, network connections, and registry modifications, Sysmon enhances visibility into Windows environments. These logs are crucial for identifying persistence mechanisms, lateral movement, and other tactics outlined in the MITRE ATT&CK framework. By leveraging Sysmon event IDs, security teams can proactively hunt for threats, investigate incidents, and strengthen their organization's security posture.**

---

## Sysmon Event IDs

| Event ID | Name | Description | MITRE ATT&CK Technique |
|----------|------|-------------|------------------------|
| 1 | Process Creation | Logs the creation of new processes | [T1059: Command and Scripting Interpreter](https://attack.mitre.org/techniques/T1059/), [T1204: User Execution](https://attack.mitre.org/techniques/T1204/) |
| 2 | File Creation Time Changed | Detects changes to file creation timestamps | [T1070.006: Timestomp](https://attack.mitre.org/techniques/T1070/006/) |
| 3 | Network Connection | Records network connections and related processes | [T1071: Application Layer Protocol](https://attack.mitre.org/techniques/T1071/), [T1021: Remote Services](https://attack.mitre.org/techniques/T1021/) |
| 4 | Sysmon Service State Changed | Logs changes to the Sysmon service state | N/A |
| 5 | Process Terminated | Records when a process ends | [T1480: Execution Guardrails](https://attack.mitre.org/techniques/T1480/), [T1562: Impair Defenses](https://attack.mitre.org/techniques/T1562/) |
| 6 | Driver Loaded | Detects when a driver is loaded on the system | [T1547.006: Boot or Logon Autostart Execution: Kernel Modules and Extensions](https://attack.mitre.org/techniques/T1547/006/), [T1014: Rootkit](https://attack.mitre.org/techniques/T1014/) |
| 7 | Image Loaded | Logs when a module is loaded in a process | [T1574: Hijack Execution Flow](https://attack.mitre.org/techniques/T1574/), [T1129: Shared Modules](https://attack.mitre.org/techniques/T1129/) |
| 8 | CreateRemoteThread | Detects when a process creates a thread in another process | [T1055: Process Injection](https://attack.mitre.org/techniques/T1055/), [T1056.004: Credential API Hooking](https://attack.mitre.org/techniques/T1056/004/) |
| 9 | RawAccessRead | Detects when a process conducts raw reading of a drive | [T1006: Direct Volume Access](https://attack.mitre.org/techniques/T1006/), [T1003: OS Credential Dumping](https://attack.mitre.org/techniques/T1003/) |
| 10 | ProcessAccess | Logs when a process opens another process | [T1057: Process Discovery](https://attack.mitre.org/techniques/T1057/), [T1003: OS Credential Dumping](https://attack.mitre.org/techniques/T1003/) |
| 11 | FileCreate | Records file creation events | [T1105: Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/), [T1078: Valid Accounts](https://attack.mitre.org/techniques/T1078/) |
| 12 | RegistryEvent (Object create and delete) | Detects registry key and value create and delete operations | [T1112: Modify Registry](https://attack.mitre.org/techniques/T1112/), [T1547.001: Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder](https://attack.mitre.org/techniques/T1547/001/) |
| 13 | RegistryEvent (Value Set) | Logs registry value modifications | [T1112: Modify Registry](https://attack.mitre.org/techniques/T1112/), [T1546.001: Event Triggered Execution: Change Default File Association](https://attack.mitre.org/techniques/T1546/001/) |
| 14 | RegistryEvent (Key and Value Rename) | Records registry key and value rename operations | [T1112: Modify Registry](https://attack.mitre.org/techniques/T1112/), [T1070: Indicator Removal on Host](https://attack.mitre.org/techniques/T1070/) |
| 15 | FileCreateStreamHash | Logs the creation of alternate data streams | [T1564.004: Hide Artifacts: NTFS File Attributes](https://attack.mitre.org/techniques/T1564/004/), [T1027: Obfuscated Files or Information](https://attack.mitre.org/techniques/T1027/) |
| 16 | ServiceConfigurationChange | Detects changes to service configurations | [T1543: Create or Modify System Process](https://attack.mitre.org/techniques/T1543/), [T1569.002: System Services: Service Execution](https://attack.mitre.org/techniques/T1569/002/) |
| 17 | PipeEvent (Pipe Created) | Logs when named pipes are created | [T1559: Inter-Process Communication](https://attack.mitre.org/techniques/T1559/), [T1021.002: Remote Services: SMB/Windows Admin Shares](https://attack.mitre.org/techniques/T1021/002/) |
| 18 | PipeEvent (Pipe Connected) | Records when a named pipe connection is made | [T1559: Inter-Process Communication](https://attack.mitre.org/techniques/T1559/), [T1570: Lateral Tool Transfer](https://attack.mitre.org/techniques/T1570/) |
| 19 | WmiEvent (WmiEventFilter activity detected) | Detects WMI event filter creation | [T1047: Windows Management Instrumentation](https://attack.mitre.org/techniques/T1047/), [T1546.003: Event Triggered Execution: Windows Management Instrumentation Event Subscription](https://attack.mitre.org/techniques/T1546/003/) |
| 20 | WmiEvent (WmiEventConsumer activity detected) | Logs WMI event consumer creation | [T1047: Windows Management Instrumentation](https://attack.mitre.org/techniques/T1047/), [T1546.003: Event Triggered Execution: Windows Management Instrumentation Event Subscription](https://attack.mitre.org/techniques/T1546/003/) |
| 21 | WmiEvent (WmiEventConsumerToFilter activity detected) | Records WMI filter to consumer bindings | [T1047: Windows Management Instrumentation](https://attack.mitre.org/techniques/T1047/), [T1546.003: Event Triggered Execution: Windows Management Instrumentation Event Subscription](https://attack.mitre.org/techniques/T1546/003/) |
| 22 | DNSEvent (DNS query) | Logs DNS queries and responses | [T1071.004: Application Layer Protocol: DNS](https://attack.mitre.org/techniques/T1071/004/), [T1568: Dynamic Resolution](https://attack.mitre.org/techniques/T1568/) |
| 23 | FileDelete (File Delete archived) | Records file deletion events | [T1070.004: Indicator Removal on Host: File Deletion](https://attack.mitre.org/techniques/T1070/004/), [T1485: Data Destruction](https://attack.mitre.org/techniques/T1485/) |
| 24 | ClipboardChange | Detects changes to the system clipboard | [T1115: Clipboard Data](https://attack.mitre.org/techniques/T1115/), [T1056.001: Input Capture: Keylogging](https://attack.mitre.org/techniques/T1056/001/) |
| 25 | ProcessTampering | Logs attempts to tamper with process memory | [T1562: Impair Defenses](https://attack.mitre.org/techniques/T1562/), [T1055: Process Injection](https://attack.mitre.org/techniques/T1055/) |
| 26 | FileDeleteDetected | Detects file deletion operations | [T1070.004: Indicator Removal on Host: File Deletion](https://attack.mitre.org/techniques/T1070/004/), [T1107: File Deletion](https://attack.mitre.org/techniques/T1107/) |
| 27 | FileBlockExecutable | Logs when an executable file is blocked from running | [T1562: Impair Defenses](https://attack.mitre.org/techniques/T1562/), [T1036: Masquerading](https://attack.mitre.org/techniques/T1036/) |
| 28 | FileBlockShredding | Detects attempts to securely delete files | [T1070.004: Indicator Removal on Host: File Deletion](https://attack.mitre.org/techniques/T1070/004/), [T1561: Disk Wipe](https://attack.mitre.org/techniques/T1561/) |
| 29 | FileExecutableDetected | Records when an executable file is detected | [T1204: User Execution](https://attack.mitre.org/techniques/T1204/), [T1569: System Services](https://attack.mitre.org/techniques/T1569/) |
| 255 | Error | Indicates an error condition in Sysmon | N/A |