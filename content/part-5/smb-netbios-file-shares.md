---
title: "SMB, NetBIOS, and File Shares"
date: 2026-07-22T15:00:00+02:00
draft: false
hidden: false
weight: 5
tags: [cheatsheet, smb, netbios, file-shares, windows, lateral-movement]
summary: "Hunting SMB, legacy NetBIOS traffic, administrative shares, remote execution, and file transfer."
---

__Author:__ _Roger C.B. Johnsen_

SMB is normal enterprise infrastructure and a common path for administration, software deployment, backup, lateral movement, and data staging. The useful question is rarely whether SMB occurred. Ask whether this identity, source, destination, share, operation, and follow-on activity belong together.

## Protocol map

| Transport | Purpose | Hunting context |
| --------- | ------- | --------------- |
| TCP/445 | Direct-hosted SMB | Primary transport for SMB 2 and SMB 3. |
| UDP/137 | NetBIOS Name Service | Legacy name registration and resolution; baseline broadcasts and responders. |
| UDP/138 | NetBIOS Datagram Service | Legacy connectionless messaging and browsing. |
| TCP/139 | NetBIOS Session Service | SMB over NetBIOS; uncommon in modern managed networks. |
| UDP/443 | SMB over QUIC | Encrypted remote SMB; identify approved servers and certificate-backed clients. |
| TCP/5445 | SMB Direct (RDMA) | High-performance storage traffic; expected only on designated infrastructure. |

Modern Windows can use configured alternative SMB ports. Protocol identification is therefore stronger than a port-only rule.

## Shares and operations

| Artifact | Meaning | Hunt for |
| -------- | ------- | -------- |
| `ADMIN$` | Windows directory used by remote administration | New source tools, payload writes, and subsequent service or task execution. |
| `C$`, `D$` | Administrative volume shares | Access by unexpected users, endpoints, or management tiers. |
| `IPC$` | Interprocess communication and named-pipe access | Authentication followed by service-control, SAMR, LSARPC, or other RPC activity. |
| `NETLOGON`, `SYSVOL` | Domain logon and policy content | Writes, unusual readers, or access outside expected domain workflows. |
| Custom hidden share ending in `$` | Hidden from casual browsing, not a security boundary | New share creation, weak ACLs, and sensitive staging locations. |
| Tree connect | Client connects to a share | First-seen client/share pairs and broad share enumeration. |
| Create/Open, Read, Write, SetInfo | Object access and modification | Executable writes, overwrite/rename chains, and bulk reads. |

Administrative-share access is not inherently malicious. Endpoint management, backup, vulnerability scanners, EDR, and help-desk tooling commonly use it.

## Windows telemetry

| Event ID | Meaning | Hunting use |
|---:|---|---|
| 4624 | Successful logon | Logon Type 3, authentication package, source address, account, and Logon ID. |
| 4625 | Failed logon | Password spraying, stale services, scanners, and repeated source-target pairs. |
| 4648 | Explicit credentials used | Remote administration and credential use; correlate target server and process. |
| 4672 | Special privileges assigned | Privileged network logons; correlate by Logon ID. |
| 4688 | Process created | `net.exe`, `net1.exe`, `sc.exe`, PowerShell, `wmic.exe`, and third-party tooling. |
| 4697 | Service installed | Remote service creation when Security System Extension auditing is enabled. |
| 5140 | Network share accessed | One event per client session and share; useful for source, account, and share. |
| 5142 | Network share created | Validate owner, path, ACL, and change record. |
| 5143 | Network share modified | Inspect permission and path changes. |
| 5144 | Network share deleted | Distinguish cleanup from authorized administration. |
| 5145 | Share object access checked | Detailed relative path and access request; high volume and policy dependent. |
| 7045 | Service installed | System-log corroboration for remote service execution. |

Also inspect `Microsoft-Windows-SMBClient/*`, `Microsoft-Windows-SMBServer/*`, firewall logs, domain-controller authentication, and file-server auditing. Events only exist when the relevant audit policy and SACL are configured.

Sysmon Event 1 supplies process ancestry, Events 3 and 22 add network and DNS context, Events 11/23/26 cover file activity, and Events 17/18 record named-pipe creation and connection when configured. Sysmon does not have an Event 853 for share creation or deletion.

## Named-pipe quick reference

Remote named pipes commonly traverse `IPC$` and carry RPC used by both Windows administration and attacker tooling.

