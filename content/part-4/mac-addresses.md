+++
title = "MAC Addresses"
date = 2026-06-27T15:53:32+02:00
lastmod = 2026-07-23T00:00:00+02:00
weight = 50
chapter = false
+++

{{% notice style="info" title="Illustrative queries" %}}
Queries on this page are illustrative starting points. They demonstrate investigation logic, not production-ready detections. Table availability, field names, action types, parsing, retention, and normal behaviour vary by environment. Inspect raw records and validate every query against your local schema and telemetry before relying on the result.
{{% /notice %}}

A MAC address can help connect an IP address to an interface, switch port, wireless association, or device record. It can also mislead you when observations from different times, broadcast domains, overlays, or network devices are treated as one identity.

The useful unit is not **MAC address alone**, but:

```text
MAC address + timestamp + observation point + network segment + data source
```

{{% notice style="warning" title="Do not base containment on a MAC address alone" %}}
Confirm what the address represented at the relevant time and from the relevant network viewpoint. A result set may contain client interfaces, virtual interfaces, shared or high-availability addresses, and next-hop infrastructure. Blocking the wrong address can disrupt unrelated systems or network services.
{{% /notice %}}

## What a MAC address is

A MAC address is a Layer 2 address associated with a network interface. It may be universally or locally administered. This note mainly covers 48-bit MAC addresses used with Ethernet and IEEE 802.11. Universally administered 48-bit addresses are formally EUI-48 identifiers; locally administered addresses use the same 48-bit format but are not globally unique EUI-48 identifiers. They are normally written as six hexadecimal octets:

```text
AA:BB:CC:DD:EE:FF
AA-BB-CC-DD-EE-FF
AABBCCDDEEFF
```

One device can legitimately have several addresses: wired, wireless, docking-station, virtual, and privacy-addressed interfaces may all differ. Conversely, one address can be shared deliberately by a failover or virtual-gateway design. A MAC address is therefore an interface or service clue, not a durable person or device identity.

## IEEE Assignment Prefixes: OUI, MA-L, MA-M and MA-S

IEEE Registration Authority assignments include:

| Assignment | Prefix length | Organisation-controlled EUI-48 bits |
| --- | ---: | ---: |
| MA-L | 24 bits; includes an Organizationally Unique Identifier (OUI) | 24 bits |
| MA-M | 28 bits | 20 bits |
| MA-S | 36 bits | 12 bits |

The registry prefix identifies the organisation to which IEEE assigned the address block. It does not establish the device manufacturer, model, owner, operating system, or role. Products can use interfaces supplied by another organisation, and virtual platforms can allocate addresses from registered blocks.

