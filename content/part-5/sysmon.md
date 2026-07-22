---
title: "Sysmon Hunting"
date: 2025-03-21T20:10:23+01:00
draft: false
hidden: false
weight: 8
tags: [cheatsheet, sysmon, events, windows, microsoft]
summary: "Sysmon event IDs, correlation fields, and high-value hunting questions."
---

__Author:__ _Roger C.B. Johnsen_

Sysmon produces telemetry, not detections. Event presence depends on version and configuration, and high-volume event types are often filtered. First confirm collection health and the active configuration.

Log channel: `Microsoft-Windows-Sysmon/Operational`.

## Event reference

| ID | Event | Hunt focus |
|---:|---|---|
| 1 | ProcessCreate | Parent-child anomalies, command line, signer, hashes, integrity, logon session. |
| 2 | FileCreateTime | Timestomping; compare other timestamps and file origin. |
| 3 | NetworkConnect | Process-owned connection, destination rarity, port/protocol mismatch. |
| 4 | Sysmon service state | Unexpected stop/start and collection gaps. |
| 5 | ProcessTerminate | Timeline closure and short-lived processes. |
| 6 | DriverLoad | Unsigned, revoked, vulnerable, or unusual driver paths. |
| 7 | ImageLoad | DLL search-order hijacking and unusual unsigned modules; high volume. |
| 8 | CreateRemoteThread | Cross-process execution; validate source, target, and start module. |
| 9 | RawAccessRead | Direct disk access by non-backup/non-security tools. |
| 10 | ProcessAccess | Credential access and injection; inspect access mask and target. |
| 11 | FileCreate | Payloads, staging, startup folders, and suspicious extensions. |
| 12-14 | RegistryEvent | Persistence and security-setting changes; correlate process GUID. |
| 15 | FileCreateStreamHash | Alternate data streams and downloaded content. |
| 16 | Sysmon config change | Unplanned configuration changes; cannot be filtered. |
| 17-18 | PipeEvent | Rare named pipes, remote tooling, and process context. |
| 19-21 | WmiEvent | Permanent WMI subscription filter, consumer, and binding. |
| 22 | DnsQuery | Process-attributed queries, rare domains, and pre-connection context. |
| 23 | FileDelete | Deleted file archived; monitor archive capacity and access. |
| 24 | ClipboardChange | Clipboard content hash/context; privacy and volume matter. |
| 25 | ProcessTampering | Image manipulation such as process hollowing. |
| 26 | FileDeleteDetected | Deletion recorded without archiving the file. |
| 27 | FileBlockExecutable | Executable creation blocked by configured rules. |
| 28 | FileBlockShredding | File shredding blocked by configured rules. |
| 29 | FileExecutableDetected | New PE file detected; availability depends on supported Windows version. |
| 255 | Error | Data loss, load, configuration, or integrity problem requiring triage. |

## Correlation fields

- `ProcessGuid` and `ParentProcessGuid` are preferable to reused PIDs.
- `LogonGuid` links activity to a logon session.
- `RuleName` can carry configuration-defined context such as ATT&CK tags.
- `Image`, `User`, hashes, signature fields, and command line explain the event.
- UTC timestamps simplify cross-source correlation.

## Fast triage

1. Verify the event type was enabled and forwarded during the period.
2. Build the process ancestry using GUIDs.
3. Correlate file, registry, DNS, and network events around the same process.
4. Validate signer and hash, but do not equate signed with safe.
5. Compare the behavior with the host role and fleet prevalence.
6. Check Events 4, 16, and 255 when telemetry disappears.

Do not map each event ID permanently to one ATT&CK technique. The same event can support many behaviors, and the mapping depends on fields and context.

## References

- [Sysmon documentation](https://learn.microsoft.com/sysinternals/downloads/sysmon)
- [Sysmon configuration schema](https://learn.microsoft.com/sysinternals/downloads/sysmon#configuration-files)

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2025-03-21 | Article added |
| 2026-07-22 | Corrected event semantics and added hunting and coverage guidance |