| Pipe | Common purpose | Hunt for |
| ---- | -------------- | -------- |
| `lsarpc` | LSA policy, SID, and trust operations | Broad identity or policy enumeration. |
| `samr` | Account and group operations | Enumeration across many endpoints. |
| `srvsvc` | Server and share management | Share discovery from an unusual source. |
| `wkssvc` | Workstation information | Broad host and domain discovery. |
| `svcctl` | Service Control Manager | Service access after payload transfer or network logon. |
| `atsvc` | Legacy scheduled tasks | Remote task execution context. |
| `winreg` | Remote Registry | Remote configuration discovery or modification. |
| `spoolss` | Print Spooler RPC | Broad discovery, coercion, or spooler abuse context. |
| `PSEXESVC*`, `RemCom*` | Tool-associated remote execution | Validate service, process, payload, and operator. |

Pipe names are not detections. Correlate source, target, process, identity, breadth, and the surrounding SMB/service sequence. See [Named Pipes](../named-pipes/) for local and remote path forms, Sysmon and Defender telemetry, KQL examples, and triage.

## High-value hunting patterns

### Remote service execution

Look for a sequence rather than a tool name:

1. Network logon to the target.
2. Access to `ADMIN$`, a drive share, or `IPC$`.
3. Executable or script written to the target.
4. Service installation or control-manager named-pipe activity.
5. Process creation as `SYSTEM` or another privileged identity.
6. Service deletion and payload cleanup.

PsExec-like behavior can be produced by legitimate administration products and by many tools other than PsExec.

### Share discovery

Useful command-line observations include:

```powershell
net view \\server
net use
net share
Get-SmbShare
Get-SmbSession
Get-SmbConnection
```

Do not limit hunting to command lines. Software can call Windows networking and management APIs directly. Look for one host querying or connecting to many servers or shares, especially from an endpoint without an administrative role.

### Enumeration speed and breadth

Rapid browsing is a useful hunting signal even when every individual access is permitted. Human browsing tends to be comparatively narrow and irregular. Enumeration tools and scripts often produce bursts of metadata-only access across many servers, shares, directories, or filenames.

Useful features include:

- Distinct target devices, shares, and paths per source in a short time window.
- High operation count with few bytes read or written.
- Sequential traversal of directory trees or repeated access to share roots.
- Access to several business areas that the identity does not normally use.
- A workstation behaving like an inventory, backup, indexing, or management server.
- LDAP discovery immediately before broad SMB access.

Do not define *rapid* globally. Build separate baselines for people, jump hosts, deployment systems, backup, search indexers, data-loss prevention, vulnerability scanners, and file-management services. The valuable anomaly is machine-like breadth from an identity or device whose normal behavior is human-scale.

### AD Explorer and adjacent discovery

`ADExplorer.exe` is a signed Microsoft Sysinternals utility for browsing and editing Active Directory. It queries directory objects over LDAP; it is not, by itself, an SMB share browser. Its snapshot feature can capture a substantial view of the directory for offline analysis. That makes unexpected execution and rapid directory enumeration valuable precursors to hunt, especially because a snapshot can be processed later for attack-path analysis.

Hunt for:

- First-seen or rare execution of `ADExplorer.exe`, including renamed copies; verify signer, original filename, and hash.
- Execution from downloads, temporary directories, user profiles, or remote shares.
- An unusual parent process, privileged account, or execution on a user workstation.
- Broad LDAP queries to a domain controller and creation of an AD Explorer snapshot file.
- Snapshot compression, staging, or outbound transfer after collection.
- Subsequent connections from the same host or logon session to many file servers, admin shares, remote services, or privileged systems.

Legitimate directory administrators and identity teams may use AD Explorer. The strongest signal is the chain: unusual execution, broad LDAP collection, snapshot creation, staging, and follow-on lateral discovery.

### File transfer and staging

- First-seen source-to-file-server or client-to-share relationships.
- Executables, scripts, archives, or credential material written to administrative shares.
- Large or unusual reads from finance, HR, backup, source-code, or identity stores.
- Fan-out writes to several devices, followed by correlated execution.
- File creation over SMB followed by service, task, WMI, WinRM, or interactive execution.
- New hidden shares, permissive ACLs, or short-lived shares used for collection.

Volume alone is weak evidence. Compare direction, file type, sensitivity, identity, device role, time of day, and historical prevalence.

## Authentication, signing, and encryption

Kerberos is normally preferable to NTLM for domain SMB. Connections by IP address, alias/SPN problems, workgroup systems, and legacy products can cause NTLM fallback. Investigate new or unexpected NTLM use rather than treating every NTLM event as compromise.

