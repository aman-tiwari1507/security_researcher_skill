# Executive Summary Reference

Use this whenever the user asks for an executive summary, a simplified explanation, a "TL;DR", or after completing a deep-dive analysis. Produce a concise summary calibrated to the target audience — either a technical SME or a non-technical stakeholder.

---

## When to Produce an Executive Summary

- Explicitly requested ("give me an exec summary", "summarize this for my CEO", "TL;DR")
- After a deep-dive analysis, pentest report, threat model, or risk assessment
- When the user signals a non-technical audience ("explain this to my board", "my manager needs to understand this")
- When a complex finding needs to be communicated upward without losing its significance

---

## Audience Calibration

### Non-Technical Audience (Board, C-Suite, Business Stakeholders)
- Zero jargon. If a technical term must appear, define it in the same sentence.
- Lead with business impact: money, operations, reputation, legal/regulatory exposure.
- Avoid CVE numbers, CVSS scores, protocol names, or tool names — they create noise.
- Use plain analogies where helpful. One good analogy beats three paragraphs of explanation.
- Answer the three questions every non-technical reader has:
  1. **What happened / what is the risk?** (the plain-language problem statement)
  2. **Why does it matter to us?** (business consequence)
  3. **What are we doing about it?** (treatment, timeline, owner)
- Keep it to 150–250 words. If it's longer, it's not a summary.

### Technical SME (Senior Engineer, Security Lead, Architect)
- Assume full technical literacy — no need to define standard terms.
- Lead with the core finding or vulnerability class.
- Include: affected component, attack vector, exploitability, impact scope.
- Reference relevant standards, CVEs, or framework controls concisely.
- Highlight what's novel, unexpected, or counterintuitive — they already know the basics.
- Include the key remediation direction without over-specifying implementation.
- Keep it to 150–300 words. Dense is fine; vague is not.

---

## Output Templates

### Non-Technical Executive Summary

```
## Executive Summary — [Topic/Finding Title]
Audience: [Board / CEO / Business Stakeholder]

**What is the issue?**
[1–2 sentences in plain language. No acronyms.]

**Why does it matter?**
[Business consequence: financial exposure, operational disruption, regulatory risk, 
reputational impact. Be specific where possible — vague risk statements are ignored.]

**What is the likelihood?**
[Low / Medium / High — with one sentence of plain justification.]

**What are we doing about it?**
[Treatment action, owner, and timeframe. If no action yet, say so honestly.]

**Bottom line:**
[One sentence. The single most important thing they need to walk away knowing.]
```

### Technical SME Executive Summary

```
## Technical Summary — [Topic/Finding Title]
Audience: [Security Lead / Senior Engineer / Architect]

**Finding / Risk:**
[Vulnerability class, affected component, attack vector — 1–2 sentences.]

**Exploitability:**
[Who can exploit it, under what conditions, with what level of sophistication.]

**Impact:**
[Confidentiality / Integrity / Availability impact. Data at risk. Blast radius.]

**Key control gaps:**
[What's missing or misconfigured — the root cause, not just the symptom.]

**Recommended action:**
[Primary fix and any interim mitigations. Framework reference if relevant (e.g., CIS Control 7.4, NIST PR.IP-12).]

**Risk rating:** [Critical / High / Medium / Low] — [CVSS score if applicable]
```

---

## Writing Principles

**Lead with impact, not process.** Don't open with "We conducted an assessment of..." — open with what was found and why it matters.

**Be honest about severity.** Downplaying a critical finding to avoid alarm is worse than alarm. Executives make better decisions with accurate information.

**One owner, one action.** Every summary should end with a clear next step and a named owner. Summaries that end with "the team should consider..." achieve nothing.

**Avoid false precision.** Don't say "this will cost $4,237,500 if exploited" — say "potential financial exposure in the low-to-mid seven figures based on breach cost benchmarks." Ranges build more credibility than invented point estimates.

**Parallel structure for multiple findings.** If summarizing several findings, use the same structure for each so readers can scan and compare quickly.

---

## Example — Same Finding, Two Audiences

*Underlying finding: The company's customer portal is vulnerable to SQL injection, allowing unauthenticated extraction of the full customer database including names, emails, hashed passwords, and payment card metadata.*

### Non-Technical Version
**What is the issue?**
A security weakness in our customer-facing website could allow an attacker to access our entire customer database without needing a password or account.

**Why does it matter?**
This includes names, email addresses, and partial payment card information for all customers. A breach would trigger mandatory regulatory notification, potential fines under GDPR, and significant reputational damage at a time when customer trust is a key differentiator.

**Likelihood:** High — this type of vulnerability is actively exploited by automated tools and requires no specialist skill to trigger.

**What are we doing about it?**
The engineering team is deploying a fix within 48 hours. A temporary protective control (WAF rule) has been applied immediately. Full remediation is expected by [date].

**Bottom line:** This is our highest-priority security issue right now and is being treated as such.

---

### Technical SME Version
**Finding:** Unauthenticated SQL injection (error-based / UNION) in the `/api/v2/search` endpoint. Parameter `q` is passed unsanitized to a raw SQL query. Full database read access confirmed; write access not tested but likely given permission configuration.

**Exploitability:** No authentication required. Exploitable with standard tooling (sqlmap). Publicly accessible endpoint — any external attacker qualifies.

**Impact:** Full exfiltration of `customers` table (~2.3M rows): PII, bcrypt password hashes, last-four and card type metadata. Potential lateral movement to internal services via DB link if server permissions are misconfigured.

**Key control gap:** Raw string concatenation in the ORM fallback query path; parameterized queries not enforced at the framework layer. No WAF rule covering this endpoint prior to today.

**Recommended action:** Parameterize all queries in the affected module (primary fix). Audit remaining endpoints for same pattern. Enforce parameterized-query-only policy in code review gates going forward. Rotate DB credentials as precaution.

**Risk rating:** Critical — CVSS 9.8 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)
