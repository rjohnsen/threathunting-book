---
title: "File Staging and User-writable Paths"
date: 2026-07-22T16:00:00+02:00
draft: false
hidden: false
weight: 10
tags: [cheatsheet, files, staging, windows, defender-xdr]
summary: "Hunting file origin, staging, extraction, execution, and cleanup in user-writable locations."
---

__Author:__ _Roger C.B. Johnsen_

User-writable locations are normal working areas and convenient attacker staging locations. A path does not establish intent. Hunt the sequence that produced the file, the process that touched it, what happened next, and whether the behavior fits the user and device.

## Locations worth monitoring

| Location | Normal use | Hunting context |
| -------- | ---------- | --------------- |
| `Downloads` | Browser and application downloads | Executables, scripts, disk images, shortcuts, archives, and rapid execution. |
| `Desktop` | User-created and copied files | Operator tools, command output, archives, and temporary staging. |
| `Documents` | User documents and application data | Collection, bulk copying, and archives inconsistent with the user role. |
| `%TEMP%` | Installers and temporary application content | Short-lived payloads, extraction, compilation, and execution. |
| `AppData\Local` | Per-user application state and caches | Payloads, persistence, side-loading, and browser or collaboration-app artifacts. |
| `AppData\Roaming` | Roaming application state | Logon persistence, scripts, and configuration. |
| OneDrive or other sync roots | Cloud-synchronized work | External delivery, staging, cross-device propagation, and bulk upload. |
| `Public` and shared profile paths | Cross-user content | Shared staging and execution by another account. |
| Recycle Bin | User deletion | Cleanup context; deletion does not remove all evidence. |

Do not exclude `AppData` globally. Reduce noise with process, extension, path depth, prevalence, signer, and sequence.

## Useful file context

| Field or artifact | Question |
| ----------------- | -------- |
| `ActionType` | Was the file created, modified, renamed, or deleted? |
| `FolderPath`, `FileName` | Is the location and name consistent with the producing process? |
| `SHA1` | Where else did the same content appear? |
| `FileOriginUrl` | Which URL supplied the file when origin telemetry is available? |
| `FileOriginReferrerUrl` | Which page or application led to the download? |
| `FileOriginIP` | Which remote address supplied the content? |
| Initiating process fields | Which process performed the file operation? |
| `RequestProtocol`, `RequestSourceIP`, `ShareName` | Was it written remotely over SMB or NFS? |
| `Zone.Identifier` | Did Windows preserve Mark of the Web information? |
| Signer and original filename | Is a familiar filename actually the expected binary? |
| Sensitivity label | Does staging involve protected information? |

Hash fields can be empty when files are remote, locked, compressed, or virtual. In Defender endpoint tables, `SHA1` is commonly more populated than `SHA256`.

## High-value sequences

| Sequence | Possible explanation | Validate |
| -------- | -------------------- | -------- |
| Create -> execute | Downloaded or staged payload | Origin, signer, parent, user action, and reputation. |
| Archive -> extraction -> execute | Tool or payload staging | Archive source, extracted set, and execution chain. |
| Rename -> execute | Unpacking or masquerading | Original name, signer, extension, and producer. |
| Create -> execute -> delete | Temporary execution and cleanup | Process, network, prefetch, service/task, and hash prevalence. |
| Documents -> archive | Data collection or legitimate packaging | File sensitivity, volume, account role, destination. |
| Remote SMB write -> execute | Lateral tool transfer | Source host, share, logon, service/task/WMI activity. |
| Browser/Office/Teams -> script host | User-assisted execution | Mark of the Web, child process, URL, and message context. |
| Many reads -> one archive -> outbound transfer | Collection and staging | Bytes, destination, cloud upload, and DLP telemetry. |

These are hypotheses, not severity labels.

## Baseline query

