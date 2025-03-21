---
title: "Windows Registry"
date: 2025-03-21T20:10:17+01:00
draft: false
hidden: false
weight: 6
tags:
    - cheatsheet
    - registry
    - windows
    - microsoft
summary: ""
---

|Revised Date | Author | Comment |
| ----------- | ------ | ------- |
| 21.03.2025  | Roger Johnsen | Article added |

## Introduction

**The Windows Registry is a critical component of the Windows operating system that serves as a hierarchical database for storing system configurations, application settings, and user preferences. For threat hunters, it is an invaluable resource for detecting malicious activity, identifying persistence mechanisms, and analyzing system behavior. Attackers frequently leverage the registry to hide malware, execute fileless attacks, or establish persistence by modifying startup keys and injecting code into registry values. By monitoring and analyzing registry changes, threat hunters can uncover unauthorized modifications, trace the origins of security breaches, and mitigate stealthy threats such as Advanced Persistent Threats (APTs). Its vast scope of information makes the registry a goldmine for forensic investigations and proactive threat detection.**

---

## Windows Registry Keys

### System Information and Configuration
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKLM\SYSTEM\CurrentControlSet\Control\ComputerName` | System's assigned name | Computer name (e.g., `DESKTOP-1234AB`) | Check for unexpected name changes | [T1082](https://attack.mitre.org/techniques/T1082/) |
| `HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation` | System's time zone | Time zone string (e.g., `Pacific Standard Time`) | Verify time zone consistency across network | [T1082](https://attack.mitre.org/techniques/T1082/) |
| `HKLM\SYSTEM\CurrentControlSet\Control\Windows` | Last shutdown time | `ShutdownTime` (Binary value) | FILETIME format; check for unexpected shutdowns | [T1082](https://attack.mitre.org/techniques/T1082/) |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\EnableLUA` | UAC settings | 0 (disabled) or 1 (enabled) | Ensure UAC is enabled (value should be 1) | [T1548.002](https://attack.mitre.org/techniques/T1548/002/) |

### User Accounts and Profiles
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList` | User SID to profile path mapping | SID and profile path pairs | Identify unauthorized user accounts | [T1136](https://attack.mitre.org/techniques/T1136/) |
| `HKLM\SAM\Domains\Account\Users` | User account details (protected) | Binary data, SID-linked user records | Requires special tools; check for unauthorized changes | [T1003](https://attack.mitre.org/techniques/T1003/) |

### Autostart and Persistence Mechanisms
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` | Auto-launch programs at startup | Executable paths | Common persistence mechanism; check for unusual entries | [T1547.001](https://attack.mitre.org/techniques/T1547/001/) |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache` | Scheduled tasks | Task names, executable paths, run times | Check for unauthorized scheduled tasks | [T1053.005](https://attack.mitre.org/techniques/T1053/005/) |
| `HKLM\SYSTEM\CurrentControlSet\Services` | Installed Windows services | Service name, binary path | Identify potentially malicious services | [T1543.003](https://attack.mitre.org/techniques/T1543/003/) |

### Network Configuration and Remote Access
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces` | Network interface configurations | IP addresses, subnet masks, gateways | Check for unauthorized network changes | [T1016](https://attack.mitre.org/techniques/T1016/) |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged` | Previously connected networks | SSID, MAC address, timestamps | Trace system movement; timestamps in Unix format | [T1012](https://attack.mitre.org/techniques/T1012/) |
| `HKCU\Software\Microsoft\Terminal Server Client\Servers` | RDP accessed hosts | IP addresses, hostnames | Identify unauthorized remote access | [T1021.001](https://attack.mitre.org/techniques/T1021/001/) |
| `HKCU\Network` | Mapped network drives | Drive letter, remote path | Check for suspicious network mappings | [T1021](https://attack.mitre.org/techniques/T1021/) |
| `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\DisableRestrictedAdmin` | RDP Restricted Admin mode | 0 (enabled) or 1 (disabled) | Should be 0 for better security | [T1021.001](https://attack.mitre.org/techniques/T1021/001/) |

### USB and External Device Usage
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR` | Connected USB storage devices | Vendor ID, product ID, serial number | Identify potential data exfiltration attempts | [T1091](https://attack.mitre.org/techniques/T1091/) |
| `HKLM\SYSTEM\MountedDevices` | Volume GUID to drive letter mapping | Drive letter mappings | Track external device usage | [T1091](https://attack.mitre.org/techniques/T1091/) |

### User Activity and Program Execution
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` | Run box command history | String list of executed commands | Can reveal attacker activities | [T1112](https://attack.mitre.org/techniques/T1112/) |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU` | Accessed files and folders history | File paths and timestamps | Identify accessed sensitive files | [T1083](https://attack.mitre.org/techniques/T1083/) |
| `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache` | Executed program metadata (Shimcache) | File paths, timestamps, execution flags | Reveals program execution even if deleted | [T1118](https://attack.mitre.org/techniques/T1118/) |
| `HKLM\SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}` | Recently executed applications per user | File paths, timestamps | Background Activity Moderator; check for suspicious executions | [T1118](https://attack.mitre.org/techniques/T1118/) |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist` | GUI-launched programs (ROT13 encoded) | Encrypted program names, execution count | Requires ROT13 decoding; reveals user activities | [T1118](https://attack.mitre.org/techniques/T1118/) |

### Software and Application Management
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` | Installed applications | Application names, install dates | Identify potentially malicious software | [T1072](https://attack.mitre.org/techniques/T1072/) |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options` | Debugger redirection | Debugger path | Check for potential process hijacking | [T1546.012](https://attack.mitre.org/techniques/T1546/012/) |

### Security and Credential Management
| Registry Key | Description | Possible Values | Threat Hunting Notes | MITRE ATT&CK |
|--------------|-------------|-----------------|----------------------|--------------|
| `HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest` | WDigest credential storage setting | `UseLogonCredential` (`0` or `1`) | Should be 0 to prevent credential storage | [T1003](https://attack.mitre.org/techniques/T1003/) |