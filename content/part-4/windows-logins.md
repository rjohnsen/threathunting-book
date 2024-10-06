---
title: "Windows Logins"
date: 2024-10-06T13:28:41+02:00
draft: false
---

| Revised Date | Comment |
| ------------ | ------- |
| 06.10.2024   | Page added | 

## Introduction

**Windows security logs are essential for understanding user activity on a system. Two critical events that provide insights into user logon attempts are Event 4624 and Event 4625. These logs help track both successful and failed logons, allowing administrators, SOC and threat hunters to monitor access and detect suspicious behavior. Monitoring both of these events is vital for maintaining system security. While Event 4624 provides visibility into successful logon attempts, event 4625 helps detect potential security risks by logging failed access attempts. Together, they offer a comprehensive view of who is accessing your system and highlight potential threats. Since these are so central, I'll share with you my notes on these events.** 

## Windows Event 4624 (Successful Logon)

Let's first start by looking at successful logons. Event ID **4624** is logged whenever a user successfully logs into a Windows system (local and networked). It plays an essential role in auditing user activity and ensuring the system's security. This event is recorded in the **Security** section of the Windows Event Viewer.

Example how this event looks like in Windows event viewer (courtesy Microsoft):

![Event viewer for event id 4624](/images/event-4624.png)

### Key Uses

- **Auditing**: Event 4624 helps monitor user access to both local and networked machines.
- **Security**: It enables the detection of legitimate logon activity and identifies unauthorized access.

### Common Logon Types

