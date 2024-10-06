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