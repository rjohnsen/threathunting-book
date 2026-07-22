---
title: "PowerShell Hunting"
date: 2025-03-21T21:22:12+01:00
draft: false
hidden: false
weight: 7
tags: [microsoft, powershell, cheatsheet]
summary: "PowerShell telemetry, suspicious patterns, and practical pivots."
---

__Author:__ _Roger C.B. Johnsen_

PowerShell is legitimate administration infrastructure and a frequent attack surface. A flag or cmdlet alone is weak evidence. Hunt the full chain: parent process, host, identity, script content, network activity, and follow-on execution.

## Telemetry to collect

| Source | Event/data | Use |
| ------ | ---------- | --- |
| PowerShell Operational | 4103 | Module/pipeline execution when Module Logging is enabled. |
| PowerShell Operational | 4104 | Script block content; reconstruct multipart messages using message number and total. |
| Windows PowerShell | 400/403/600 | Engine and provider lifecycle context. |
| Security | 4688 | Process creation and command line when policy permits. |
| Sysmon | 1, 3, 7, 11, 22 | Process, network, module, file, and DNS context. |
| Defender XDR | `DeviceProcessEvents`, `DeviceNetworkEvents`, `DeviceFileEvents` | Cross-device correlation and pivots. |
| Transcription | Text transcripts | Commands and output; secure access because secrets may be recorded. |

PowerShell 7 writes to `PowerShellCore/Operational`; Windows PowerShell uses `Microsoft-Windows-PowerShell/Operational`. Script Block Logging can expose sensitive data, so protect collected logs.

## High-signal patterns

| Pattern | Why it matters | Validate |
| ------- | -------------- | -------- |
| `-EncodedCommand`, `-enc` | Obscures command text, but is also used by automation. | Decode as UTF-16LE for Windows PowerShell and inspect content. |
| `-WindowStyle Hidden` | Suppresses the console. | Parent, user interaction, scheduled task, and child processes. |
| `-NoProfile` | Produces predictable execution; common in both admin and attack tooling. | Baseline the invoking product and account. |
| `Invoke-Expression` / `iex` | Executes generated strings. | Trace string construction and source. |
| `Invoke-WebRequest`, `Invoke-RestMethod`, `DownloadString` | Retrieves or submits content. | Destination, response type, file writes, and follow-on execution. |
| `FromBase64String`, compression, XOR | Can unpack staged content. | Decode safely and hash extracted material. |
| Reflection, `Add-Type`, unmanaged API calls | May execute in memory or bypass normal tooling. | Script block, loaded modules, memory alerts, child activity. |
| AMSI or logging modification strings | May indicate defense evasion. | Registry/config changes, errors, and security product telemetry. |
| PowerShell from Office, browser, archive tool, or service | Unusual parent-child relationship. | User action, file origin, signer, and adjacent events. |

Execution policy is not a security boundary. `-ExecutionPolicy Bypass` is context, not proof of compromise.

## Triage workflow

1. Preserve the original command line and script block events.
2. Normalize aliases and decode layers without executing content.
3. Build the process tree and identify logon session and parent.
4. Correlate DNS, network, file, registry, task, service, and WMI activity.
5. Compare the host, user, script path, signer, and destination with baseline.
6. Search for the same script hash, command fragment, URL, or infrastructure fleet-wide.

## References

- [PowerShell logging](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_logging)
- [PowerShell security features](https://learn.microsoft.com/powershell/scripting/security/security-features)
- [MITRE ATT&CK T1059.001](https://attack.mitre.org/techniques/T1059/001/)

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2025-03-21 | Article added |
| 2026-07-22 | Added telemetry, caveats, signals, and triage workflow |