```sql
let TargetName = "TARGET-DEVICE.contoso.invalid";
DeviceFileEvents
| where Timestamp > ago(7d)
| where DeviceName =~ TargetName
| where FolderPath has @"\Users\"
| where FolderPath has_any (
    @"\Downloads\", @"\Desktop\", @"\Documents\",
    @"\AppData\Local\Temp\", @"\AppData\Roaming\"
)
| project Timestamp, DeviceId, DeviceName, ActionType,
          FolderPath, FileName, FileSize, SHA1,
          FileOriginUrl, FileOriginReferrerUrl, FileOriginIP,
          RequestProtocol, RequestSourceIP, ShareName,
          InitiatingProcessFileName,
          InitiatingProcessCommandLine,
          InitiatingProcessAccountUpn,
          InitiatingProcessUniqueId
| order by Timestamp asc
```

Path matching should be adapted for localized or redirected profiles and synchronized folders.

## File to process pivot

```sql
let TargetHash = "SHA1-HERE";
let FileActivity =
    DeviceFileEvents
    | where Timestamp > ago(30d)
    | where SHA1 == TargetHash
    | project FileTime=Timestamp, DeviceId, DeviceName,
              FolderPath, FileName, SHA1,
              InitiatingProcessFileName,
              InitiatingProcessUniqueId;
FileActivity
| join kind=leftouter (
    DeviceProcessEvents
    | where Timestamp > ago(30d)
    | project ProcessTime=Timestamp, DeviceId,
              ProcessUniqueId, FileName,
              FolderPath, ProcessCommandLine,
              AccountUpn, SHA1
) on DeviceId, SHA1
| where ProcessTime between (FileTime - 5m .. FileTime + 1h)
| order by FileTime asc
```

A file event does not prove execution. Use `DeviceProcessEvents`, Prefetch where available, service/task telemetry, or other execution evidence.

## Remote file activity

```sql
DeviceFileEvents
| where Timestamp > ago(7d)
| where RequestProtocol in~ ("SMB", "NFS")
| project Timestamp, DeviceName, ActionType,
          ShareName, FolderPath, FileName, SHA1,
          RequestProtocol, RequestSourceIP,
          RequestAccountDomain, RequestAccountName
| order by Timestamp desc
```

Correlate remote writes with logon, network, named-pipe, service, task, WMI, WinRM, and process activity on the target.

## Burst and staging candidates

```sql
DeviceFileEvents
| where Timestamp > ago(7d)
| where FolderPath has @"\Users\"
| summarize Events=count(),
            Files=dcount(strcat(FolderPath, @"\", FileName)),
            Bytes=sum(FileSize),
            Archives=countif(FileName matches regex @"(?i)\.(zip|7z|rar|cab)$"),
            Executables=countif(FileName matches regex @"(?i)\.(exe|dll|scr|com)$"),
            Scripts=countif(FileName matches regex @"(?i)\.(ps1|bat|cmd|vbs|js|hta)$")
    by DeviceId, DeviceName,
       InitiatingProcessFileName,
       InitiatingProcessAccountUpn,
       bin(Timestamp, 15m)
| where Files >= 50 or Archives > 0 or Executables > 0 or Scripts > 0
| order by Timestamp desc
```

Tune this candidate generator for software deployment, development, build tools, browsers, synchronization clients, archive utilities, and endpoint management.

## Triage workflow

1. Preserve path, timestamps, size, hash, origin, and initiating process.
2. Determine whether the file arrived locally, through a browser, sync client, email, SMB, NFS, or removable media.
3. Inspect archive contents and scripts safely without execution.
4. Reconstruct create, rename, extract, execute, network, and delete events.
5. Identify the user and logon session that produced and executed the content.
6. Compare signer, original filename, prevalence, and peer behavior.
7. Search the hash, origin URL, filename, and infrastructure fleet-wide.
8. Record whether telemetry proves presence, transfer, execution, or only access.

## ATT&CK references

- [T1105: Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/)
- [T1074.001: Local Data Staging](https://attack.mitre.org/techniques/T1074/001/)
- [T1570: Lateral Tool Transfer](https://attack.mitre.org/techniques/T1570/)
- [T1070.004: File Deletion](https://attack.mitre.org/techniques/T1070/004/)

## References

- [Defender XDR DeviceFileEvents schema](https://learn.microsoft.com/defender-xdr/advanced-hunting-devicefileevents-table)
- [Defender XDR DeviceProcessEvents schema](https://learn.microsoft.com/defender-xdr/advanced-hunting-deviceprocessevents-table)

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2026-07-22 | Article added |