Windows logon types are numerical identifiers that specify the method or context in which a user logs on to a system. Each logon type corresponds to a different method of authentication and access, helping to categorize how users interact with the Windows operating system. Here’s some common logn types (we'll look into more of them later in this chapter):

- **Logon Type 2 (Interactive)**: Occurs when a user logs on directly via a keyboard or mouse.
- **Logon Type 3 (Network)**: Used when accessing shared resources, such as files or printers over the network.
- **Logon Type 10 (RemoteInteractive)**: Occurs when a user logs on remotely through services like Remote Desktop (RDP).

These logon types are quite nifty to track in a hunt to understand lateral movement, amongst other things.

### Key Details of Event 4624 (Successful Logon)

A Windows log entry contains many fields. Here are some you should pay attention to:

| Field                    | Description                                                                 |
|---------------------------|-----------------------------------------------------------------------------|
| **Logon Type**            | Method used for logging in (e.g., interactive, network, or remote).         |
| **Account Name**          | The username that successfully logged on.                                   |
| **Account Domain**        | The domain the account belongs to (local machine or network domain).        |
| **Logon ID**              | Unique identifier for the logon session.                                    |
| **Security ID (SID)**     | The security identifier associated with the account.                        |
| **Source Network Address**| IP address of the machine where the logon originated (important for remote logons). |
| **Logon Process**         | The process responsible for handling the logon (e.g., `NtLmSsp`, `Kerberos`).|
| **Authentication Package**| Authentication protocol used (e.g., NTLM, Kerberos).                        |
| **Impersonation Level**   | Specifies the degree of impersonation rights granted, if any.               |

## Windows Event 4625 (Failed Logon)

Event ID **4625** is triggered when a logon attempt fails. This event provides crucial information on failed authentication, helping detect unauthorized access attempts, such as brute force attacks. Like Event 4624, it is logged in the **Security** section of the Windows Event Viewer.

Example how this event looks like in Windows event viewer (courtesy Microsoft):

![Event viewer for event id 4625](/images/event-4625.png)

Keeping track of how many failed attempts pr. logon type is a great way to find indicators of foul play in the logs. 

### Key Uses

- **Intrusion Detection**: Tracks multiple failed logon attempts to identify possible malicious activity, like brute-force attacks.
- **Audit Failed Access**: Provides insights into user account issues, such as incorrect passwords or locked accounts.

### Common Logon Types

As with event id 4624, we got several logon types here as well:

- **Logon Type 2 (Interactive)**: Failed local logon attempts, typically due to mistyped passwords.
- **Logon Type 3 (Network)**: Failed attempts to access network resources, such as shared folders.

### Key Details of Event 4625 (Failed Logon)

A Windows log entry contains many fields. Here are some you should pay attention to when dealing with failed log ins: 


| Field                    | Description                                                                 |
|---------------------------|-----------------------------------------------------------------------------|
| **Reason for Failure**    | The specific reason for the logon failure (e.g., incorrect password, account disabled).   |
| **Logon Type**            | Method of attempted logon (e.g., local, network, or remote).                 |
| **Account Name**          | The username used in the failed logon attempt.                               |
| **Account Domain**        | The domain to which the account belongs.                                     |
| **Source Network Address**| The IP address of the machine from which the failed logon attempt originated. |
| **Logon Process**         | The process responsible for handling the logon attempt.                      |
| **Failure Code**          | Hexadecimal code giving more details on why the logon failed (e.g., `0xC000006A` for bad password). |

## Logon Types

As I stated earlier, we would look more into the various logon types available. Under is a nice reference list for determining what logons are: 

| Logon Type | Name                      | Description                                                                 |
|------------|---------------------------|-----------------------------------------------------------------------------|
| 2          | Interactive               | Logon when a user interacts with the system locally (keyboard or mouse).    |
| 3          | Network                   | Logon to access shared network resources (e.g., shared folder, printer).    |
| 4          | Batch                     | Logon for scheduled tasks or batch jobs.                                    |
| 5          | Service                   | Logon when a service starts under a service account.                        |
| 7          | Unlock                    | Logon when a user unlocks their workstation.                                |
| 8          | NetworkCleartext          | Logon using cleartext credentials for network authentication.               |
| 9          | NewCredentials            | Logon using new credentials while maintaining the current process (RunAs).  |
| 10         | RemoteInteractive         | Logon via Remote Desktop or Terminal Services.                              |
| 11         | CachedInteractive         | Logon using cached domain credentials.                                      |
| 12         | CachedRemoteInteractive   | Cached credentials used for a remote interactive logon (e.g., Remote Desktop). |
| 13         | CachedUnlock              | Logon to unlock a workstation using cached domain credentials.              |

## Other Windows events to keep an eye on

We are not limited to just looking for event code 4625 and 4625 during our hunts. There are several other event codes that might catch our interest:

**4624 - Successful Logon**
- **Description**: Indicates a successful attempt to log on to a computer.
- **Important Log Fields**:
  - **SubjectUserSid**: Security identifier (SID) of the account that performed the action.
  - **SubjectUserName**: Account name that initiated the logon.
  - **LogonType**: Type of logon (e.g., interactive, remote, etc.).
  - **IpAddress**: Source IP address of the logon.
  - **WorkstationName**: Machine name where the logon occurred.
  - **TargetUserName**: User account that was logged on.

**4625 - Failed Logon**
- **Description**: Indicates an unsuccessful attempt to log on to a computer.
- **Important Log Fields**:
  - **FailureReason**: Reason for the failed logon attempt.
  - **TargetUserName**: User account name that the logon attempt was made for.
  - **IpAddress**: IP address where the failed attempt originated.
  - **LogonType**: Type of logon attempted (e.g., RDP, network, etc.).
  - **Status/SubStatus**: Error codes for the failure.

**4634 - Successful Logoff**
- **Description**: Logs when a user logs off from a session or is disconnected.
- **Important Log Fields**:
  - **TargetUserName**: Account that was logged off.
  - **LogonID**: Unique session identifier.
  - **LogonType**: Indicates how the user logged off (e.g., interactive, remote, etc.).

**4647 - User-Initiated Logoff**
- **Description**: Logs when a user initiates the logoff process.
- **Important Log Fields**:
  - **TargetUserName**: User who initiated the logoff.
  - **LogonID**: Session ID of the user logging off.

**4648 - Logon Using Explicit Credentials**
- **Description**: Occurs when a user logs on using alternate credentials (e.g., Run as).
- **Important Log Fields**:
  - **SubjectUserSid**: Security ID of the user who requested the logon.
  - **TargetUserName**: Account being logged on using explicit credentials.
  - **IpAddress**: IP address of the computer requesting the logon.
  - **ProcessName**: Process used for the logon (e.g., Runas).

**4672 - Special Privileges Assigned**
- **Description**: Logs when an account with special privileges logs on.
- **Important Log Fields**:
  - **SubjectUserSid**: Account SID of the user with special privileges.
  - **PrivilegeList**: List of special privileges assigned (e.g., SeBackupPrivilege, SeDebugPrivilege).

**4768 - Kerberos Ticket (TGT) Requested**
- **Description**: A request for a Kerberos Ticket Granting Ticket (TGT) was made.
- **Important Log Fields**:
  - **TargetUserName**: Account requesting the TGT.
  - **IpAddress**: IP address of the client requesting the TGT.
  - **ServiceName**: Service for which the TGT is requested.
  - **TicketOptions**: Kerberos ticket options such as renewal, forwarding, etc.

**4769 - Kerberos Service Ticket Requested**
- **Description**: A request for a Kerberos service ticket was made.
- **Important Log Fields**:
  - **TargetUserName**: Account requesting the service ticket.
  - **ServiceName**: Service that the ticket is being requested for.
  - **IpAddress**: Source IP address.
  - **TicketEncryptionType**: Encryption type used for the service ticket.

**4771 - Kerberos Pre-authentication Failed**
- **Description**: Occurs when the pre-authentication for a Kerberos ticket fails.
- **Important Log Fields**:
  - **TargetUserName**: Account for which the Kerberos authentication failed.
  - **FailureCode**: Reason for pre-authentication failure.
  - **IpAddress**: Client IP address that made the request.
  - **ServiceName**: Service name where the failure occurred.

**4776 - Attempted to Validate Credentials**
- **Description**: Logs when credentials were validated against a domain controller.
- **Important Log Fields**:
  - **AuthenticationPackageName**: Package used to validate credentials (e.g., NTLM, Kerberos).
  - **WorkstationName**: Name of the workstation where validation was attempted.
  - **Status**: Status of the credential validation (e.g., success or failure).

**4778 - Session Reconnected**
- **Description**: Indicates that a user has reconnected to a previously disconnected session.
- **Important Log Fields**:
  - **TargetUserName**: User account reconnected to the session.
  - **TargetLogonId**: Session ID of the reconnected session.
  - **Source Network Address**: Network address from which the session was reconnected.

**4779 - Session Disconnected**
- **Description**: Indicates that a user has disconnected from a session.
- **Important Log Fields**:
  - **TargetUserName**: User account that disconnected from the session.
  - **TargetLogonId**: Session ID of the disconnected session.
  - **Source Network Address**: Network address from which the session was disconnected.

## Resources

1. **Microsoft Documentation for Event ID 4624 (Successful Logon)**
   - [Windows Security Auditing Events - 4624](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4624)
   
2. **Microsoft Documentation for Event ID 4625 (Failed Logon)**
   - [Windows Security Auditing Events - 4625](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4625)

3. **Windows Security Event ID Reference**
   - [Event ID 4624 and 4625 Reference Guide](https://www.ultimatewindowssecurity.com/securitylog/usage.aspx?eventid=4624)
   - [Event ID 4624 and 4625](https://www.varonis.com/blog/windows-event-4624-and-4625)

4. **Security Events Explained**
   - [Understanding Windows Security Events](https://www.petri.com/windows-security-event-logging)