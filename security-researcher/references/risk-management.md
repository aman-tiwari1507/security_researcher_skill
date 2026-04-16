# Risk Management Reference

Use when performing risk identification, risk assessment, building risk registers, advising on risk treatment, or helping an organization understand and communicate its risk posture.

---

## Risk Fundamentals

### Core Definitions
- **Risk:** The possibility that an event will occur and adversely affect the achievement of objectives. Risk = Likelihood × Impact.
- **Inherent risk:** The risk that exists before any controls are applied.
- **Residual risk:** The risk that remains after controls are applied.
- **Risk appetite:** The amount and type of risk an organization is *willing to accept* in pursuit of its objectives. Set by the board.
- **Risk tolerance:** The acceptable *deviation* from risk appetite. More operational — how far from appetite before escalation is required?
- **Risk threshold:** The point at which risk becomes unacceptable and requires escalation.

The distinction between appetite and tolerance matters in practice. A company might have a low risk appetite for data breaches (appetite), but tolerate a residual risk of medium probability of a breach on non-critical systems given cost constraints (tolerance within appetite).

### Risk Treatment Options (ISO 31000)
Every identified risk must be assigned a treatment:

| Treatment | Description | When to use |
|-----------|-------------|------------|
| **Mitigate** | Reduce likelihood or impact through controls | Most common; when controls are cost-effective |
| **Accept** | Acknowledge and monitor; no additional controls | Residual risk is within appetite; cost of control exceeds benefit |
| **Transfer** | Shift financial impact (insurance) or operational responsibility (outsourcing) | High-impact, low-frequency risks; third-party better positioned |
| **Avoid** | Stop the activity that creates the risk | Risk is outside appetite and cannot be adequately mitigated |

Risk acceptance is not the same as ignoring a risk. Accepted risks must be documented, owned, time-bounded, and reviewed.

---

## Risk Identification Techniques

### Structured Techniques
**Threat modeling (STRIDE):** See `references/threat-model-guide.md`. Best for technical risks in specific systems.

**Bow-tie analysis:** Maps threat sources → hazard → top event → consequences, with preventive controls on the left (barriers to the event) and mitigating controls on the right (barriers to consequences). Excellent for visualizing control gaps.

**SWOT / risk workshops:** Facilitated sessions with stakeholders to identify risks across people, process, technology, and external factors. Use structured prompts: "What could prevent us from achieving X?" "What would cause a significant incident?"

**Risk taxonomy:** Start from a structured list of risk categories to ensure comprehensive coverage:
- Strategic (third-party dependency, M&A integration, market change)
- Operational (process failure, human error, supply chain)
- Financial (fraud, incorrect data, audit failure)
- Compliance (regulatory change, contractual breach)
- Reputational (data breach, insider misconduct)
- Technology (system failure, cyber attack, technical debt)

**Historical analysis:** What incidents, near-misses, or audit findings have occurred in the last 1–3 years? These are the best predictors of future risk.

**External threat intelligence:** What are peers in the same industry experiencing? ISAC (Information Sharing and Analysis Center) reports, DBIR (Verizon Data Breach Investigations Report), ENISA threat landscape reports.

---

## Risk Assessment Methodologies

### Qualitative Assessment (most common)
Uses descriptive scales — straightforward, faster, works well when quantitative data is unavailable.

**Likelihood scale:**
| Score | Label | Description |
|-------|-------|-------------|
| 1 | Rare | May occur only in exceptional circumstances (<1% / year) |
| 2 | Unlikely | Could occur at some time (1–10% / year) |
| 3 | Possible | Might occur at some time (10–50% / year) |
| 4 | Likely | Will probably occur in most circumstances (50–90% / year) |
| 5 | Almost certain | Expected to occur in most circumstances (>90% / year) |

**Impact scale:**
| Score | Label | Financial | Operational | Reputational |
|-------|-------|-----------|-------------|-------------|
| 1 | Negligible | <$10K | No disruption | No coverage |
| 2 | Minor | $10K–$100K | Brief disruption | Limited coverage |
| 3 | Moderate | $100K–$1M | Significant disruption | Negative media |
| 4 | Major | $1M–$10M | Prolonged disruption | Serious reputational damage |
| 5 | Catastrophic | >$10M | Business survival at risk | Regulatory action, existential |

**Risk score = Likelihood × Impact (1–25)**
- 1–4: Low (monitor)
- 5–9: Medium (manage and mitigate)
- 10–15: High (senior attention, treatment plan required)
- 16–25: Critical (board visibility, immediate treatment)

### Semi-Quantitative / Quantitative Assessment
Used when the risk justifies the investment in data collection.

**Annual Loss Expectancy (ALE):**
```
ALE = Single Loss Expectancy (SLE) × Annual Rate of Occurrence (ARO)
SLE = Asset Value × Exposure Factor (% of asset lost in event)
```

