---
title: "Windows Registry Hunting"
date: 2025-03-21T20:10:17+01:00
draft: false
hidden: false
weight: 9
tags: [cheatsheet, registry, windows, microsoft]
summary: "High-value Windows Registry paths, artefacts, caveats, and pivots."
---

__Author:__ _Roger C.B. Johnsen_

Registry data is both live configuration and historical evidence. A key's presence is rarely enough: collect value name, data, hive, user SID, last-write time, writing process, and surrounding activity. Remember 32-bit and 64-bit views, per-user hives, `CurrentControlSet` resolution, and offline hive paths.

## Persistence and execution

| Path | Hunt focus |
|---|---|
| `HKLM/HKCU\Software\Microsoft\Windows\CurrentVersion\Run*` | New or modified values, user-writable payload paths, script hosts. |
| `HKLM\SYSTEM\CurrentControlSet\Services` | New services/drivers; inspect `ImagePath`, `Start`, account, and failure actions. |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache` | Correlate with task XML and Task Scheduler logs; do not parse in isolation. |
| `...\Image File Execution Options\<image>` | Unexpected `Debugger`, `GlobalFlag`, or verifier settings. |
| `...\SilentProcessExit` | Pair with IFEO; can trigger monitor processes. |
| `HKLM/HKCU\Software\Classes\CLSID` | COM hijacking; compare user overrides with machine registrations. |
| `HKCU\Software\Classes\mscfile\shell\open\command` | Unexpected handler changes and UAC-bypass patterns. |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon` | Changes to `Shell`, `Userinit`, and notification components. |
| `HKLM\SYSTEM\CurrentControlSet\Control\Lsa` | Authentication packages, security packages, and LSA protection changes. |
| `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager` | `BootExecute`, AppCertDlls, and other early execution settings. |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows\AppInit_DLLs` | Legacy DLL injection settings; OS protections affect behaviour. |
| `HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components` | Per-user execution at logon. |
| `HKCU\Environment` | User environment and logon execution abuse, including `UserInitMprLogonScript`. |

## Security controls and remote access

| Path/value | Hunt focus |
|---|---|
| `...\Policies\Microsoft\Windows Defender` and Defender policy paths | Tamper attempts; interpret with Tamper Protection and product events. |
| `...\Policies\Microsoft\Windows\PowerShell` | Script block/module logging and transcription changes. |
| `HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest\UseLogonCredential` | Enabling clear-text credential caching. |
| `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\RunAsPPL` | LSA protection state and downgrades. |
| `HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\fDenyTSConnections` | RDP enablement; correlate firewall and service changes. |
| `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\DisableRestrictedAdmin` | Despite the name, `0` enables Restricted Admin Mode and `1` disables it; easy to invert when reading quickly. |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\EnableLUA` | UAC changes; reboot and policy context matter. |

## Forensic and activity artefacts

| Path | What it can support | Caveat |
| ---- | ------------------- | ------ |
| `HKLM\SYSTEM\MountedDevices` and `...\Enum\USBSTOR` | Volume and USB-device association | Presence does not prove malicious transfer. |
| `HKCU\Software\Microsoft\Terminal Server Client\Servers` | RDP destination history | Per-user artefact; deletion and roaming affect completeness. |
| `HKCU\Network` | Mapped drive configuration | Correlate with SMB and logon telemetry. |
| `HKCU\...\Explorer\RunMRU` | Run-dialog history | User interaction evidence, not a complete execution record. |
| `HKCU\...\Explorer\UserAssist` | GUI program-use artefacts | ROT13 names and version-specific binary structures. |
| `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache` | Program presence and compatibility metadata | Do not claim execution solely from Shimcache. |
| `HKLM\SYSTEM\CurrentControlSet\Services\bam\State\UserSettings\<SID>` | Recent executable activity | Version, shutdown, and retention behaviour matter. |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` | Installed software metadata | Also inspect Wow6432Node and per-user equivalents. |

## Collection and pivots

Collect Sysmon Events 12-14, Defender `DeviceRegistryEvents`, Security process creation, and relevant service/task logs. For offline work, preserve `SYSTEM`, `SOFTWARE`, `SAM`, `SECURITY`, `NTUSER.DAT`, and `UsrClass.dat` plus transaction logs.

1. Resolve the hive and SID.
2. Capture old and new value data and the writing process.
3. Expand environment variables and inspect the referenced file.
4. Correlate process, logon, service, task, file, and network telemetry.
5. Compare across peers and policy baselines before declaring maliciousness.

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2025-03-21 | Article added |
| 2026-07-22 | Corrected semantics and added artefacts, caveats, and pivots |
| 2026-07-22 | Corrected the DisableRestrictedAdmin value semantics after QA |
