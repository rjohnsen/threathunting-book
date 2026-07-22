---
title: "Microsoft Defender XDR Hunting Tables"
date: 2025-03-21T20:10:35+01:00
draft: false
hidden: false
weight: 1
tags: [cheatsheet, kusto, tables, microsoft]
summary: "A practical map of Defender XDR advanced hunting tables and pivot keys."
---

__Author:__ _Roger C.B. Johnsen_

Microsoft Defender XDR advanced hunting and Microsoft Sentinel are related but not interchangeable schemas. The `Device*`, `Email*`, `Identity*`, and `CloudAppEvents` tables below are Defender XDR schema tables; Sentinel also exposes connector-specific tables such as `SecurityEvent`, `SigninLogs`, and `CommonSecurityLog`.

Table availability depends on deployed products, licensing, retention, and telemetry health. Confirm the in-portal schema before assuming a column or `ActionType` exists.

{{% notice warning %}}
This cheat sheet covers the core Defender XDR advanced hunting tables used in common investigations. It is not a complete table catalogue. The available schema depends on the Microsoft security products deployed in your tenant and continues to change as new data sources and preview features are introduced. Use the [Microsoft Defender XDR advanced hunting schema reference](https://learn.microsoft.com/defender-xdr/advanced-hunting-schema-tables) for the complete and current list of tables, columns, and supported `ActionType` values.
{{% /notice %}}

## Core tables

| Table | Contains | Useful pivots |
| ------------ | -------- | ------------- |
| `DeviceProcessEvents` | Process creation and related events | `DeviceId`, `ProcessUniqueId`, `InitiatingProcessUniqueId`, `SHA1` |
| `DeviceNetworkEvents` | Network connections and related events | `DeviceId`, process IDs, `RemoteIP`, `RemoteUrl` |
| `DeviceFileEvents` | File creation, modification, rename, and deletion | `DeviceId`, `SHA1`, `FileName`, initiating process |
| `DeviceRegistryEvents` | Registry creation and modification | `DeviceId`, key/value, initiating process |
| `DeviceLogonEvents` | Device authentication activity | `DeviceId`, account SID/name, remote IP, logon ID |
| `DeviceEvents` | Security-control and miscellaneous device events | `ActionType`, `DeviceId`, `AdditionalFields` |
| `DeviceImageLoadEvents` | DLL and image loads | `DeviceId`, `SHA1`, initiating process |
| `DeviceInfo` | Device metadata and posture snapshots | `DeviceId`, `DeviceName`, tags, exposure |
| `DeviceNetworkInfo` | Interfaces, addresses, networks, domains | `DeviceId`, IP and MAC address |
| `EmailEvents` | Mail flow and delivery metadata | `NetworkMessageId`, sender, recipient |
| `EmailAttachmentInfo` | Attachment metadata | `NetworkMessageId`, `SHA1`, filename |
| `EmailUrlInfo` | URLs found in email | `NetworkMessageId`, URL |
| `UrlClickEvents` | Safe Links clicks in supported workloads | URL, account, `NetworkMessageId` |
| `CloudAppEvents` | Cloud application and governance activity | account/object IDs, application, IP |
| `IdentityLogonEvents` | AD and Microsoft online authentication | account SID/UPN, IP, device |
| `IdentityDirectoryEvents` | On-premises directory and DC activity | account SID, device, `ActionType` |
| `IdentityQueryEvents` | Queries for AD objects | querying identity/device and queried target |
| `IdentityInfo` | Identity context from available sources | account SID, object ID, UPN |
| `AlertInfo` | Alert metadata | `AlertId` |
| `AlertEvidence` | Entities associated with alerts | `AlertId`, entity identifiers |

Use `SHA1` when hunting Defender endpoint tables unless the schema confirms `SHA256` is populated; several tables document SHA-256 as commonly empty.

## Common pivot patterns

| Start with | Pivot through | Use it to answer |
| ---------- | ------------- | ---------------- |
| Device | `DeviceInfo` -> `DeviceNetworkInfo` -> `DeviceNetworkEvents` | Which identities and network peers belonged to the device over time? |
| Process | `ProcessUniqueId` -> initiating process fields | Which network, file, registry, and child-process activity belongs to it? |
| IP address | assignment window -> local and remote network perspectives | Which device owned the address, and which peers communicated with it? |
| File | `SHA1`, origin fields, initiating process | Where did it arrive, where else did it appear, and was it executed? |
| Account | SID/object ID -> logon and device events | Which sessions, devices, and resources did the identity use? |
| Alert | `AlertId` -> `AlertEvidence` -> native event tables | Which underlying telemetry supports the alert? |
| SMB or pipe | source IP, account, share, pipe, target | Was it discovery, administration, transfer, or remote execution? |

For complete workflows, see [Device-centric Pivoting in Defender XDR](../device-centric-pivoting/), [Named Pipes](../named-pipes/), and [File Staging and User-writable Paths](../file-staging-user-writable-paths/).

## Reliable pivot habits

1. Filter time and reduce columns before joins.
2. Prefer stable service identifiers such as `DeviceId`, account SID/object ID, `AlertId`, and `NetworkMessageId`.
3. Use process unique identifiers where available; PIDs are reused.
4. Normalize case and identity format before joining names.
5. Use `arg_max(Timestamp, *) by DeviceId` for the latest snapshot, not an unconstrained join to `DeviceInfo`.
6. Use `leftouter` when enrichment may be absent and `innerunique` only when its left-side deduplication is intended.
7. Treat `AdditionalFields` as dynamic data and parse only the keys needed.

## Query starters

### Process to network activity

```sql
let start = ago(24h);
DeviceProcessEvents
| where Timestamp >= start
| where FileName =~ "powershell.exe" or FileName =~ "pwsh.exe"
| project Timestamp, DeviceId, DeviceName, ProcessUniqueId,
          FileName, ProcessCommandLine, AccountName
| join kind=leftouter (
    DeviceNetworkEvents
    | where Timestamp >= start
    | project NetworkTime=Timestamp, DeviceId,
              InitiatingProcessUniqueId, RemoteUrl, RemoteIP, RemotePort
) on DeviceId
| where InitiatingProcessUniqueId == ProcessUniqueId
| where NetworkTime between (Timestamp .. Timestamp + 10m)
```

### Alert to evidence

```sql
AlertInfo
| where Timestamp > ago(7d)
| project AlertId, AlertTime=Timestamp, Title, Severity
| join kind=inner (AlertEvidence | where Timestamp > ago(7d)) on AlertId
| project AlertTime, Title, Severity, EntityType,
          DeviceName, AccountName, RemoteIP, FileName, SHA1
```

## References

- [Defender XDR advanced hunting schema](https://learn.microsoft.com/defender-xdr/advanced-hunting-schema-tables)
- [Kusto join operator](https://learn.microsoft.com/kusto/query/join-operator)

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2025-03-21 | Article added |
| 2026-07-22 | Corrected scope, tables, pivots, and query patterns |
