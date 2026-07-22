---
title: "IP Protocol Numbers"
date: 2025-03-16T11:29:22+01:00
draft: false
hidden: false
weight: 4
tags: [cheatsheet, protocols, network]
summary: "A hunting-oriented reference for IP protocol and IPv6 Next Header values."
---

__Author:__ _Roger C.B. Johnsen_

The IPv4 protocol field and IPv6 `Next Header` field identify what follows the IP header. They are not TCP or UDP ports: protocol `6` means TCP.

## Values worth recognizing

| Decimal | Keyword | Hunting relevance |
|---:|---|---|
| 0 | HOPOPT | IPv6 extension header; validate parser visibility and expected use. |
| 1 | ICMP | Baseline type, direction, volume, and payload size. |
| 2 | IGMP | Usually local multicast control; routed traffic deserves validation. |
| 4 | IPv4 | Encapsulation can be legitimate tunneling or an inspection gap. |
| 6 | TCP | Interpret with ports, flags, direction, process, and result. |
| 17 | UDP | Distinguish request/response from scanning and one-way beaconing. |
| 41 | IPv6 | Hunt for unmanaged transition tunnels and unexpected IPv6 use. |
| 43/44/60 | IPv6 extension headers | Inspect unusual chains, fragmentation, and sensor coverage. |
| 47 | GRE | Common on infrastructure; unusual from ordinary endpoints. |
| 50/51 | ESP/AH | Correlate with approved VPN peers; ESP payload is encrypted. |
| 58 | ICMPv6 | Essential to IPv6; do not treat all ICMPv6 as hostile. |
| 59 | No Next Header | Validate malformed or evasive IPv6 traffic. |
| 88/89 | EIGRP/OSPF | Unexpected from user endpoints or non-routing segments. |
| 103/112 | PIM/VRRP | Validate senders against network-device inventory. |
| 115 | L2TPv3 | Distinct from UDP/1701; verify approved tunneling. |
| 132 | SCTP | Expected in some telecom workloads, rare elsewhere. |
| 253-254 | Experimental | Establish local ownership before escalation. |
| 255 | Reserved | Validate parser output and packet integrity. |

## Fast triage

1. Confirm this is an IP protocol/Next Header value, not a port.
2. Identify endpoint roles and compare them with the segment baseline.
3. Check whether encapsulation or extension headers reduce visibility.
4. Pivot to duration, bytes, direction, packet capture, process telemetry, and DNS.

Use the live [IANA Protocol Numbers registry](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) for exact assignments.

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2025-03-16 | Article added |
| 2026-07-22 | Reworked as a hunting reference and aligned with IANA |
