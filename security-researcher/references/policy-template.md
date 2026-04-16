# Security Policy Template

Use this structure when drafting security policies, procedures, or standards. Adapt tone and depth to the organization's maturity and the policy's audience.

---

## Universal Policy Structure

### Header Block
```
Policy Title: [Name]
Policy ID: POL-XXX
Version: 1.0
Effective Date:
Review Date: [Annual by default]
Owner: [Role, not individual name]
Approved By:
Classification: Internal / Confidential
```

---

### 1. Purpose
One paragraph. Why does this policy exist? What risk does it address? What is the intent?

*Example: "This policy establishes requirements for managing user access to [Company] systems to protect against unauthorized access, data breaches, and insider threats. It supports [Company]'s obligations under [GDPR / SOC 2 / ISO 27001] and reflects our commitment to the principle of least privilege."*

---

### 2. Scope
Who and what does this policy apply to? Be explicit.

- **Applies to:** All employees, contractors, consultants, temporary staff, and third parties with access to [Company] systems
- **Covers:** All [Company] information systems, networks, cloud environments, applications, and data
- **Excludes:** [If applicable — e.g., personal devices used exclusively for personal purposes]

---

### 3. Policy Statements
The actual requirements. Write as "shall" (mandatory) vs "should" (recommended). Group by topic.

Use clear, unambiguous language. Avoid "may" and "might" for requirements.

*Example statements:*
- "All user accounts shall be provisioned through the approved access request process."
- "Accounts shall be reviewed quarterly for continued business need."
- "Privileged accounts shall not be used for routine non-privileged tasks."

---

### 4. Roles and Responsibilities

| Role | Responsibilities |
|------|----------------|
| Information Security Team | Policy ownership, exception management, compliance monitoring |
| IT / System Administrators | Technical implementation, access provisioning, audit log management |
| Managers / People Leaders | Approving access requests, notifying of role changes/terminations |
| All Employees | Compliance with policy, reporting violations |
| Third Parties | Adherence to policy terms within their access scope |

---

### 5. Procedures
Reference to detailed "how to" documents, or include inline for shorter policies.

- *How to request access:* [Link to procedure]
- *How to report a policy violation:* [Link to incident reporting]
- *Exception request process:* [Link or describe]

---

### 6. Exceptions
How are exceptions handled?

"Exceptions to this policy must be submitted to the Information Security team using the Exception Request Form. Exceptions require written approval from the CISO or designee, are time-limited (maximum 12 months), and must include documented compensating controls. All exceptions are tracked in the risk register."

---

### 7. Enforcement
"Violations of this policy may result in disciplinary action up to and including termination of employment or contract. Violations that constitute criminal activity will be reported to appropriate law enforcement authorities."

---

### 8. Related Documents
- [Policy name] — [POL-XXX]
- [Procedure name] — [PROC-XXX]
- [Standard name] — [STD-XXX]

---

### 9. Definitions / Glossary
Define any terms that might be ambiguous to the intended audience.

---

### 10. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | | | Initial release |

---

## Common Policy Types & Key Requirements

### Access Control Policy
Key areas: least privilege, need-to-know, access reviews, separation of duties, privileged access management, joiners/movers/leavers process, third-party access

### Password / Authentication Policy
Key areas: minimum length (≥12 chars), complexity, MFA requirements, password manager use, no password sharing, service account passwords, rotation for compromised credentials (not arbitrary periodic rotation per NIST 800-63B)

### Acceptable Use Policy (AUP)
Key areas: permitted and prohibited uses of company systems, monitoring notice, personal use provisions, data handling expectations, BYOD rules, social media

### Incident Response Policy
Key areas: what constitutes an incident, classification (P1–P4), notification timelines, roles (IR lead, legal, comms, exec), containment/eradication/recovery/lessons learned phases, external notification obligations (GDPR 72-hour rule, breach notification laws)

### Data Classification Policy
Key areas: classification tiers (Public / Internal / Confidential / Restricted), labeling requirements, handling requirements per tier, retention and disposal

### Vulnerability Management Policy
Key areas: scan frequency, SLA by severity (Critical: 24–72h, High: 7–14 days, Medium: 30–90 days), exceptions, patch testing process, compensating controls

### Third-Party / Vendor Security Policy
Key areas: security assessment before onboarding, contractual requirements (DPA, security addendum), ongoing monitoring, offboarding and data return/destruction

---

## Compliance Framework Quick Map

| Requirement | SOC 2 | ISO 27001 | NIST CSF | PCI-DSS |
|-------------|-------|-----------|----------|---------|
| Access control | CC6 | A.9 | PR.AC | Req 7,8 |
| Incident response | CC7 | A.16 | RS | Req 12.10 |
| Risk management | CC3,4 | Clause 6 | ID.RA | Req 12.2 |
| Encryption | CC6.7 | A.10 | PR.DS | Req 3,4 |
| Logging/monitoring | CC7.2 | A.12.4 | DE | Req 10 |
| Vendor management | CC9.2 | A.15 | ID.SC | Req 12.8 |
