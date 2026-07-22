---
title: "Network Services and Ports"
date: 2025-03-16T10:32:41+01:00
draft: false
hidden: false
weight: 3
tags: [cheatsheet, ports, services, network]
summary: "Common ports, useful pivots, and the context needed to hunt network services."
---

__Author:__ _Roger C.B. Johnsen_

A port suggests a service; it does not prove one. Prefer application identification, process telemetry, certificates, DNS, and packet content over port labels alone.

## High-value service reference

| Port | Service | Hunt for |
| ---- | ------- | -------- |
| 20-21/TCP | FTP | Clear-text credentials, external uploads, and unapproved use. |
| 22/TCP | SSH/SFTP | New sources, unusual accounts, reverse tunnels, long sessions. |
| 23/TCP | Telnet | Clear-text remote access; require explicit ownership. |
| 25,465,587/TCP | SMTP | Direct-to-internet mail and high-volume egress. |
| 53/UDP,TCP | DNS | Unapproved resolvers, long labels, rare types, tunneling. |
| 67-68/UDP | DHCP | Rogue offers and altered gateway or DNS options. |
| 80/TCP | HTTP | Clear-text payloads, direct-IP requests, downloads. |
| 88/UDP,TCP | Kerberos | Abnormal ticket volume, downgrade, unusual hosts. |
| 123/UDP | NTP | Unapproved sources, amplification, clock changes. |
| 135/TCP | MS RPC | Remote administration; follow negotiated dynamic ports. |
| 137-139 | NetBIOS | Legacy discovery, poisoning exposure, cross-segment use. |
| 161-162/UDP | SNMP | Default communities, scanning, unexpected writes. |
| 389,636/TCP | LDAP(S) | Reconnaissance, clear-text binds, unusual query volume. |
| 443/TCP,UDP | HTTPS/QUIC | SNI/certificate anomalies, rare destinations, periodicity. |
| 445/TCP | SMB | Admin shares, remote execution, fan-out, file transfer. |
| 500,4500/UDP | IKE/IPsec | Unapproved VPN peers and endpoint tunnels. |
| 1433/TCP | MS SQL | Exposure, discovery bursts, unusual clients. |
| 2049/TCP,UDP | NFS | Unexpected mounts and sensitive exports. |
| 3268,3269/TCP | AD Global Catalog | Directory discovery from unusual hosts. |
| 3389/TCP,UDP | RDP | New pairs, exposure, off-hours use, identity anomalies. |
| 5353,5355/UDP | mDNS/LLMNR | Cross-segment leakage and poisoning exposure. |
| 5985,5986/TCP | WinRM | Correlate caller, account, process, and target scope. |
| 6379/TCP | Redis | Unauthenticated exposure and unexpected clients. |
| 8080,8443/TCP | Alternate web | Shadow consoles, proxies, and tunneling. |

## Fast triage

1. Determine direction, result, duration, bytes, and listener versus connector.
2. Resolve the owning process, user, workload, and device role.
3. Compare historical prevalence; first-seen pairs often outperform rare ports.
4. Inspect protocol metadata and look for fan-out, periodicity, and abnormal transfer.

Use the live [IANA port registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) for authoritative assignments.

## Revision

| Revised Date | Comment |
| ------------ | ------- |
| 2025-03-16 | Article added |
| 2026-07-22 | Replaced static OS list with hunting-oriented reference |
