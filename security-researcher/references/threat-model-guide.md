# Threat Modeling Guide

Use this when asked to perform, facilitate, or document a threat model. Adapt to the system's complexity and the user's familiarity level.

---

## When to Use Which Framework

| Framework | Best for |
|-----------|----------|
| **STRIDE** | General application/system threat modeling — most widely used |
| **PASTA** | Risk-centric, business-aligned threat modeling |
| **LINDDUN** | Privacy threat modeling |
| **Attack Trees** | Specific scenario deep-dives, CTF, red team planning |
| **MITRE ATT&CK** | Enterprise infrastructure, incident response mapping |

---

## STRIDE Threat Modeling Process

### Step 1 — Define Scope and Objectives

Answer these before starting:
- What system / feature / service are we modeling?
- What are we trying to protect? (assets)
- Who cares if it fails? (stakeholders and impact)
- What's in-scope vs out-of-scope?

---

### Step 2 — Build the Data Flow Diagram (DFD)

A DFD shows how data moves through the system. Elements:

| Symbol | Meaning | Security relevance |
|--------|---------|-------------------|
| Rectangle | External entity (user, 3rd party) | Trust boundary edge |
| Rounded box | Process (application, service) | Logic to review |
| Parallel lines | Data store (DB, cache, file) | Confidentiality/integrity target |
| Arrow | Data flow | Eavesdropping, tampering |
| Dashed line | Trust boundary | Auth/authz enforcement point |

**Draw or describe:**
1. External actors (users, APIs, external services)
2. Entry points into the system
3. Internal processes and services
4. Data stores
5. Trust boundaries (where privilege or ownership changes)
6. Exit points / external outputs

---

### Step 3 — Apply STRIDE Per Element

For **each DFD element**, systematically ask:

#### S — Spoofing Identity
*Can an attacker pretend to be someone or something they're not?*
- Is authentication enforced at this point?
- Can sessions be hijacked or forged?
- Are API keys or tokens validated?

**Example threats:** JWT alg:none, session fixation, ARP spoofing, DNS spoofing

#### T — Tampering with Data
*Can an attacker modify data in transit or at rest?*
- Is integrity protection in place (HMAC, digital signatures, checksums)?
- Are inputs validated and sanitized?
- Is the data store write-protected appropriately?

**Example threats:** SQLi, parameter tampering, log injection, CSRF, MITM

#### R — Repudiation
*Can an attacker deny performing an action?*
- Is there sufficient, tamper-evident audit logging?
- Are logs stored separately and protected?
- Do logs capture the right identity context?

**Example threats:** Log wiping, action without auth attribution, unsigned transactions

#### I — Information Disclosure
*Can an attacker access data they shouldn't?*
- Is data encrypted in transit and at rest?
- Are error messages leaking internal information?
- Is access control enforced at every layer?

**Example threats:** XXE, IDOR, verbose errors, directory listing, SSRF, path traversal

#### D — Denial of Service
*Can an attacker make the system unavailable?*
- Are resource limits enforced (rate limiting, connection limits)?
- Are expensive operations protected?
- Are there single points of failure?

**Example threats:** ReDoS, XML bomb, DDoS, lock exhaustion, resource starvation

#### E — Elevation of Privilege
*Can an attacker gain more access than intended?*
- Are authorization checks enforced after authentication?
- Can users access admin functions?
- Can user-supplied input reach privileged operations?

**Example threats:** SQL injection to admin, SSTI to RCE, container escape, SSRF to metadata

---

### Step 4 — Rate and Prioritize Threats

For each identified threat, assess:

**DREAD scoring (optional, qualitative):**
| Factor | 1 (Low) | 5 (High) |
|--------|---------|---------|
| Damage | Minor inconvenience | Full system compromise |
| Reproducibility | Rare conditions | Always reproducible |
| Exploitability | Expert attacker | Script kiddie |
| Affected users | One user | All users |
| Discoverability | Internal knowledge | Publicly known |

Or simply: **Critical / High / Medium / Low** with a one-sentence justification.

---

### Step 5 — Define Mitigations

For each threat, specify:
- **Mitigation**: The control that addresses the threat
- **Layer**: Where it's applied (code, infrastructure, process)
- **Owner**: Who implements it
- **Status**: Not started / In progress / Done / Accepted risk

---

### Step 6 — Document and Iterate

The threat model is a living document. Revisit when:
- New features are added
- Architecture changes
- New vulnerability classes emerge
- Post-incident (did the threat model miss it?)

---

## Threat Model Output Template

```
# Threat Model: [System Name]
Date: 
Version:
Authors:
Reviewers:

## 1. System Overview
[What does this system do? Who uses it?]

## 2. Scope
In-scope: 
Out-of-scope:
Assets being protected:

## 3. Data Flow Diagram
[Embed or describe the DFD]
Trust boundaries identified:
- [List]

## 4. Threat Analysis

### [FIND-TM-001] [Threat Title]
Category: STRIDE category
Component: [DFD element affected]
Description: [How does this threat manifest?]
Impact: [What could an attacker achieve?]
Likelihood: High / Medium / Low
Severity: Critical / High / Medium / Low
Mitigation: [What control addresses this?]
Status: [Not started / Implemented / Accepted]

[Repeat for each threat]

## 5. Risk Summary

| ID | Threat | Severity | Mitigation Status |
|----|--------|----------|------------------|

## 6. Open Questions / Assumptions
[Things that need clarification or were assumed during the model]
```

---

## MITRE ATT&CK Mapping (Enterprise)

When mapping findings to ATT&CK, use:
- **Tactic**: The adversary's goal (e.g., Initial Access, Privilege Escalation)
- **Technique**: How they achieve it (e.g., T1190 Exploit Public-Facing Application)
- **Sub-technique**: More specific variant (e.g., T1059.001 PowerShell)

Useful for: incident response, detection engineering, red team planning, board-level risk communication.
