---
title: "Mac Addresses"
date: 2026-06-27T15:53:32+02:00
draft: false
weight: 5
---

# MAC Addresses

__Author:__ _Roger C.B. Johnsen_

## Introduction

**There are data points in an investigation that most people treat as background noise. MAC addresses are one of them. They sit at Layer 2 of the OSI model, below the IP layer where most investigators spend their time, and as a result they tend to get skipped over. The IP is enough, right? Most of the time, maybe. But sometimes the IP alone will lead you in the wrong direction, and the MAC address is the thing that corrects your course.**

**This article covers what MAC addresses are, how they are structured, where they appear in logs, and why they matter to a threat hunter. It ends with a scenario drawn from real investigation experience, one where understanding MAC addresses prevented a team from making a significant mistake.**

---

## What Is a MAC Address?

A MAC address (short for *Media Access Control address*) is a hardware identifier assigned to a network interface card (NIC). It operates at Layer 2 of the OSI model and is used to identify devices within a local network segment. While IP addresses route traffic across networks, MAC addresses are used to deliver frames within the same network.

Most network-connected devices have at least one MAC address. A laptop with both a wired Ethernet port and a wireless adapter has two. A virtual machine running on a hypervisor has a virtual MAC address assigned to each virtual NIC. Containers and certain network overlays blur this further: not everything that sends traffic has a meaningful MAC in the traditional sense.

The MAC address is 48 bits long, typically written as six pairs of hexadecimal digits:

```
AA:BB:CC:DD:EE:FF
```

Or in some systems and vendors, you will see it written with hyphens instead of colons, or without any separator at all. The format depends on the platform. Learn to recognize all three.

---

## The OUI - Organizationally Unique Identifier

The first three octets (24 bits) of a MAC address make up the **OUI** - the Organizationally Unique Identifier. This is assigned by the IEEE to hardware manufacturers. It effectively tells you who made the network interface.

```
AA:BB:CC  <-- OUI (manufacturer)
DD:EE:FF  <-- NIC-specific identifier
```

The IEEE maintains a public registry of OUI assignments. This means that if you have a MAC address, you can look up the first three octets and find out who manufactured the device, or at least who manufactured the network card.

Some well-known examples:

| OUI (first 3 octets) | Vendor                      |
|----------------------|-----------------------------|
| 00:50:56             | VMware                      |
| 00:0C:29             | VMware (Workstation/Fusion) |
| 00:05:69             | VMware                      |
| 00:15:5D             | Microsoft Hyper-V           |
| 00:1B:21             | Intel                       |
| 3C:FD:FE             | Intel                       |
| 00:1A:A1             | Juniper                     |
| 00:00:0C             | Cisco                       |