Use the current [IEEE Registration Authority registries](https://standards.ieee.org/products-programs/regauth/) when attribution matters. Cached and third-party databases are convenient for bulk enrichment, but their age and interpretation should be recorded.

{{% notice style="warning" title="External lookups disclose the subject" %}}
A lookup sends the address, or at least its prefix, to another service. Use the authoritative IEEE registry or an approved local copy when the investigation target is sensitive.
{{% /notice %}}

## Address bits and special ranges

The least-significant bits of the first octet describe address scope:

```text
First octet, least-significant bits: ... U/L I/G
                                      0   0  universally administered unicast
                                      1   0  locally administered unicast
                                          1  group address
```

The **U/L bit** distinguishes universal from local administration. The **I/G bit** distinguishes an individual address from a group address. A locally administered address is not automatically malicious and does not by itself prove virtualisation, randomisation, or spoofing.

| Address or range | Meaning |
| --- | --- |
| `FF:FF:FF:FF:FF:FF` | Ethernet broadcast |
| `01:00:5E:00:00:00`–`01:00:5E:7F:FF:FF` | IPv4 multicast mapping range; the low-order 23 IP multicast bits are mapped |
| `33:33:00:00:00:00`–`33:33:FF:FF:FF:FF` | IPv6 multicast mapping range; the low-order 32 IPv6 bits are mapped |

ARP poisoning operates within the local Layer 2 domain. Forged ARP messages may be sent as broadcast or unicast traffic, depending on the technique and implementation. The broadcast address itself is not an indicator of poisoning.

References: [RFC 1112, IPv4 multicast mapping](https://www.rfc-editor.org/rfc/rfc1112), [RFC 2464, IPv6 over Ethernet](https://www.rfc-editor.org/rfc/rfc2464), and [RFC 826, ARP](https://www.rfc-editor.org/rfc/rfc826).

## Legitimate address change and sharing

Modern operating systems can use locally administered addresses for Wi-Fi privacy. Rotation behaviour varies by operating system, version, network, and policy. Virtual machines, containers, bridges, software-defined networking, and some VPN or overlay designs also create or expose non-physical addresses.

Duplicate or changing addresses require context:

- **Wi-Fi privacy:** a client can use a stable private address per network or rotate it according to platform policy.
- **Docking and adapters:** the same laptop can present wired, wireless, dock, and virtual interfaces.
- **High availability:** virtual gateways, clusters, and failover designs can deliberately share or move an address.
- **Network fabrics:** MLAG/vPC, EVPN, port-channels, roaming, and VM migration can make one address visible through several paths.
- **Bridges and downstream devices:** one access port can learn several addresses behind an IP phone, hypervisor, bridge, access point, or downstream switch.
- **Configuration error or spoofing:** duplicate or unexpected use can still be malicious or accidental; the surrounding topology decides.

## Where the evidence comes from

A MAC address is not an end-to-end identifier. At a routed hop, the Layer 2 header is replaced for the next local link. The address in an ARP or neighbour table may therefore represent the next hop observed by that system, not the remote endpoint associated with an application log.

| Source | What it can establish | Important limitation |
| --- | --- | --- |
| DHCP audit data | A client identifier or hardware address associated with a lease and time | Static addresses, relay behaviour, retention, and parsing can create gaps |
| ARP or neighbour tables | Local IP-to-link-layer mapping on hosts, routers, firewalls, and Layer 3 switches | Snapshot and viewpoint dependent; may identify a next hop or proxy-ARP interface |
| Switch MAC/CAM table | Address learned on a Layer 2 port or logical interface | Port-channels, fabrics, roaming, and ageing affect the result |
| NAC / 802.1X / RADIUS | Authentication, endpoint profiling, identity, and access-port context | MAB and profiling can be weak or stale identifiers |
| Wireless controller | Client association, SSID/AP, roaming, and private-address behaviour | Controller and retention scope vary |
| Asset and endpoint inventory | Adapter, device, hostname, OS, user, and address history | Reports configuration, not necessarily every frame-level observation |
| Cloud control plane | Virtual NIC and workload metadata where exposed | Physical underlay is usually abstracted |
| Packet capture | Source and destination addresses on the capture link | Only the capture point and time are represented |

Cloud platforms may expose virtual NIC MAC addresses while abstracting the physical underlay, reducing their value for physical topology pivots.

## Windows DHCP: do not mix the ID systems

Windows DHCP exposes Event Viewer channels and text-based `DhcpSrvLog-*.log` audit files. They use different schemas and identifier spaces. The text-based audit files include a legend in their header; those record identifiers must not be treated as Windows Event Log event IDs.

Inspect the header and schema of the collected audit files before querying them. Event Viewer data instead comes from the DHCP Server Administrative, Operational, System, Filter Notification, or Audit channels and uses the event IDs documented for those channels.

A schema-neutral starting point is safer than assuming a product table:

```sql
<YourDhcpAuditTable>
| where TimeGenerated between (<start_time> .. <end_time>)
| where IPAddress == "<suspicious_ip>"
| project TimeGenerated, RecordId, Description,
          IPAddress, HostName, MacAddress
| order by TimeGenerated asc
```

Reference: [Microsoft DHCP server Event Viewer channels](https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-server-events).

## Investigation leads

None of these is a verdict. Each is a reason to inspect topology, timing, ownership, and corroborating telemetry.

| Lead | Questions to ask |
| --- | --- |
| Address observed on unexpected ports or at physically incompatible locations | Are the observations simultaneous? Are MLAG/vPC, EVPN, roaming, port-channels, or VM migration involved? |
| Duplicate address use | Is this a failover, virtual gateway, cluster, or configuration error before considering spoofing? |
| Changed registry assignee for one device identity | Have wired, wireless, docking-station, replacement, and virtual-adapter inventories been considered? |
| Locally administered address in an unusual context | Is privacy addressing or platform policy expected on this network and interface type? |
| Many addresses behind one access port | Is the port connected to an IP phone, hypervisor, bridge, AP, downstream switch, or unauthorised device? |
| New IP-to-address relationship | Was the lease active at the event time, and do NAC, wireless, switch, or endpoint records agree? |
| Address inconsistent with expected equipment | Does the registry identify a block assignee only, and could an OEM or virtual platform explain it? |

## Case study: one IP, five associations

A server administrator reported a suspicious logon and supplied an IP address. Endpoint and authentication telemetry associated the address with a managed laptop. The network team found a mobile device associated with the same address.

Across several telemetry sources and timestamps, investigators recovered five MAC associations. They did not represent one simultaneous Layer 2 mapping: some were historical DHCP clients, while others were next-hop, proxy-ARP, or infrastructure interfaces observed from different network viewpoints.

The laptop and phone had each held the IP during different lease periods. Other addresses belonged to infrastructure visible from the systems where ARP or network data had been collected. Both initial findings were valid within their own time window and viewpoint.

A suggestion was made to block every recovered MAC address. That would have risked blocking infrastructure and would not have contained activity beyond the relevant Layer 2 domain.

The defensible conclusion was narrower:

- The IP address did not represent one stable device across the full review period.
- Two client associations were supported at different times.
- Several other addresses described network-path observations rather than additional endpoints.
- Containment required the current device, account, switch/NAC context, and event time, not the union of every historical address.

## Pivot workflow

### 1. Fix the event time and viewpoint

Record the original event timestamp, timezone, source system, destination, and the system that logged the IP address. Determine whether the address was client, proxy, gateway, VPN, or translated egress from that viewpoint.

### 2. Resolve the lease or local mapping

Query DHCP, IP address management, NAC, wireless, ARP/neighbour, and relevant control-plane data for the smallest useful time window. Do not combine historical and current mappings without retaining their timestamps.

### 3. Validate the interface and device

Microsoft Defender XDR exposes adapter information through `DeviceNetworkInfo`:

```sql
DeviceNetworkInfo
| where Timestamp > ago(30d)
| where MacAddress =~ "<mac_address>"
| project Timestamp,
          ReportId,
          DeviceId,
          DeviceName,
          NetworkAdapterName,
          NetworkAdapterType,
          NetworkAdapterVendor,
          MacAddress,
          IPAddresses
| order by Timestamp desc
```

`NetworkAdapterVendor` is product metadata and may aid triage, but it is not proof of physical manufacturer or device role. Microsoft documents `ReportId` as a repeating counter that must be combined with `DeviceName` and `Timestamp` to identify an event.

Reference: [DeviceNetworkInfo schema](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-devicenetworkinfo-table).

### 4. Locate it in the topology

Use wireless association or switch MAC/CAM data to establish AP, switch, port, VLAN, or logical interface. Check whether the design can legitimately expose the address along several paths.

### 5. Correlate identity and activity

Pivot from the confirmed device and time window into authentication, process, network, file, and cloud telemetry. The address establishes local network context; it does not establish who operated the device.

### 6. State confidence and gaps

A useful note records:

| Item | Example |
| --- | --- |
| Observation | Address `02:00:5E:10:00:01` was associated with the IP in NAC telemetry |
| Time and viewpoint | 09:14 UTC, wireless controller for the corporate SSID |
| Corroboration | DHCP lease and endpoint adapter inventory agree |
| Caveat | Locally administered address; registry attribution is not applicable |
| Assessment | Strong support for the managed endpoint association at that time |
| Gap | No switch telemetry retained before 08:00 UTC |

## When DHCP is unavailable

Use NAC, wireless-controller history, IPAM, switch MAC/CAM data, ARP or neighbour tables, endpoint inventory, or cloud control-plane records. A current ARP or CAM snapshot cannot retroactively prove an older mapping. Reduce confidence when the timestamps or observation points do not align.

## Summary

A MAC address is valuable when it narrows a local network relationship at a particular time. It becomes dangerous when treated as a permanent endpoint identity or combined across incompatible viewpoints.

Keep five things together:

1. Address
2. Timestamp
3. Observation point
4. Network segment or topology
5. Data source and retention limits

That context turns a low-level address into defensible evidence.

## Resources

- [IEEE Registration Authority](https://standards.ieee.org/products-programs/regauth/)
- [Microsoft DeviceNetworkInfo schema](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-devicenetworkinfo-table)
- [Microsoft DHCP server events](https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-server-events)
- [RFC 1112: IPv4 multicasting](https://www.rfc-editor.org/rfc/rfc1112)
- [RFC 2464: IPv6 over Ethernet](https://www.rfc-editor.org/rfc/rfc2464)
- [RFC 826: ARP](https://www.rfc-editor.org/rfc/rfc826)
- [MITRE ATT&CK T1557.002: ARP Cache Poisoning](https://attack.mitre.org/techniques/T1557/002/)

## Revision

| Revised Date | Comment |
| --- | --- |
| 2026-06-27 | Added page |
| 2026-07-23 | Fully revised for Layer 2 viewpoint, IEEE assignments, DHCP schemas, and defensible investigation pivots |
