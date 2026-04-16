# GRC & Vulnerability Management Reference

Use this when the task involves governance, risk and compliance programs, vulnerability management lifecycle, compliance frameworks, gap analysis, audit readiness, or TPRM.

---

## Governance

### Security Program Structure
A mature security program has a clear hierarchy: the Board or Risk Committee sets appetite, the CISO translates that into program strategy, and operational teams execute. The policy hierarchy flows downward:

```
Policy       → The "what and why" (e.g., Access Control Policy)
Standard     → The "how much" (e.g., passwords must be ≥14 chars, MFA required)
Procedure    → The "how to" (step-by-step, e.g., onboarding access provisioning)
Guideline    → Advisory best practice (e.g., recommended IDE security settings)
```

Policies are mandatory. Standards are mandatory. Procedures are mandatory. Guidelines are recommended. This distinction matters enormously during audits and incident investigations.

### Board-Level Risk Communication
Boards don't speak CVSSv3. They speak business impact, likelihood, and cost. When communicating upward:
- Express risk in financial or operational terms ("unpatched critical vulnerability on payment processor = potential $X breach cost, X days downtime")
- Use a simple 5×5 heat map (likelihood × impact), not a 50-row risk register
- Lead with top 3–5 risks and their treatment status
- Include metrics trend lines, not just point-in-time numbers

---

## Compliance Frameworks — Deep Reference

### ISO 27001:2022
Structure: 10 clauses (4–10 are mandatory) + Annex A (93 controls, 4 themes).

**Annex A themes (2022 revision):**
- Organizational controls (37) — policies, roles, threat intelligence, supplier security
- People controls (8) — background checks, awareness, remote working
- Physical controls (14) — physical security perimeters, clear desk
- Technological controls (34) — authentication, malware protection, logging, cryptography

**Statement of Applicability (SoA):** Every Annex A control must be listed. For each: applicable or not, justification for exclusion, implementation status. Auditors scrutinize this heavily.

**Certification process:** Stage 1 (document review) → Stage 2 (on-site audit) → Surveillance audits annually → Recertification every 3 years.

### SOC 2
Not a certification — it's an attestation. A licensed CPA firm opines on whether controls meet Trust Services Criteria.

**Type I:** Point-in-time. "Controls are suitably designed." Easier, faster. Less valuable to enterprise buyers.
**Type II:** Period of time (typically 6–12 months). "Controls operated effectively." What most enterprise contracts require.

**Five Trust Services Criteria:**
- **CC** (Common Criteria / Security) — always required, covers 9 categories including logical access, change management, risk assessment
- **Availability** — system uptime, incident response, capacity management
- **Confidentiality** — data classification, encryption, disposal
- **Processing Integrity** — complete, valid, accurate, timely processing
- **Privacy** — AICPA privacy notice, consent, data subject rights (aligns with GDPR concepts)

**Audit evidence examples:** access review logs, change tickets, vulnerability scan reports, security awareness training records, vendor risk assessments.

### NIST CSF 2.0
Six functions (the 2.0 addition is **Govern**):

| Function | Purpose | Example categories |
|----------|---------|-------------------|
| Govern | Oversee and contextualize | Risk governance, supply chain risk, policy |
| Identify | Understand assets and risk | Asset management, risk assessment, improvement |
| Protect | Safeguard capabilities | Identity mgmt, awareness, data security, config |
| Detect | Find anomalies | Monitoring, event analysis |
| Respond | Act on incidents | Incident management, analysis, mitigation, comms |
| Recover | Restore capabilities | Recovery planning, improvements, comms |

CSF is voluntary, non-prescriptive, and outcome-based. It's excellent for maturity modeling and communicating security posture to executives.

### PCI-DSS v4.0 (key changes from v3.2.1)
- Targeted risk analysis now required for many controls (justify *why* your frequency/scope is appropriate)
- MFA required for all access into the CDE, not just remote access
- Password minimum length increased to 12 characters
- Phishing-resistant MFA (passkeys, hardware tokens) encouraged for privileged access
- New requirements for e-commerce (inline scripts, script integrity, payment page monitoring)
- Software security: adds secure software development requirements (Req 6)

**CDE (Cardholder Data Environment):** Where PANs are stored, processed, or transmitted. Scope reduction is the most impactful thing an org can do — tokenization, point-to-point encryption (P2PE), and network segmentation all reduce scope.

### GDPR — Key Security Obligations
- **Article 32:** Implement appropriate technical and organizational measures — encryption, confidentiality, integrity, availability, resilience, ability to restore access
- **Article 33:** Notify supervisory authority within 72 hours of becoming aware of a personal data breach
- **Article 34:** Notify affected individuals without undue delay if high risk to their rights
- **DPIA (Data Protection Impact Assessment):** Required for high-risk processing (large-scale sensitive data, systematic monitoring, automated decision-making). Must include risk description, measures to address risk, DPO consultation.

### CIS Controls v8 — Three Implementation Groups
- **IG1** (essential hygiene, all orgs): Inventory of assets, software inventory, data protection, secure config, account management, access control, continuous vuln management, audit log management, email/browser protection, malware defense, network monitoring
- **IG2** (adds): Data recovery, network infrastructure management, security awareness, service provider management, application software security, incident response
- **IG3** (adds):** Penetration testing, security awareness advanced

