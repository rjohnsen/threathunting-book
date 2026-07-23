+++
title = "Intelligence Resources"
date = 2024-10-27T12:25:21+01:00
lastmod = 2026-07-23T00:00:00+02:00
weight = 10
chapter = false
+++

Threat intelligence is most useful when it adds context to an observation. A reputation result is not a verdict: infrastructure changes ownership, shared services host legitimate and malicious activity, and an indicator can outlive the campaign that created it.

{{% notice style="warning" title="Treat every external query as disclosure" %}}
Uploads expose the submitted artefact, but lookups may also reveal investigation targets or cause a provider to retrieve a submitted URL. Before querying a public service, check its sharing, retrieval, visibility, and retention behaviour. Use approved private services or local datasets when the subject is sensitive.
{{% /notice %}}

## A practical lookup sequence

1. Start with internal evidence: first and last seen, affected identities and assets, process ancestry, DNS, proxy, authentication, and file activity.
2. Form a question. For example: *Was this address associated with scanning at the time of the event?*
3. Where possible, use sources with distinct collection methods. Several services may repackage the same underlying feed.
4. Separate provider facts from your inference. “Flagged by 12 vendors at the time of lookup” is evidence; “therefore compromised” is a conclusion.
5. Retain time context. Registration, routing, DNS, certificates, reputation, and hosting can all change.
6. Save the query, timestamp, source, relevant result, confidence, and caveat in the case.

## Infrastructure and reputation

| Resource | Useful for | Caveat |
| --- | --- | --- |
| [VirusTotal](https://www.virustotal.com/) | Cross-provider enrichment for domains, IP addresses, URLs, and files | Vendor counts are neither prevalence nor proof; review relationships and observation dates |
| [GreyNoise](https://www.greynoise.io/) | Context for widespread internet scanning and opportunistic activity | Sensor coverage is incomplete; classifications do not determine the intent of your event |
| [AbuseIPDB](https://www.abuseipdb.com/) | Community reports associated with public IP addresses | Report quality and age vary; addresses can change owner |
| [Censys Search](https://search.censys.io/) | Internet-facing services, certificates, and historical exposure | Describes observations in time, not current owner intent |
| [Shodan](https://www.shodan.io/) | Exposed services, banners, products, and historical observations | Banners can be stale, misleading, proxied, or shared |
| [urlscan.io](https://urlscan.io/) | Page behaviour, redirects, requests, screenshots, and infrastructure | A scan can disclose a private or token-bearing URL |
| [Mnemonic Passive DNS](https://passivedns.mnemonic.no/) | Historical DNS relationships | Absence reflects collection coverage, not proof that a relationship never existed |
| [RIPEstat](https://stat.ripe.net/) | ASN, routing, prefix, and registration context | Network registration does not identify a workload operator |
| [ICANN Lookup](https://lookup.icann.org/) | RDAP-backed domain registration data | Privacy services and redaction limit attribution |
| [crt.sh](https://crt.sh/) | Certificate Transparency searches and certificate pivots | Certificate presence does not prove a host is active or controlled by the named subject |

## Malware, URLs, and indicators

| Resource | Useful for | Caveat |
| --- | --- | --- |
| [MalwareBazaar](https://bazaar.abuse.ch/) | Malware samples, hashes, tags, and family context | Community and vendor labels require validation |
| [URLhaus](https://urlhaus.abuse.ch/) | URLs observed distributing malware | Coverage reflects submissions and collection, not all malicious URLs |
| [ThreatFox](https://threatfox.abuse.ch/) | Community indicators associated with malware | Review confidence, age, and the relationship claimed |
| [VirusTotal](https://www.virustotal.com/) | Hash lookup, relationships, and behaviour summaries | Do not upload sensitive samples to public analysis |
| [ANY.RUN](https://any.run/) | Interactive behavioural analysis | Public tasks may expose samples and artefacts |
| [Joe Sandbox](https://www.joesandbox.com/) | Static and dynamic analysis across several platforms | Results depend on configuration, execution path, and sandbox visibility |

## Vulnerability exploitation context

| Resource | Useful for | Caveat |
| --- | --- | --- |
| [CISA Known Exploited Vulnerabilities](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) | Vulnerabilities with evidence of exploitation in the wild | Inclusion supports prioritisation; absence does not mean no exploitation |
| [FIRST EPSS](https://www.first.org/epss/) | Daily probability that a published CVE will be exploited in the wild in the next 30 days | A model score is not evidence that your asset was targeted or compromised |
| [NVD](https://nvd.nist.gov/) | CVE, CPE, CVSS, references, and enrichment | CVSS measures severity, not local exposure or exploitation likelihood |

## Research, sharing, and intelligence management

| Resource | Useful for |
| --- | --- |
| [MITRE ATT&CK](https://attack.mitre.org/) | Shared vocabulary for behaviours, software, groups, mitigations, and data sources |
| [CISA Cybersecurity Advisories](https://www.cisa.gov/news-events/cybersecurity-advisories) | Authoritative advisories and defensive guidance |
| [Microsoft Threat Intelligence](https://www.microsoft.com/en-us/security/security-insider/intelligence-reports) | Campaign reporting with Microsoft telemetry context |
| [Google Threat Intelligence Group](https://cloud.google.com/blog/topics/threat-intelligence) | Campaign, malware, vulnerability, and actor research |
| [Palo Alto Networks Unit 42](https://unit42.paloaltonetworks.com/) | Incident and campaign research |
| [The Shadowserver Foundation](https://www.shadowserver.org/) | Exposure and infection reporting for network owners |
| [MISP](https://www.misp-project.org/) | Structured sharing and correlation within a controlled community |
| [OpenCTI](https://www.opencti.io/) | Modelling and managing intelligence in a knowledge graph |

## Useful pivot types

| Pivot | What it may connect | Common trap |
| --- | --- | --- |
| Passive DNS | Domain, address, and time | Treating absence as proof |
| Certificate Transparency | Names, certificates, and issuance | Assuming issuance proves deployment or control |
| RDAP and routing | Registrant, ASN, prefix, and dates | Attributing a hosted workload to the network owner |
| File relationships | Hash, signer, filename, archive, and behaviour | Trusting family labels without validating the sample |
| URL relationships | Redirects, path, referrer, and retrieved resources | Triggering retrieval or disclosing tokens during lookup |
| Campaign reporting | Behaviour, infrastructure, malware, and time | Forcing weak similarities into actor attribution |

## What to record in a case

| Field | Example |
| --- | --- |
| Indicator | `203.0.113.24` |
| Lookup time | `2026-07-23 08:42 UTC` |
| Internal context | First seen contacting one workstation after a document opened |
| External context | Two differently sourced datasets associated the address with scanning that week |
| Confidence | Moderate |
| Caveat | Shared hosting; no payload or operator attribution |
| Next pivot | DNS history, certificate reuse, process and file activity |

## Revision

| Revised Date | Comment |
| --- | --- |
| 2024-10-27 | Added page |
| 2026-07-23 | Reworked as a disclosure-aware, investigation-focused resource guide |
