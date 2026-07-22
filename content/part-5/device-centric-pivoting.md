---
title: "Device-centric Pivoting in Defender XDR"
date: 2026-07-22T16:00:00+02:00
draft: false
hidden: false
weight: 2
tags: [cheatsheet, kusto, defender-xdr, pivoting, investigation]
summary: "A repeatable workflow for expanding one device into network, process, file, identity, and alert evidence."
---

__Author:__ _Roger C.B. Johnsen_

A device is often the first reliable anchor in an investigation. The objective is not to run one large query. Preserve the identity of the device over time, establish the relevant window, and pivot through independent evidence until the activity can be explained.

{{% notice warning %}}
The examples use neutral placeholders and starting thresholds. Table availability, retention, columns, and `ActionType` values depend on the products deployed in your tenant. Confirm every query against the in-portal schema.
{{% /notice %}}

## Pivot map

| Starting point | Next table | Stable pivot | Question |
| -------------- | ---------- | ------------ | -------- |
| Device name | `DeviceInfo` | `DeviceId` | Which Defender entity represents the host? |
| Device | `DeviceNetworkInfo` | `DeviceId` | Which addresses and networks belonged to it? |
| Device or IP | `DeviceNetworkEvents` | `DeviceId`, IP, time | What communicated with it, and what did it contact? |
| Device | `DeviceProcessEvents` | `ProcessUniqueId` | Which process tree explains the behavior? |
| Process | `DeviceNetworkEvents` | initiating process unique ID | Which destinations did that process use? |
| Device | `DeviceFileEvents` | `DeviceId`, `SHA1` | What was created, staged, or transferred? |
| Device and account | `DeviceLogonEvents` | SID, Logon ID | Which identity and session were involved? |
| Device | `DeviceEvents` | `DeviceId`, `ActionType` | Which security-control or specialized events add context? |
| Alert | `AlertEvidence` | `AlertId` | Which entities and evidence support the alert? |

Prefer `DeviceId` over a mutable hostname, process unique identifiers over reused PIDs, and SID or object ID over display names.

## Establish the device

Use the latest device snapshot to resolve an exact identifier:

```sql
let TargetName = "TARGET-DEVICE.contoso.invalid";
DeviceInfo
| where DeviceName =~ TargetName
| summarize arg_max(Timestamp, *) by DeviceId
| project Timestamp, DeviceId, DeviceName, OSPlatform,
          OSVersion, MachineGroup, DeviceType, OnboardingStatus
```

A partial `contains` match can silently combine several devices. Use it for discovery, then switch to an exact `DeviceId`.

## Reconstruct IP history

Microsoft provides `AssignedIPAddresses()` for recent or point-in-time assignments:

```sql
let TargetName = "TARGET-DEVICE.contoso.invalid";
AssignedIPAddresses(TargetName)
| where IPAddress !startswith "127."
| where IPAddress !startswith "169.254."
| project Timestamp, DeviceName, IPAddress, IPType,
          NetworkAdapterType, ConnectedNetworks
```

For a longer history, expand the adapter snapshots:

```sql
let TargetName = "TARGET-DEVICE.contoso.invalid";
DeviceNetworkInfo
| where Timestamp > ago(30d)
| where DeviceName =~ TargetName
| mv-expand Address = todynamic(IPAddresses)
| extend IPAddress=tostring(Address.IPAddress),
         AddressType=tostring(Address.AddressType)
| where isnotempty(IPAddress)
| where IPAddress !startswith "127."
| where IPAddress !startswith "169.254."
| where NetworkAdapterType !~ "Tunnel"
| summarize FirstSeen=min(Timestamp), LastSeen=max(Timestamp),
            Records=count()
    by DeviceId, DeviceName, IPAddress, AddressType,
       NetworkAdapterName, NetworkAdapterType, MacAddress
| order by FirstSeen asc
```

Snapshots show observation intervals, not authoritative DHCP lease boundaries. Add a safety margin and validate with DHCP, VPN, NAC, firewall, or identity telemetry when attribution matters.

## Examine both network perspectives

A known address can appear as either `LocalIP` or `RemoteIP` depending on which onboarded device reported the event.

### Activity reported by the target

```sql
let TargetId = "00000000-0000-0000-0000-000000000000";
DeviceNetworkEvents
| where Timestamp > ago(7d)
| where DeviceId == TargetId
| project Timestamp, DeviceName, ActionType,
          LocalIP, LocalPort, RemoteIP, RemotePort, RemoteUrl,
          Protocol, InitiatingProcessFileName,
          InitiatingProcessCommandLine, InitiatingProcessUniqueId
| order by Timestamp asc
```