You can look up OUIs manually at [macvendors.com](https://macvendors.com) or via the IEEE registry at [regauth.standards.ieee.org](https://regauth.standards.ieee.org). There are also Python libraries and CLI tools that will do the lookup for you programmatically if you are processing large volumes.

This lookup is a simple but powerful step in an investigation. More on that in a moment.

One important caveat: the OUI identifies the manufacturer of the network interface, not the device type or its owner. An Intel OUI does not tell you it is a laptop, since Intel makes NICs for all kinds of devices. A VMware OUI tells you the interface is virtual, but not what is running inside the VM. The OUI narrows the field; it does not close it.

---

## Special MAC Addresses

Not all MAC addresses refer to specific devices. A few patterns are worth knowing:

| Address             | Meaning                                        |
|---------------------|------------------------------------------------|
| FF:FF:FF:FF:FF:FF   | Broadcast, sent to all devices on the segment |
| 01:00:5E:xx:xx:xx   | IPv4 multicast                                 |
| 33:33:xx:xx:xx:xx   | IPv6 multicast                                 |

The **broadcast address** is particularly relevant for certain attack techniques. ARP poisoning involves sending forged ARP messages so that victims associate a legitimate IP address (often the default gateway) with an attacker-controlled MAC address, redirecting traffic through the attacker.

There is also the concept of **locally administered addresses**. The second-least-significant bit of the first octet indicates whether the address was assigned by the manufacturer (globally administered) or configured locally (locally administered). This bit is called the **L/A bit**.

```
First octet in binary:  X X X X X X L G
                                     ^ ^
                         L/A bit: 0 = globally administered (manufacturer-assigned)
                                  1 = locally administered (overridden or virtual)
                                    ^
                         I/G bit: 0 = unicast (individual)
                                  1 = multicast/broadcast (group)
```

A locally administered MAC address indicates that the address is not manufacturer-assigned. This is normal in virtual environments and on devices using MAC randomization, but on a physical device in an unexpected context it can be a sign of **MAC spoofing**.

From a detection standpoint, the patterns in this table are useful filters. Broadcast and multicast traffic at unexpected volumes or from unexpected sources can signal reconnaissance or ARP-based attacks. The locally administered bit, as discussed further in the MAC Randomization section, is one of the cheapest checks you can run on any MAC address that looks suspicious.

---

## MAC Spoofing

MAC spoofing is the practice of changing the MAC address of a network interface to impersonate another device or obscure identity. It is trivial to do on most operating systems:

**Linux:**
```bash
ip link set dev eth0 down
ip link set dev eth0 address AA:BB:CC:DD:EE:FF
ip link set dev eth0 up
```

**Windows (registry):**
```powershell
# Find the adapter's registry key under HKLM\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-...}
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}\0001" `
    -Name "NetworkAddress" -Value "AABBCCDDEEFF"
# Disable and re-enable the adapter to apply
Disable-NetAdapter -Name "Ethernet" -Confirm:$false
Enable-NetAdapter -Name "Ethernet"
```

From an adversary's perspective, MAC spoofing is used to:

- Bypass weak MAC-based controls such as MAC filtering or MAC Authentication Bypass (MAB)
- Impersonate a trusted device on a network segment
- Complicate attribution during an intrusion

From a hunter's perspective, MAC spoofing leaves traces. A device that suddenly appears with a MAC address belonging to a known vendor that does not match the device type, or a MAC that was previously associated with a different IP, is worth a second look.

---

## MAC Randomization

Not every changing MAC address is malicious. Modern operating systems (including iOS, Android, and supported Windows 10/11 Wi-Fi adapters) implement MAC randomization as a privacy feature, particularly on Wi-Fi. Instead of broadcasting the hardware-burned MAC when probing for networks or associating with an access point, the device generates a randomized locally administered address. Depending on the OS and configuration, this can rotate per SSID, per connection, or on a timed schedule.

This has real consequences for hunters working in enterprise Wi-Fi environments. A device that appears to change its MAC regularly may simply be a modern smartphone or laptop with randomization enabled, not an adversary covering their tracks. The locally administered bit is your first signal: if it is set, the address was not burned by the manufacturer. That is expected behaviour for a randomized MAC, and it is also what you see with spoofing.

The distinction matters. Ask yourself: does this device show a consistent randomization pattern consistent with what the OS vendor documents, or does the MAC change in ways that do not match normal randomization behaviour? A corporate laptop that suddenly appears with a locally administered MAC on a wired segment is more suspicious than a personal phone doing the same on wireless.

Know your environment. Know which devices and OS versions are in use, and what your wireless infrastructure logs as normal. That baseline is what separates expected randomization from a signal worth chasing.

---

## Where MAC Addresses Appear in Logs

MAC addresses do not travel beyond the local network segment. Once traffic crosses a router, the Layer 2 header is stripped and replaced. Put differently, a MAC address is only meaningful within its broadcast domain, such as a VLAN or physical segment. This limits where you will find MAC addresses in your telemetry, but they do appear in several important places.

In cloud and overlay-heavy environments, this gets more complicated. AWS, Azure, and similar platforms often abstract MAC visibility away from the analyst, or expose it in ways that are less useful for traditional Layer 2 pivots. Overlay networks like VXLAN and SDN fabrics can decouple Layer 2 from physical infrastructure, making MAC-based pivots unreliable or meaningless. If you are working in a heavily virtualised or cloud-native environment, set your expectations accordingly. MAC may simply not be a useful pivot point there.

| Source                          | Where MACs Appear                                             |
|---------------------------------|---------------------------------------------------------------|
| DHCP logs                       | IP-to-MAC binding at lease assignment                        |
| ARP tables / ARP logs           | IP-to-MAC mapping on switches and routers                    |
| Network access control (NAC)    | Device registration and authentication events                |
| 802.1X / RADIUS logs            | Authentication events including device MAC                   |
| Switch port logs                | MAC-to-port mapping (CAM table)                              |
| Wireless controller logs        | Association events with client MAC                           |
| DHCP server logs (Windows)      | Event ID 10 (assign), 11 (renew), 12 (release), 13 (denied), 14 (deleted) |
| Sysmon (Event ID 3)             | Network connections, does not include MAC directly           |
| Defender for Endpoint           | DeviceNetworkInfo includes MAC address per network adapter    |
| Splunk with network data        | MAC visible if DHCP or switch logs are ingested              |

In DHCP-managed networks, the DHCP server log is often the best first source for IP-to-MAC correlation. If you have an IP and a timestamp, check DHCP early. The lease record ties the IP to a MAC at a specific point in time. From there, you can often correlate the MAC to a device and, depending on the environment, to a user. That said, validate against other sources where available (NAC, switch logs, wireless controllers, EDR device inventory) because static IPs, VPN pools, VDI, and short lease cycles can all complicate the picture.

---

## When One IP Tells Five Different Stories

The following scenario is a composite drawn from real investigation experience. The details have been changed, but the underlying situation and the mistake that was nearly made are real.

A server administrator notices a suspicious logon on a system they manage, something that does not look right, and raises it for investigation. The starting point is a single data point: an IP address, nothing else.

The IP is traced through the available log sources - endpoint telemetry, authentication logs, network data - and the trail leads to a standard-issue corporate laptop, the kind the organisation deploys to its users. A user is associated with the device and the picture starts to take shape.

At the same time, the network team is brought in independently. They go to their own sources and come back with a different answer: according to them, that IP belongs to a mobile phone.

Two investigators, two data sources, two contradictory answers. One says laptop, one says phone. This is a fork in the road that investigations hit more often than people admit. The temptation is to argue about which source is more authoritative and commit to one answer. The better move is to go one layer deeper.

The MAC addresses associated with that IP are pulled. There are five of them.

The first instinct is multiple network interfaces - wired, wireless, maybe a VPN adapter - which would account for two or three MACs. Five is a different problem. OUI lookups are run on each one, and the results come back as a mix of network infrastructure vendors (routing and firewall manufacturers) alongside the mobile device vendor and the laptop manufacturer.

That is the explanation. The IP sits in a part of the network that passes through several layers of physical and virtual infrastructure, and some of those MACs belong to network equipment whose interfaces appear in ARP tables in relation to that address. The mobile device and the laptop are both legitimate end-user devices, each associated with that IP at different points in time - different lease periods, different moments captured in different log sources. Neither investigator was wrong. The data was just more layered than a single IP address could convey.

Then comes the containment discussion, and the suggestion is raised: block those MAC addresses.

The answer has to be no. Blocking the addresses tied to network infrastructure would take down parts of the network itself. What looked like a clean list of suspects was actually a mix of end-user devices and critical network equipment, indistinguishable from each other without the OUI lookup and an understanding of the topology.

The lesson is not that anyone made a careless mistake. The lesson is that MAC addresses require context to be useful. The OUI tells you who made the hardware, but it does not tell you what role that hardware plays in this specific environment at this specific moment. Those are two different questions, and both need an answer before you act. The mistake was not misidentification - it was assuming that all the data represented a single point in time and a single device.

---

## What This Means for Threat Hunters

A few things to take away from this:

**MAC addresses are context, not conclusions.** They are one more data point in a chain of evidence. They can confirm, contradict, or complicate the picture, but they rarely tell the whole story on their own.

**OUI lookups are fast and should be routine.** If you have a MAC address in your investigation, run the OUI. It takes seconds and it can immediately tell you whether something is off. A device claiming to be one thing but carrying the OUI of a completely different manufacturer is worth investigating. Remember though: the OUI tells you who made the interface, not what the device is or who owns it.

**Virtual MACs are everywhere.** Hypervisors, network appliances, VPN concentrators, and SDN environments all generate MAC addresses that are not tied to physical hardware in the traditional sense. Know your environment. Know which OUI ranges your virtualization platforms use.

**IP-to-MAC mappings are not static.** DHCP leases expire. Devices reconnect. In a busy network, the same IP can cycle through several devices over the course of days or weeks. Always correlate with timestamps. A MAC seen today may not be the same device that held that IP last Tuesday.

**A changing MAC is not always an attack.** Modern operating systems randomize MAC addresses for privacy, especially on Wi-Fi. Check the locally administered bit, check the OS and device type, and check whether the pattern matches documented randomization behaviour before treating it as suspicious.

**MAC addresses support detection, not just investigation.** A few patterns worth building detection logic around:

- The same MAC appearing on multiple switch ports simultaneously. This can indicate a port security issue or active spoofing.
- A device with a corporate hostname but a non-corporate OUI on a wired segment. Something is inconsistent and worth a look.
- A single device identity (hostname or asset ID) appearing with MAC addresses from different OUIs over a short timeframe. A physical NIC has one OUI baked in. If your logs show the same device alternating between, say, an Intel OUI and a VMware OUI, you have either a data quality problem or something worth investigating.
- Two devices claiming the same MAC address simultaneously. This is a collision and should not happen on a healthy network. It points to either a misconfiguration or active spoofing where an attacker is impersonating a known device.
- A MAC that cycles through multiple addresses on the same port in a short window. Normal devices do not do this. It can indicate an attacker iterating through addresses to probe access controls.

**Blocking by MAC requires careful judgement.** There are scenarios where MAC-based containment makes sense: NAC enforcement on a controlled wireless segment, or isolation within a very small and well-understood Layer 2 domain. But in routed environments, MAC blocks do not propagate beyond the local segment. And as the scenario in this article illustrated, a list of MACs associated with a suspicious IP may contain infrastructure addresses (routers, firewalls, switches) that you absolutely do not want to block. Verify what each MAC belongs to before acting.

---

## The Pivot Workflow

When a MAC address becomes relevant in an investigation, the pivot follows a consistent pattern. The exact queries will depend on your SIEM and data sources, but the logic is the same across platforms.

**Step 1 - Resolve IP to MAC via DHCP**

Start with the IP and the timestamp of the event you are investigating. Go to your DHCP logs and find the lease that was active at that time.

**Example (KQL - Windows DHCP Server events in Sentinel or Defender):**
```sql
// Windows DHCP server logs event ID 10 = lease assigned
// Adjust table name to match your ingestion
Event
| where EventID == 10
| where RenderedDescription contains "<suspicious_ip>"
| project TimeGenerated, RenderedDescription
| order by TimeGenerated desc
```

**Example (SPL - Splunk with DHCP sourcetype):**
```sql
index=network sourcetype=dhcp
| search src_ip="<suspicious_ip>"
| table _time, src_ip, src_mac, host
| sort -_time
```

**Step 2 - Pivot MAC across data sources**

Once you have the MAC, search for it across NAC logs, wireless controller logs, EDR device inventory, and switch logs. You are looking to confirm device identity and build a timeline of where that MAC has been seen.

```sql
// Splunk - pivot MAC across all indexed network sources
index=network "<mac_address>"
| table _time, src_ip, src_mac, host, sourcetype
| sort -_time
```

**Step 3 - Validate device identity**

Cross-reference the MAC with your asset inventory and EDR. Check whether the device hostname, OS, and user match what you expect for that MAC and OUI combination.

```sql
// KQL - Defender for Endpoint, DeviceNetworkInfo table
// Note: adjust field names and dynamic parsing to match your tenant schema
DeviceNetworkInfo
| where NetworkAdapterMacAddress =~ "<mac_address>"
| project Timestamp, DeviceName, NetworkAdapterName, NetworkAdapterType, NetworkAdapterMacAddress, IPAddresses
| order by Timestamp desc
```

**Step 4 - Build the timeline**

Collect all log entries associated with that MAC (DHCP leases, authentication events, network connections) and order them by time. You are looking for gaps, overlaps, and anything that does not fit the expected behaviour of the identified device.

The pivot chain in summary:

- IP + Timestamp -> DHCP -> MAC address
- MAC -> NAC / Wireless / Switch logs -> device confirmation
- MAC -> EDR device inventory -> hostname, OS, user
- Host -> Authentication logs -> what did this device do?

**When DHCP data is unavailable**

DHCP logs are not always present, retained, or reliable. Short lease times, DHCP relay configurations, or simply gaps in log ingestion can leave you without a clean IP-to-MAC record. In that case, fall back to ARP tables from switches or routers, NAC logs, or EDR device inventory, but expect reduced timestamp accuracy and a less precise correlation window. The pivot logic remains the same; the confidence level is just lower.

---

## Summary

MAC addresses sit below the layer where most investigators work, but they are more useful than they appear. The OUI embedded in the first three octets can tell you who made the hardware, or more precisely, who made the network interface. That single lookup can confirm or challenge your assumptions about a device, and it costs you almost nothing to check.

The scenario in this article is the kind of situation that does not make it into reports or post-mortems, because nothing went wrong. Someone asked the right question at the right moment. That is usually how it works. Neither investigator was wrong. The data was just more complex than a single IP address could convey.

Understanding MAC addresses will not solve every investigation. But not understanding them has a way of creating problems at exactly the wrong moment.

---

## Resources

- [IEEE OUI Registry](https://regauth.standards.ieee.org/standards-ra-web/pub/view.html#registries)
- [MAC Vendors Lookup](https://macvendors.com)
- [Wireshark OUI Lookup](https://www.wireshark.org/tools/oui-lookup.html)
- [MITRE ATT&CK - T1557.002 ARP Cache Poisoning](https://attack.mitre.org/techniques/T1557/002/)
- [RFC 826 - An Ethernet Address Resolution Protocol](https://www.rfc-editor.org/rfc/rfc826)

---

## Revision

| Revised Date | Author        | Comment       |
|--------------|---------------|---------------|
| 27.06.2026   | Roger Johnsen | Article added |