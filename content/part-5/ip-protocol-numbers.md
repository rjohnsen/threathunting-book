---
title: "IP Protocol Numbers"
date: 2025-03-16T11:29:22+01:00
draft: false
hidden: false
weight: 1
tags:
    - cheatsheet
    - protocols
    - services
summary: ""
---

|Revised Date | Author | Comment |
| ----------- | ------ | ------- |
| 16.03.2025  | Roger Johnsen | Article added |

## Introduction

**For threat hunters, knowing the list of IP protocol numbers is crucial for identifying and analyzing network traffic - or when even working with partially parsed logs. Each protocol number in the IP header represents a specific protocol used within a network, helping you quickly identify the type of traffic - such as TCP, UDP, or ICMP - that’s flowing through your environment. This understanding allows you to pinpoint anomalies or unauthorized activities that could indicate malicious behavior. While some protocols are commonly used, others may be less familiar, making it vital to have a reference list for accurate analysis.**

---

The following list is sourced from "[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)" on Wikipedia:

| Hex | Protocol Number | Keyword | Protocol | References/RFC |
| --- | --------------- | ------- | -------- | -------------- | 
| 0x00 | 0 | HOPOPT | IPv6 Hop-by-Hop Option | RFC 8200 |
| 0x01 | 1 | ICMP | Internet Control Message Protocol | RFC 792 |
| 0x02 | 2 | IGMP | Internet Group Management Protocol | RFC 1112 |
| 0x03 | 3 | GGP | Gateway-to-Gateway Protocol | RFC 823 |
| 0x04 | 4 | IP-in-IP | IP in IP (encapsulation) | RFC 2003 |
| 0x05 | 5 | ST | Internet Stream Protocol | RFC 1190, RFC 1819 |
| 0x06 | 6 | TCP | Transmission Control Protocol | RFC 793 |
| 0x07 | 7 | CBT | Core-based trees | RFC 2189 |
| 0x08 | 8 | EGP | Exterior Gateway Protocol | RFC 888 |
| 0x09 | 9 | IGP | Interior gateway protocol (any private interior gateway, for example Cisco's IGRP) |  |
| 0x0A | 10 | BBN-RCC-MON | BBN RCC Monitoring |  |
| 0x0B | 11 | NVP-II | Network Voice Protocol | RFC 741 |
| 0x0C | 12 | PUP | Xerox PUP |  |
| 0x0D | 13 | ARGUS | ARGUS |  |
| 0x0E | 14 | EMCON | EMCON |  |
| 0x0F | 15 | XNET | Cross Net Debugger | IEN 158[2] |
| 0x10 | 16 | CHAOS | Chaos |  |
| 0x11 | 17 | UDP | User Datagram Protocol | RFC 768 |
| 0x12 | 18 | MUX | Multiplexing | IEN 90[3] |
| 0x13 | 19 | DCN-MEAS | DCN Measurement Subsystems |  |
| 0x14 | 20 | HMP | Host Monitoring Protocol | RFC 869 |
| 0x15 | 21 | PRM | Packet Radio Measurement |  |
| 0x16 | 22 | XNS-IDP | XEROX NS IDP |  |
| 0x17 | 23 | TRUNK-1 | Trunk-1 |  |
| 0x18 | 24 | TRUNK-2 | Trunk-2 |  |
| 0x19 | 25 | LEAF-1 | Leaf-1 |  |
| 0x1A | 26 | LEAF-2 | Leaf-2 |  |
| 0x1B | 27 | RDP | Reliable Data Protocol | RFC 908 |
| 0x1C | 28 | IRTP | Internet Reliable Transaction Protocol | RFC 938 |
| 0x1D | 29 | ISO-TP4 | ISO Transport Protocol Class 4 | RFC 905 |
| 0x1E | 30 | NETBLT | Bulk Data Transfer Protocol | RFC 998 |
| 0x1F | 31 | MFE-NSP | MFE Network Services Protocol |  |
| 0x20 | 32 | MERIT-INP | MERIT Internodal Protocol |  |
| 0x21 | 33 | DCCP | Datagram Congestion Control Protocol | RFC 4340 |
| 0x22 | 34 | 3PC | Third Party Connect Protocol |  |
| 0x23 | 35 | IDPR | Inter-Domain Policy Routing Protocol | RFC 1479 |
| 0x24 | 36 | XTP | Xpress Transport Protocol |  |
| 0x25 | 37 | DDP | Datagram Delivery Protocol |  |
| 0x26 | 38 | IDPR-CMTP | IDPR Control Message Transport Protocol |  |
| 0x27 | 39 | TP++ | TP++ Transport Protocol |  |
| 0x28 | 40 | IL | IL Transport Protocol |  |
| 0x29 | 41 | IPv6 | IPv6 Encapsulation (6to4 and 6in4) | RFC 2473 |
| 0x2A | 42 | SDRP | Source Demand Routing Protocol | RFC 1940 |
| 0x2B | 43 | IPv6-Route | Routing Header for IPv6 | RFC 8200 |
| 0x2C | 44 | IPv6-Frag | Fragment Header for IPv6 | RFC 8200 |
| 0x2D | 45 | IDRP | Inter-Domain Routing Protocol |  |
| 0x2E | 46 | RSVP | Resource Reservation Protocol | RFC 2205 |
| 0x2F | 47 | GRE | Generic Routing Encapsulation | RFC 2784, RFC 2890 |
| 0x30 | 48 | DSR | Dynamic Source Routing Protocol | RFC 4728 |
| 0x31 | 49 | BNA | Burroughs Network Architecture |  |
| 0x32 | 50 | ESP | Encapsulating Security Payload | RFC 4303 |
| 0x33 | 51 | AH | Authentication Header | RFC 4302 |
| 0x34 | 52 | I-NLSP | Integrated Net Layer Security Protocol | TUBA |
| 0x35 | 53 | SwIPe | SwIPe | RFC 5237 |
| 0x36 | 54 | NARP | NBMA Address Resolution Protocol | RFC 1735 |
| 0x37 | 55 | MOBILE | IP Mobility  (Min Encap) | RFC 2004 |
| 0x38 | 56 | TLSP | Transport Layer Security Protocol (using Kryptonet key management) |  |
| 0x39 | 57 | SKIP | Simple Key-Management for Internet Protocol | RFC 2356 |
| 0x3A | 58 | IPv6-ICMP | ICMP for IPv6 | RFC 4443, RFC 4884 |
| 0x3B | 59 | IPv6-NoNxt | No Next Header for IPv6 | RFC 8200 |
| 0x3C | 60 | IPv6-Opts | Destination Options for IPv6 | RFC 8200 |
| 0x3D | 61 |  | Any host internal protocol |  |
| 0x3E | 62 | CFTP | CFTP |  |
| 0x3F | 63 |  | Any local network |  |
| 0x40 | 64 | SAT-EXPAK | SATNET and Backroom EXPAK |  |
| 0x41 | 65 | KRYPTOLAN | Kryptolan |  |
| 0x42 | 66 | RVD | MIT Remote Virtual Disk Protocol |  |
| 0x43 | 67 | IPPC | Internet Pluribus Packet Core |  |
| 0x44 | 68 |  | Any distributed file system |  |
| 0x45 | 69 | SAT-MON | SATNET Monitoring |  |
| 0x46 | 70 | VISA | VISA Protocol |  |
| 0x47 | 71 | IPCU | Internet Packet Core Utility |  |
| 0x48 | 72 | CPNX | Computer Protocol Network Executive |  |
| 0x49 | 73 | CPHB | Computer Protocol Heart Beat |  |
| 0x4A | 74 | WSN | Wang Span Network |  |
| 0x4B | 75 | PVP | Packet Video Protocol |  |
| 0x4C | 76 | BR-SAT-MON | Backroom SATNET Monitoring |  |
| 0x4D | 77 | SUN-ND | SUN ND PROTOCOL-Temporary |  |
| 0x4E | 78 | WB-MON | WIDEBAND Monitoring |  |
| 0x4F | 79 | WB-EXPAK | WIDEBAND EXPAK |  |
| 0x50 | 80 | ISO-IP | International Organization for Standardization Internet Protocol |  |
| 0x51 | 81 | VMTP | Versatile Message Transaction Protocol | RFC 1045 |
| 0x52 | 82 | SECURE-VMTP | Secure Versatile Message Transaction Protocol | RFC 1045 |
| 0x53 | 83 | VINES | VINES |  |
| 0x54 | 84 | TTP | TTP (Transaction Transport Protocol) (obsoleted March 2023) |  |
| 0x54 | 84 | IPTM | Internet Protocol Traffic Manager |  |
| 0x55 | 85 | NSFNET-IGP | NSFNET-IGP |  |
| 0x56 | 86 | DGP | Dissimilar Gateway Protocol |  |
| 0x57 | 87 | TCF | TCF |  |
| 0x58 | 88 | EIGRP | EIGRP | Informational RFC 7868 |
| 0x59 | 89 | OSPF | Open Shortest Path First | RFC 2328 |
| 0x5A | 90 | Sprite-RPC | Sprite RPC Protocol |  |
| 0x5B | 91 | LARP | Locus Address Resolution Protocol |  |
| 0x5C | 92 | MTP | Multicast Transport Protocol |  |
| 0x5D | 93 | AX.25 | AX.25 |  |
| 0x5E | 94 | OS | KA9Q NOS compatible IP over IP tunneling |  |
| 0x5F | 95 | MICP | Mobile Internetworking Control Protocol |  |
| 0x60 | 96 | SCC-SP | Semaphore Communications Sec. Pro |  |
| 0x61 | 97 | ETHERIP | Ethernet-within-IP Encapsulation | RFC 3378 |
| 0x62 | 98 | ENCAP | Encapsulation Header | RFC 1241 |
| 0x63 | 99 |  | Any private encryption scheme |  |
| 0x64 | 100 | GMTP | GMTP |  |
| 0x65 | 101 | IFMP | Ipsilon Flow Management Protocol |  |
| 0x66 | 102 | PNNI | PNNI over IP |  |
| 0x67 | 103 | PIM | Protocol Independent Multicast |  |
| 0x68 | 104 | ARIS | IBM's ARIS (Aggregate Route IP Switching) Protocol |  |
| 0x69 | 105 | SCPS | SCPS (Space Communications Protocol Standards) | SCPS-TP[4] |
| 0x6A | 106 | QNX | QNX |  |
| 0x6B | 107 | A/N | Active Networks |  |
| 0x6C | 108 | IPComp | IP Payload Compression Protocol | RFC 3173 |
| 0x6D | 109 | SNP | Sitara Networks Protocol |  |
| 0x6E | 110 | Compaq-Peer | Compaq Peer Protocol |  |
| 0x6F | 111 | IPX-in-IP | IPX in IP |  |
| 0x70 | 112 | VRRP | Virtual Router Redundancy Protocol, Common Address Redundancy Protocol (not IANA assigned) | RFC 5798 |
| 0x71 | 113 | PGM | PGM Reliable Transport Protocol | RFC 3208 |
| 0x72 | 114 |  | Any 0-hop protocol |  |
| 0x73 | 115 | L2TP | Layer Two Tunneling Protocol Version 3 | RFC 3931 |
| 0x74 | 116 | DDX | D-II Data Exchange (DDX) |  |
| 0x75 | 117 | IATP | Interactive Agent Transfer Protocol |  |
| 0x76 | 118 | STP | Schedule Transfer Protocol |  |
| 0x77 | 119 | SRP | SpectraLink Radio Protocol |  |
| 0x78 | 120 | UTI | Universal Transport Interface Protocol |  |
| 0x79 | 121 | SMP | Simple Message Protocol |  |
| 0x7A | 122 | SM | Simple Multicast Protocol | draft-perlman-simple-multicast-03 |
| 0x7B | 123 | PTP | Performance Transparency Protocol |  |
| 0x7C | 124 | IS-IS over IPv4 | Intermediate System to Intermediate System (IS-IS) Protocol over IPv4 | RFC 1142 and RFC 1195 |
| 0x7D | 125 | FIRE | Flexible Intra-AS Routing Environment |  |
| 0x7E | 126 | CRTP | Combat Radio Transport Protocol |  |
| 0x7F | 127 | CRUDP | Combat Radio User Datagram |  |
| 0x80 | 128 | SSCOPMCE | Service-Specific Connection-Oriented Protocol in a Multilink and Connectionless Environment | ITU-T Q.2111 (1999) |
| 0x81 | 129 | IPLT |  |  |
| 0x82 | 130 | SPS | Secure Packet Shield |  |
| 0x83 | 131 | PIPE | Private IP Encapsulation within IP | Expired I-D draft-petri-mobileip-pipe-00.txt |
| 0x84 | 132 | SCTP | Stream Control Transmission Protocol | RFC 4960 |
| 0x85 | 133 | FC | Fibre Channel |  |
| 0x86 | 134 | RSVP-E2E-IGNORE | Reservation Protocol (RSVP) End-to-End Ignore | RFC 3175 |
| 0x87 | 135 | Mobility Header | Mobility Extension Header for IPv6 | RFC 6275 |
| 0x88 | 136 | UDPLite | Lightweight User Datagram Protocol | RFC 3828 |
| 0x89 | 137 | MPLS-in-IP | Multiprotocol Label Switching Encapsulated in IP | RFC 4023, RFC 5332 |
| 0x8A | 138 | manet | MANET Protocols | RFC 5498 |
| 0x8B | 139 | HIP | Host Identity Protocol | RFC 5201 |
| 0x8C | 140 | Shim6 | Site Multihoming by IPv6 Intermediation | RFC 5533 |
| 0x8D | 141 | WESP | Wrapped Encapsulating Security Payload | RFC 5840 |
| 0x8E | 142 | ROHC | Robust Header Compression | RFC 5856 |
| 0x8F | 143 | Ethernet | Segment Routing over IPv6 | RFC 8986 |
| 0x90 | 144 | AGGFRAG | AGGFRAG Encapsulation Payload for ESP | RFC 9347 |
| 0x91 | 145 | NSH | Network Service Header | draft-ietf-spring-nsh-sr |
| 0x92-0xFC | 146-252 | Unassigned | Unassigned |  |
| 0xFD-0xFE | 253-254 | Use for experimentation and testing | Use for experimentation and testing | RFC 3692 |
| 0xFF | 255 | Reserved | Reserved |  |

---

## References

| Resource | Description |
| ----- | --- |
| [List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers) | List of IP protocol numbers |