### Other devices reporting the target as their peer

```sql
let TargetName = "TARGET-DEVICE.contoso.invalid";
let TargetIPs =
    AssignedIPAddresses(TargetName)
    | project TargetIP=IPAddress, AssignedTime=Timestamp;
TargetIPs
| join kind=inner (
    DeviceNetworkEvents
    | where Timestamp > ago(7d)
) on $left.TargetIP == $right.RemoteIP
| where Timestamp between (AssignedTime - 1h .. AssignedTime + 1h)
| project Timestamp, ReportingDevice=DeviceName,
          LocalIP, LocalPort, TargetIP, RemotePort,
          ActionType, InitiatingProcessFileName
```

Local and remote fields describe the reporting device's record; they do not always prove who initiated a session. Use `ActionType`, listening versus ephemeral ports, process ownership, packet or firewall data, and the peer's telemetry. A successful TCP connection does not prove successful authentication.

## Build the process story

```sql
let TargetId = "00000000-0000-0000-0000-000000000000";
let StartTime = datetime(2026-01-15T09:00:00Z);
let EndTime = StartTime + 2h;
DeviceProcessEvents
| where Timestamp between (StartTime .. EndTime)
| where DeviceId == TargetId
| project Timestamp, DeviceName, AccountSid, AccountUpn,
          ProcessUniqueId, InitiatingProcessUniqueId,
          FileName, FolderPath, ProcessCommandLine,
          InitiatingProcessFileName, InitiatingProcessCommandLine,
          SHA1, ProcessVersionInfoOriginalFileName
| order by Timestamp asc
```

Do not subtract every process seen on one reference device. A peer baseline is useful for ranking, but exact command-line subtraction is brittle and broad allowlists can hide living-off-the-land activity. Compare prevalence, path, signer, original filename, account, integrity, parentage, and host role.

## Correlate a process with network activity

```sql
let StartTime = ago(24h);
DeviceProcessEvents
| where Timestamp >= StartTime
| where DeviceName =~ "TARGET-DEVICE.contoso.invalid"
| project ProcessTime=Timestamp, DeviceId, DeviceName,
          ProcessUniqueId, FileName, ProcessCommandLine, AccountUpn
| join kind=leftouter (
    DeviceNetworkEvents
    | where Timestamp >= StartTime
    | project NetworkTime=Timestamp, DeviceId,
              InitiatingProcessUniqueId, RemoteIP,
              RemotePort, RemoteUrl, ActionType
) on DeviceId
| where ProcessUniqueId == InitiatingProcessUniqueId
| where NetworkTime between (ProcessTime .. ProcessTime + 30m)
| project ProcessTime, NetworkTime, DeviceName, AccountUpn,
          FileName, ProcessCommandLine, RemoteIP,
          RemotePort, RemoteUrl, ActionType
```

## Expand into files, identities, and alerts

Use the same `DeviceId`, time window, account SID, logon session, process identifier, URL, IP, or hash to ask focused questions:

- Did the process create or delete a file?
- Did a remote SMB or NFS request create the file?
- Which logon session supplied the identity?
- Was the account new to the device?
- Did the file retain origin URL or referrer context?
- Did an alert associate the device with another entity?
- Did activity continue on a different device?

Keep joins narrow. Filter time, project only required columns, and summarize snapshot tables before joining.

## Triage sequence

1. Resolve the exact device and investigation window.
2. Reconstruct addresses and connected networks for that window.
3. Examine both network perspectives.
4. Build process ancestry with stable process identifiers.
5. Pivot to file, logon, registry, named-pipe, and alert evidence.
6. Compare with the device role, peers, user history, and change records.
7. Record telemetry gaps and alternative explanations.
8. Expand fleet-wide only after identifying stable behaviors or pivots.

## References

- [AssignedIPAddresses function](https://learn.microsoft.com/defender-xdr/advanced-hunting-assignedipaddresses-function)
- [DeviceNetworkInfo schema](https://learn.microsoft.com/defender-xdr/advanced-hunting-devicenetworkinfo-table)
- [Defender XDR advanced hunting schema](https://learn.microsoft.com/defender-xdr/advanced-hunting-schema-tables)

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2026-07-22 | Article added |
