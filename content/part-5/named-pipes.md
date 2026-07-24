---
title: "Named Pipes"
date: 2026-07-22T16:10:00+02:00
draft: false
hidden: false
weight: 6
tags: [cheatsheet, named-pipes, windows, smb, rpc, lateral-movement]
summary: "Local and remote named-pipe telemetry for discovery, RPC, administration, and lateral movement."
---

__Author:__ _Roger C.B. Johnsen_

Named pipes are Windows interprocess communication endpoints. They support normal operating-system, application, RPC, and management activity, but also appear in discovery, credential access, lateral movement, and command-and-control workflows. A pipe name is context, not a verdict.

## Path context

| Pattern | Meaning | Hunting value |
| ------- | ------- | ------------- |
| `\Device\NamedPipe\<name>` | Local named pipe | Correlate creator, connector, process ancestry, user, and prevalence. |
| `\\.\pipe\<name>` | Win32 local pipe notation | Often maps to the local NamedPipe device path. |
| `\\SERVER\IPC$\<pipe>` | Remote pipe over SMB | Indicates RPC or pipe access through the remote IPC share. |
| `\Device\Mup\SERVER\IPC$\<pipe>` | Kernel path through Multiple UNC Provider | Strong evidence of remote UNC/SMB context. |

## Pipes worth recognising

| Pipe or family | Common role | Hunt for |
| -------------- | ----------- | -------- |
| `lsarpc` | LSA policy, SID, and trust operations | Broad identity or policy enumeration from an unusual host. |
| `samr` | SAM account and group operations | Account enumeration across many endpoints. |
| `srvsvc` | Server and share management | Share or server discovery and remote administration. |
| `wkssvc` | Workstation service RPC | Host and domain information gathering. |
| `netlogon` | Netlogon RPC | Unexpected client, process, or target outside normal domain activity. |
| `svcctl` | Service Control Manager RPC | Service query/control following SMB authentication or payload transfer. |
| `atsvc` | Legacy scheduled-task RPC | Remote task creation or execution context. |
| `winreg` | Remote Registry | Remote configuration discovery or modification. |
| `spoolss` | Print Spooler RPC | Widespread printer discovery, coercion, or spooler abuse context. |
| `PSEXESVC*` | PsExec-associated service pipe | Validate tool deployment, service creation, payload, and operator. |
| `RemCom*` | RemCom and related remote execution | Validate service and process activity on both endpoints. |
| Random or campaign-specific names | Application, malware, or C2 IPC | Rarity, entropy, creator/connector pair, and recurrence. |

The presence of `samr`, `lsarpc`, or `svcctl` does not prove malicious use. Windows components and administration tools use the same RPC interfaces.

## High-value patterns

- One workstation accessing the same RPC pipe across many servers.
- Many identity-related pipes against a domain controller in a short window.
- `ADMIN$` file transfer followed by `svcctl`, service installation, and SYSTEM process creation.
- Pipe activity immediately after an unusual network logon.
- A rare local pipe created by an unsigned process in a user-writable path.
- A common pipe name used by an unexpected process or account.
- Snapshot or directory enumeration followed by `samr`, `srvsvc`, and broad SMB access.
- Short-lived service and pipe activity followed by payload and service deletion.

Speed and breadth are useful features. Build separate baselines for endpoint management, backup, monitoring, vulnerability scanning, identity administration, and software deployment.

## Sysmon telemetry

| Event ID | Meaning | Key fields |
| -------: | ------- | ---------- |
| 17 | Pipe created | `PipeName`, `Image`, `ProcessGuid`, `ProcessId`, `User` |
| 18 | Pipe connected | `PipeName`, `Image`, `ProcessGuid`, `ProcessId`, `User` |

Sysmon coverage depends on configuration. Event 17 and 18 identify local process participation; enrich with process creation, logon, SMB, service, and network telemetry before inferring a remote action.

## Defender XDR telemetry

Some tenants expose named-pipe activity through `DeviceEvents` with `ActionType == "NamedPipeEvent"` and pipe-specific values in `AdditionalFields`. The shape and availability can vary, so inspect the schema and sample records first:

```sql
DeviceEvents
| where Timestamp > ago(24h)
| where ActionType == "NamedPipeEvent"
| take 20
| project Timestamp, DeviceId, DeviceName,
          InitiatingProcessFileName,
          InitiatingProcessCommandLine,
          InitiatingProcessAccountUpn,
          AdditionalFields
```

After confirming the local field names:

```sql
let TargetName = "TARGET-DEVICE.contoso.invalid";
DeviceEvents
| where Timestamp > ago(7d)
| where DeviceName =~ TargetName
| where ActionType == "NamedPipeEvent"
| extend Fields=todynamic(AdditionalFields)
| extend PipeName=tostring(Fields.PipeName),
         FileOperation=tostring(Fields.FileOperation),
         NamedPipeEnd=tostring(Fields.NamedPipeEnd)
| project Timestamp, DeviceName, PipeName,
          FileOperation, NamedPipeEnd,
          InitiatingProcessFileName,
          InitiatingProcessCommandLine,
          InitiatingProcessAccountUpn
| order by Timestamp asc
```

Filter remote MUP paths only after verifying how the tenant formats `PipeName`:

```sql
DeviceEvents
| where Timestamp > ago(7d)
| where ActionType == "NamedPipeEvent"
| extend Fields=todynamic(AdditionalFields)
| extend PipeName=tostring(Fields.PipeName)
| where PipeName startswith @"\Device\Mup\"
| summarize Events=count(),
            Pipes=dcount(PipeName),
            SamplePipes=make_set(PipeName, 10)
    by DeviceId, DeviceName,
       InitiatingProcessFileName,
       InitiatingProcessAccountUpn,
       bin(Timestamp, 5m)
| order by Events desc
```

## Remote execution correlation

A useful sequence is:

```text
network logon
  -> IPC$ or ADMIN$ access
  -> payload transfer
  -> svcctl or tool-specific pipe
  -> service installation
  -> child process on target
  -> service and payload cleanup
```

Correlate Security Events 4624, 4648, 4697, 5140, and 5145; System Event 7045; Sysmon Events 1, 3, 11, 17, and 18; and Defender device, file, network, and logon tables. Not every step is present in every telemetry source.

## Triage workflow

1. Normalise the pipe path and decide whether it is local or remote.
2. Identify creator and connector processes using stable process identifiers.
3. Resolve account, integrity, logon session, source, and target.
4. Check pipe prevalence by process, device role, and peer group.
5. Correlate nearby SMB, RPC, service, task, file, and process activity.
6. Determine whether the behaviour is interactive, automated, or fleet-wide.
7. Validate management tooling and change records.
8. Expand on pipe name, process hash, source account, and target set.

## ATT&CK references

- [T1559: Inter-Process Communication](https://attack.mitre.org/techniques/T1559/)
- [T1021.002: SMB/Windows Admin Shares](https://attack.mitre.org/techniques/T1021/002/)
- [T1569.002: Service Execution](https://attack.mitre.org/techniques/T1569/002/)
- [T1135: Network Share Discovery](https://attack.mitre.org/techniques/T1135/)

## References

- [Sysmon documentation](https://learn.microsoft.com/sysinternals/downloads/sysmon)
- [Defender XDR DeviceEvents schema](https://learn.microsoft.com/defender-xdr/advanced-hunting-deviceevents-table)

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2026-07-22 | Article added |
