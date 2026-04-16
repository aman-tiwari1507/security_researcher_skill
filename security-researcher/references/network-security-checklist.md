# Network Security Review Checklist

Use during network security assessments, architecture reviews, or when answering questions about network-layer security.

---

## Perimeter & Firewall
- [ ] Default deny inbound and outbound — only explicitly permitted traffic allowed
- [ ] Firewall rules reviewed for overly permissive rules (0.0.0.0/0, any/any)
- [ ] Egress filtering in place — not just ingress
- [ ] Management interfaces (SSH, RDP, web admin) not exposed to internet
- [ ] Unused ports and protocols disabled on all perimeter devices
- [ ] Firewall rules have documented business justification, owner, and review date
- [ ] Firewall logging enabled — both allowed and denied traffic

---

## Network Segmentation
- [ ] Network segmented into zones (DMZ, internal, guest, OT/IoT, management)
- [ ] Inter-zone traffic enforced by firewall, not just VLAN
- [ ] Flat network? (All hosts can reach all hosts = catastrophic lateral movement risk)
- [ ] Database servers in separate segment, not directly accessible from DMZ
- [ ] Jump host / bastion required to access sensitive segments
- [ ] Zero Trust Network Access (ZTNA) or software-defined perimeter where appropriate

---

## DNS Security
- [ ] Zone transfers restricted to authorized secondary servers only (AXFR)
- [ ] DNSSEC implemented and validated
- [ ] DNS logging enabled for detection of DNS tunneling (unusually large TXT records, high query volume to single domain)
- [ ] Internal DNS resolvers separate from external
- [ ] Recursive resolvers not open to internet (open resolver = DDoS amplification)
- [ ] DNS RPZ (Response Policy Zone) or DNS filtering for malware/C2 blocking
- [ ] DoH/DoT deployment considered (to prevent ISP eavesdropping)

---

## TLS/PKI
- [ ] TLS 1.2 minimum, TLS 1.3 preferred, older versions disabled
- [ ] Certificate expiry monitoring with automated alerting
- [ ] Internal PKI: root CA offline, intermediate CAs used for issuance
- [ ] Certificate pinning considered for high-value mobile/thick clients
- [ ] HSTS enforced on all public web services
- [ ] OCSP stapling enabled
- [ ] CAA (Certification Authority Authorization) DNS record set
- [ ] No self-signed certificates in production facing systems (except internal-to-internal where chain is managed)

---

## Remote Access / VPN
- [ ] MFA required for all remote access
- [ ] VPN client software patched (Pulse, Fortinet, GlobalProtect — historically high CVE count)
- [ ] Split tunneling: if enabled, what traffic bypasses VPN? Acceptable?
- [ ] Idle session timeout enforced
- [ ] VPN access logs reviewed — especially off-hours access, unusual geographies
- [ ] WireGuard or IKEv2/IPSec preferred over legacy SSL VPN where possible
- [ ] Clientless VPN: what's accessible through browser? Is it appropriately restricted?

---

## Wireless Security
- [ ] WPA3 or WPA2-Enterprise (802.1X) used — not WPA2-PSK for corporate
- [ ] Guest SSID isolated from corporate network
- [ ] PSK rotated on schedule (if WPA2-PSK used for IoT/legacy)
- [ ] Rogue AP detection enabled on wireless controller
- [ ] WPS disabled (vulnerable to brute force — Pixie Dust attack)
- [ ] Management interface not accessible over wireless
- [ ] PMKID harvesting mitigated (WPA3-SAE preferred)

---

## Network Monitoring & Detection
- [ ] IDS/IPS deployed: network-based (Zeek, Suricata) and/or host-based
- [ ] Full packet capture or flow logging (NetFlow/IPFIX) at key chokepoints
- [ ] Baselining: normal traffic patterns documented; alerting on deviation
- [ ] East-west traffic monitored (not just north-south)
- [ ] C2 detection: domain generation algorithm (DGA) detection, beaconing patterns, JA3 fingerprinting
- [ ] Log aggregation in SIEM with retention policy meeting compliance requirements
- [ ] Threat intel feeds integrated (IP/domain blocklists)

---

## Routing & Switching
- [ ] OSPF/BGP authentication configured (MD5 at minimum, keychain preferred)
- [ ] BGP route filtering: prefix lists, max-prefix limits, IRR validation, RPKI
- [ ] ARP inspection (DAI) enabled on access layer switches
- [ ] IP Source Guard enabled to prevent IP spoofing
- [ ] Port security or 802.1X on access ports
- [ ] STP security: BPDU guard on access ports, root guard on designated ports
- [ ] Storm control enabled to limit broadcast/multicast flood
- [ ] VLAN hopping mitigations: disable DTP, native VLAN != management VLAN

---

## Cloud Network Security (AWS/GCP/Azure)
- [ ] Security Groups / NSGs follow least privilege (no 0.0.0.0/0 on sensitive ports)
- [ ] No publicly exposed S3 buckets / Storage Accounts / GCS buckets
- [ ] VPC Flow Logs enabled
- [ ] Private endpoints used for PaaS services (no public internet traversal for DB, storage)
- [ ] EC2/VM instances without public IPs where possible; NAT gateway for outbound
- [ ] Network ACLs as secondary defense layer
- [ ] Cross-account network access reviewed
- [ ] AWS IMDSv2 enforced (instance metadata not accessible without token)
- [ ] Inter-region traffic encrypted

---

## DDoS Resilience
- [ ] DDoS mitigation service in place (AWS Shield Advanced, Cloudflare, Akamai, Fastly)
- [ ] Rate limiting at WAF/CDN layer
- [ ] Anycast routing for geographic load distribution
- [ ] Auto-scaling configured with DDoS budgets considered
- [ ] Upstream provider notified and plan in place for large volumetric attacks
- [ ] BGP blackholing capability available with upstream ISP
- [ ] Amplification sources mitigated: open DNS resolvers closed, NTP monlist disabled, memcached not exposed

---

## Physical & Out-of-Band
- [ ] Out-of-band management network separate from production
- [ ] Console access to network devices requires MFA
- [ ] Physical access to network infrastructure controlled and logged
- [ ] Network closets locked; cable management reviewed

---

## OSI Layer Quick Reference for Review Framing

| Layer | Name | Common Attacks |
|-------|------|---------------|
| 1 | Physical | Cable tapping, hardware implants |
| 2 | Data Link | ARP spoofing, VLAN hopping, MAC flooding, STP attacks |
| 3 | Network | IP spoofing, ICMP attacks, BGP hijacking, route injection |
| 4 | Transport | TCP session hijacking, SYN flood, port scanning |
| 5 | Session | Session replay, TLS downgrade |
| 6 | Presentation | SSL stripping, encoding attacks, compression side-channels |
| 7 | Application | Everything in the webapp checklist, DNS, HTTP attacks |
