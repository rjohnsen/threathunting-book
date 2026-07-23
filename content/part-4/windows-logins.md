+++
title = "Windows Logons"
date = 2024-10-06T13:28:41+02:00
lastmod = 2026-07-23T00:00:00+02:00
weight = 40
chapter = false
+++

{{% notice style="info" title="Illustrative queries" %}}
Queries on this page are illustrative starting points. They demonstrate investigation logic, not production-ready detections. Table availability, field names, action types, parsing, retention, and normal behaviour vary by environment. Inspect raw records and validate every query against your local schema and telemetry before relying on the result.
{{% /notice %}}

Windows logon events are evidence of authentication and session creation. They are not proof that a person was physically present, that the activity was interactive, or that the source address identifies the initiating endpoint. Services, scheduled tasks, remote administration, cached credentials, and authentication intermediaries all leave different shapes.

{{% notice style="info" title="Start with where the event was written" %}}
Event 4624 is normally recorded on the computer where the logon session was created. Event 4625 is normally recorded on the computer where the failed logon attempt was made. Domain-controller events such as 4768 and 4769 describe Kerberos activity from the domain's viewpoint. Correlate these viewpoints instead of treating one event as the complete story.
{{% /notice %}}

## Core events

| Event ID | Meaning | Investigation value |
| --- | --- | --- |
| 4624 | An account was successfully logged on | Session type, target identity, authentication package, source details, and logon identifier |
| 4625 | An account failed to log on | Status, substatus, attempted identity, source details, process, and logon type |
| 4634 | A logon session ended | Session cleanup; may be missing after crashes or collection gaps |
| 4647 | A user initiated logoff | Stronger indication of an intentional user logoff than 4634 |
| 4648 | A logon was attempted using explicit credentials | Useful around runas, remote administration, scripts, and credential reuse |
| 4672 | Special privileges were assigned to a new logon | Helps identify privileged sessions; common for expected administrative and system activity |
| 4688 | A new process was created | Connects a session and identity to execution when command-line auditing is available |
| 4768 | A Kerberos authentication ticket (TGT) was requested | Domain-controller view of initial Kerberos authentication |
| 4769 | A Kerberos service ticket was requested | Useful for service access, encryption types, and unusual service-ticket volume |
| 4771 | Kerberos pre-authentication failed | Normally observed on a domain controller; investigate status, client, account, and surrounding Kerberos activity |
| 4776 | An NTLM credential validation was attempted | Written on the system authoritative for the supplied credentials: normally a domain controller for domain accounts and the local computer for local accounts |
| 4778 / 4779 | A session was reconnected / disconnected | Useful for Terminal Services and RDP session continuity; can also reflect Fast User Switching or Hyper-V Enhanced Session Mode |
| 4740 | A user account was locked out | Explains failure sequences and identifies the caller computer where populated |
| 1102 | The audit log was cleared | May explain missing Security events; high-value context that requires administrative and system validation |

Event availability depends on audit policy, operating-system version, forwarding configuration, and parser quality.

## Logon types

| Type | Name | Typical context | Hunting note |
| ---: | --- | --- | --- |
| 0 | System | Operating-system use | Rare in ordinary user investigations |
| 2 | Interactive | Local console or equivalent local session | Does not by itself prove physical presence |
| 3 | Network | Access to a remote resource such as a share | Extremely common; interpret with source, service, and target context |
| 4 | Batch | Scheduled task or batch processing | Compare with task configuration and expected service identity |
| 5 | Service | Service Control Manager starting a service | Pivot to service installation, configuration, and binary path |
| 7 | Unlock | Unlocking an existing workstation session | Different from creation of a fresh interactive session |
| 8 | NetworkCleartext | Credentials supplied in an unhashed form to the authentication package | Does **not** mean the credentials crossed the network in plaintext |
| 9 | NewCredentials | Existing token cloned with different outbound credentials | Often associated with `runas /netonly`; local identity can differ from network identity |
| 10 | RemoteInteractive | Remote Desktop or similar terminal-services logon | Baseline source-to-destination pairs and follow the resulting session |
| 11 | CachedInteractive | Domain user logged on with cached credentials | Domain controller may have no corresponding live authentication |
| 12 | CachedRemoteInteractive | Same as RemoteInteractive; used internally for auditing | Do not infer cached credential validation from the name alone |
| 13 | CachedUnlock | Unlock using cached credentials | Treat as continuation of an existing session |