SMB signing provides integrity and helps prevent relay. SMB encryption protects content in transit. They are separate controls. Windows 11 24H2 and Windows Server 2025 changed several defaults, including required SMB signing; newer platforms also support SMB NTLM blocking, authentication rate limiting, dialect controls, and improved auditing. Evaluate actual client/server policy and negotiated dialect instead of assuming fleet-wide defaults.

Relevant configuration locations include:

| Registry path | Context |
|---|---|
| `HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters` | SMB server configuration and signing requirements. |
| `HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters` | SMB client configuration and signing requirements. |
| `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\LmCompatibilityLevel` | NTLM compatibility policy; interpret with domain policy and observed authentication. |

## Defender XDR pivots

`DeviceFileEvents` exposes `RequestProtocol`, `RequestSourceIP`, `RequestAccountName`, and `ShareName` when available. This makes remote SMB file activity directly huntable.

```sql
DeviceFileEvents
| where Timestamp > ago(7d)
| where RequestProtocol =~ "SMB"
| where ShareName in~ ("ADMIN$", "C$", "D$")
| project Timestamp, DeviceName, ActionType, ShareName,
          FolderPath, FileName, SHA1, RequestSourceIP,
          RequestAccountDomain, RequestAccountName
| order by Timestamp desc
```

Find sources touching many devices or shares, then pivot to logons, process creation, network activity, and alerts:

```sql
DeviceFileEvents
| where Timestamp > ago(24h)
| where RequestProtocol =~ "SMB"
| summarize TargetDevices=dcount(DeviceId),
            Shares=dcount(ShareName), Files=count(),
            SampleShares=make_set(ShareName, 10)
    by RequestSourceIP, RequestAccountDomain, RequestAccountName
| where TargetDevices >= 5 or Shares >= 10
| order by TargetDevices desc
```

Thresholds are starting points, not detections. Tune them for deployment systems, file crawlers, backup, scanners, and administrator jump hosts.

The same data can expose short metadata-enumeration bursts:

```sql
DeviceFileEvents
| where Timestamp > ago(7d)
| where RequestProtocol =~ "SMB"
| summarize Events=count(),
            Targets=dcount(DeviceId),
            Shares=dcount(ShareName),
            Paths=dcount(strcat(ShareName, "\\", FolderPath))
    by bin(Timestamp, 5m), RequestSourceIP,
       RequestAccountDomain, RequestAccountName
| where Events >= 100 or Targets >= 5 or Shares >= 10 or Paths >= 50
| order by Timestamp desc
```

Treat this as a candidate generator. File servers can expose remote file operations in `DeviceFileEvents`, but coverage and field population depend on product deployment and activity type.

## Triage workflow

1. Identify the source device, process, identity, logon session, target, and share.
2. Determine authentication protocol, negotiated SMB dialect, signing, and encryption state.
3. Reconstruct file operations and preserve hashes, paths, timestamps, and sensitivity labels.
4. Correlate service, task, WMI, WinRM, RDP, named-pipe, and process telemetry.
5. Compare the relationship with peer hosts and historical activity.
6. Validate against management tooling, backup schedules, change records, and asset ownership.
7. Expand fleet-wide on source IP, account, file hash, share, command fragment, and named pipe.

## ATT&CK references

- [T1021.002: SMB/Windows Admin Shares](https://attack.mitre.org/techniques/T1021/002/)
- [T1135: Network Share Discovery](https://attack.mitre.org/techniques/T1135/)
- [T1570: Lateral Tool Transfer](https://attack.mitre.org/techniques/T1570/)
- [T1569.002: Service Execution](https://attack.mitre.org/techniques/T1569/002/)

## References

- [SMB features in Windows and Windows Server](https://learn.microsoft.com/windows-server/storage/file-server/smb-feature-descriptions)
- [SMB security hardening](https://learn.microsoft.com/windows-server/storage/file-server/smb-security-hardening)
- [Windows Event 5140](https://learn.microsoft.com/windows/security/threat-protection/auditing/event-5140)
- [Windows Event 5145](https://learn.microsoft.com/windows/security/threat-protection/auditing/event-5145)
- [Defender XDR DeviceFileEvents schema](https://learn.microsoft.com/defender-xdr/advanced-hunting-devicefileevents-table)
- [Sysinternals AD Explorer](https://learn.microsoft.com/sysinternals/downloads/adexplorer)

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2026-07-22 | Article added |
| 2026-07-22 | Added rapid enumeration and AD Explorer hunting patterns |