---

## Vulnerability Management Program

### Full Lifecycle

**1. Discover**
Asset inventory is the foundation — you can't protect what you don't know exists. Sources: CMDB, cloud asset APIs (AWS Config, Azure Resource Graph), passive network discovery, certificate transparency, external attack surface management (EASM) tools (Censys, Shodan, runZero).

**2. Assess**
Scan regularly. Frequency by asset criticality:
- Internet-facing / critical: continuous or weekly
- Internal servers: monthly
- Endpoints: monthly
- After significant changes: always

Tools: Tenable Nessus/IO, Qualys, Rapid7 InsightVM, OpenVAS (open-source). Authenticated scans are essential — unauthenticated scans miss the majority of software vulnerabilities.

**3. Prioritize**
CVSS alone is a poor prioritization signal — it measures severity in a vacuum, not exploitability in your environment. Use a layered model:

```
Priority Score = f(CVSS Base Score, EPSS Score, Asset Criticality, Exposure, Threat Intel)
```

**EPSS (Exploit Prediction Scoring System):** Probability (0–1) that a CVE will be exploited in the wild within 30 days. A CVE with CVSS 9.8 but EPSS 0.002 is lower priority than CVSS 7.1 with EPSS 0.94 and internet exposure.

**Asset criticality:** Crown jewels (payment systems, AD, prod DBs) warrant faster SLAs. A critical vuln on a dev sandbox is different from one on an internet-facing API.

**Threat intelligence correlation:** Is this CVE being actively exploited? Is there a PoC? Is it in CISA KEV (Known Exploited Vulnerabilities catalog)?

**4. Remediate**
SLA tiers (adjust to org risk appetite):

| Severity | Typical SLA | Notes |
|----------|------------|-------|
| Critical (CVSS 9.0+, actively exploited) | 24–72 hours | Escalate immediately; consider isolation |
| High (CVSS 7.0–8.9) | 7–14 days | |
| Medium (CVSS 4.0–6.9) | 30–60 days | |
| Low (CVSS <4.0) | 90–180 days | |

Remediation isn't always patching: virtual patching (WAF rule), network segmentation, configuration change, or compensating control can be valid interim measures.

**5. Verify**
Rescan after remediation. Don't close tickets based on the patch being applied — confirm the vulnerability is gone. False closures inflate metrics and create real exposure.

**6. Report**
Key VM metrics:
- Mean Time to Remediate (MTTR) by severity
- % of assets scanned in last 30 days
- % of critical/high vulns within SLA
- Vulnerability density by business unit / asset type
- Trend over time (are we improving?)
- Exceptions outstanding and aging

### Exception / Risk Acceptance Process
When remediation is not feasible within SLA:
1. Document: CVE, affected asset, business justification, compensating controls
2. Risk owner (asset owner or business unit head) signs off
3. Security team reviews and approves or escalates
4. Exception logged with expiry date (max 12 months, reviewed quarterly)
5. Tracked in risk register

---

## Gap Analysis

### Process
1. Define target framework (ISO 27001, SOC 2, NIST CSF, etc.)
2. Map existing controls to framework requirements
3. For each requirement: Met / Partially Met / Not Met / Not Applicable
4. For gaps: document current state, target state, owner, effort, priority
5. Produce remediation roadmap with timelines and milestones

### Cross-Framework Control Mapping
Many controls satisfy multiple frameworks simultaneously. Use a mapping matrix when building a unified control library:

| Control | ISO 27001 | SOC 2 CC | NIST CSF | PCI-DSS |
|---------|-----------|----------|----------|---------|
| Vulnerability scanning | A.8.8 | CC7.1 | DE.CM-8 | Req 11.3 |
| MFA | A.8.5 | CC6.1 | PR.AC-7 | Req 8.4 |
| Security awareness training | A.6.3 | CC2.2 | PR.AT-1 | Req 12.6 |
| Incident response plan | A.5.26 | CC7.3 | RS.RP | Req 12.10 |
| Encryption at rest | A.8.24 | CC6.7 | PR.DS-1 | Req 3.5 |

---

## Third-Party Risk Management (TPRM)

### Vendor Tiering
Not all vendors deserve the same scrutiny. Tier by:
- Data access: Do they process, store, or transmit your sensitive data?
- System access: Do they have network or system access?
- Business criticality: Would a breach or outage cause significant harm?

**Tier 1 (Critical):** Full security assessment, SOC 2 Type II review, contractual security addendum, annual reassessment, right to audit
**Tier 2 (High):** Security questionnaire (SIG Lite or CAIQ), SOC 2 review if available, DPA, annual review
**Tier 3 (Low):** Standard contract terms, self-attestation, periodic reassessment

### Reading a SOC 2 Report
Key sections to review:
1. **Auditor's opinion:** Qualified (controls not effective) vs unqualified (clean). Qualified opinions are significant.
2. **Description of system:** Is the scope what you expected?
3. **Complementary User Entity Controls (CUECs):** Controls the vendor expects *you* to implement. Many buyers miss these — if you don't implement them, the SOC 2 coverage doesn't apply.
4. **Exceptions:** Any control failures noted? How did the vendor respond?
5. **Subservice organizations:** Who does the vendor rely on? Are they carved in or carved out of scope?