The name `NetworkCleartext` is easy to overread. Microsoft documents that the password is passed to the authentication package in an unhashed form; the event does not prove cleartext network transport.

References: [Event 4624 and logon types](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624), [Event 4625](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625), [Event 4776](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4776), and [administrative logon types and credential exposure](https://learn.microsoft.com/en-us/windows-server/identity/securing-privileged-access/reference-tools-logon-types)

## Fields worth preserving

| Field | Question it helps answer |
| --- | --- |
| Computer / DeviceName | Where was the event generated? |
| TargetUserName and TargetDomainName | Which identity was acted upon? |
| SubjectUserName | Which local security context initiated the action? |
| LogonType | What kind of session or resource access was requested? |
| LogonId / TargetLogonId | Which local session can be correlated with later events? |
| LinkedLogonId | Is another session explicitly linked? |
| LogonGuid / TargetLogonGuid | Can this logon be correlated with other authentication events? The value may be all zeroes |
| IpAddress and IpPort | What source did the receiving system record? |
| WorkstationName | What client name was supplied, if any? |
| ProcessName and ProcessId | Which local process participated? |
| AuthenticationPackageName | Kerberos, NTLM, Negotiate, or another package? |
| LogonProcessName | Which trusted logon process handled the request? |
| Status and SubStatus | Why did the attempt fail? |
| ElevatedToken | Was the resulting token elevated? |

A logon identifier is meaningful on the computer that issued it. Pair it with the device and use a tight time window; do not join unrelated hosts on `LogonId` alone.

## Hunt patterns

### Failed attempts followed by success

Group failures by target account, source, destination, and logon type. Then ask whether a success followed from the same source, or whether the source shifted. A password reset, unlocked account, mistyped password, service restart, or adversary can all produce this sequence.

### A service identity used interactively

Look for service or application accounts using types 2 or 10, or appearing on user workstations. Validate how the account is designed, whether an operator used it for troubleshooting, and what executed inside the session.

### Rare Remote Desktop paths

A new source-to-destination pair can be more useful than raw RDP volume. Add identity, asset role, working hours, connection broker behaviour, privileged-session tooling, and 4778/4779 session events.

### Network-logon fan-out

One source producing type 3 logons to many systems may reflect administration, inventory, backup, vulnerability scanning, malware, or share enumeration. Compare destination count, rate, share access, remote service creation, process ancestry, and the source asset's role.

### NTLM where Kerberos is expected

NTLM can be caused by IP-based access, local accounts, workgroups, legacy software, name-resolution failures, or adversary activity. Treat it as an environmental question first, then narrow to new identities, systems, and paths.

### Explicit credentials near remote activity

Correlate 4648 with process creation, outbound network activity, 4624 on the destination, service or task creation, and share access. The absence of 4648 does not prove credentials were not used.

## A starting Kusto query

This example keeps enough dimensions to expose different behaviours instead of collapsing all failures into one account count.

```sql
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID == 4625
| summarize Failures=count(),
            FirstSeen=min(TimeGenerated),
            LastSeen=max(TimeGenerated),
            Statuses=make_set(strcat(Status, "/", SubStatus), 10)
    by TargetAccount, Computer, IpAddress, LogonType
| where Failures >= 10
| order by Failures desc
```

Before operational use:

- Inspect raw events for the highest groups.
- Confirm whether `IpAddress` is an endpoint, proxy, broker, or missing value.
- Split expected machine and service-account patterns from human accounts.
- Test whether collection gaps create apparent “failure without success” sequences.
- Decide whether the entity is the account, source, destination, or a relationship between them.

## Investigation workflow

1. Establish the viewpoint and timestamp of the event.
2. Identify the target identity and logon type.
3. Resolve the source cautiously; include NAT, jump hosts, gateways, and brokers.
4. Correlate the local logon ID with privilege and process events.
5. Correlate Kerberos activity with 4768, 4769, and domain-related 4771 events from the domain controller's perspective. Correlate NTLM credential validation with relevant 4776 events from the authoritative system.
6. Follow the session into execution, network, service, task, share, and file telemetry.
7. Compare the relationship with historical behaviour and asset purpose.
8. Document what the events prove, what they suggest, and what telemetry is absent.

## Revision

| Revised Date | Comment |
| --- | --- |
| 2024-10-06 | Added page |
| 2026-07-23 | Rewritten as a correlation and investigation reference |