Example: Asset value $2M, exposure factor 40%, probability of breach 0.1/year
→ SLE = $800K, ARO = 0.1, ALE = $80K/year
→ Controls costing <$80K/year are financially justified

**Limitations:** SLE, ARO, and exposure factor are estimates, and small errors compound. Use ranges, not point estimates, and communicate uncertainty explicitly.

**Monte Carlo simulation:** For complex risk portfolios, model each variable as a probability distribution (not a point estimate) and run thousands of simulations to produce a probability distribution of outcomes. Tools: @RISK, Palisade, Crystal Ball, or Python (scipy, numpy).

---

## Risk Register

### Structure
A risk register is a living document — not a one-time deliverable. It should be reviewed quarterly at minimum, and after any significant incident, audit, or architecture change.

```
Risk ID        | RISK-2024-001
Title          | Exploitation of unpatched critical vulnerability in internet-facing API
Category       | Technology / Cyber
Description    | Organization runs a customer-facing API with a 30-day patching SLA. 
               | Critical CVEs may be exposed for up to 30 days post-disclosure.
Threat Source  | External attacker (opportunistic + targeted)
Trigger Event  | Publication of critical CVE affecting API framework
Inherent Risk  | Likelihood: 4 (Likely) × Impact: 5 (Catastrophic) = 20 (Critical)
Controls       | WAF, vulnerability scanning (weekly), patch management process,
               | runtime application self-protection (RASP)
Residual Risk  | Likelihood: 2 (Unlikely) × Impact: 4 (Major) = 8 (Medium)
Treatment      | Mitigate — reduce SLA to 72h for critical CVEs on internet-facing systems
Owner          | Head of Infrastructure
Review Date    | 2025-Q1
Status         | In progress — SLA policy update in draft
```

### Risk Heat Map
Visualize the portfolio at a glance. Plot each risk on a 5×5 grid (likelihood vs impact). Group by:
- Pre-control (inherent): Shows raw exposure
- Post-control (residual): Shows actual posture
- Target state: Where you want to be after treatment

Color zones: Green (1–4), Yellow (5–9), Amber (10–15), Red (16–25)

---

## Key Risk Indicators (KRIs)

KRIs are leading indicators that signal risk *before* it becomes an incident. Unlike KPIs (which measure performance), KRIs measure risk exposure.

| Risk | KRI | Threshold (escalate if...) |
|------|-----|--------------------------|
| Unpatched vulnerabilities | % of critical vulns past SLA | >0% for 7+ days |
| Insider threat | % of users with excessive privilege | >5% of workforce |
| Third-party risk | % of Tier 1 vendors with no SOC 2 | >10% |
| Phishing | Click rate in phishing simulations | >10% |
| Change management | % of unauthorized changes | >2% of total changes |
| Data loss | DLP policy violations per week | Trend increase >20% |

KRIs should be reported to security leadership monthly and to the board/risk committee quarterly, with trend lines showing whether risk is increasing, stable, or decreasing.

---

## Risk Communication to Different Audiences

### Board / Executive Committee
- Lead with: "Our top 3 risks are X, Y, Z. Here is their current status and trend."
- Use: Heat map, risk register summary (top 10), financial exposure estimates
- Avoid: Technical jargon, CVSS scores, vulnerability counts without business context
- Frame treatment as investment: "Mitigating risk X will reduce expected annual loss by $X at a control cost of $Y."

### CISO / Security Leadership
- Full risk register with scoring details
- KRI dashboard with trend lines
- Residual risk vs appetite gap analysis
- Prioritized treatment roadmap with resource requirements

### Risk/Audit Committee
- Formal risk reporting pack: inherent vs residual, treatment status, exceptions outstanding
- Compliance attestation status
- Internal audit findings and management responses
- Third-party risk summary

### Operational Teams
- Specific risks they own, with clear treatment actions
- SLA performance vs target
- Escalation paths when risks change

---

## NIST Risk Management Framework (RMF) — SP 800-37

For organizations aligned to NIST (federal agencies, contractors, and those who adopt NIST as a baseline):

**7 Steps:**
1. **Prepare** — establish context, priorities, supply chain risk management
2. **Categorize** — categorize information and system (FIPS 199 impact levels: Low, Moderate, High)
3. **Select** — choose appropriate controls from NIST SP 800-53
4. **Implement** — deploy controls, document implementation
5. **Assess** — evaluate whether controls are implemented correctly and effective
6. **Authorize** — AO (Authorizing Official) makes a risk-based authorization decision (ATO)
7. **Monitor** — continuously monitor control effectiveness, report status, respond to changes

**ATO (Authority to Operate):** The formal approval to operate a system. Based on the residual risk being acceptable. Not permanent — systems must be continuously monitored and periodically reassessed.
