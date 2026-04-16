# 🛡️ Security Researcher Skill

> A senior-level security researcher skill for Claude — spans offensive, defensive, GRC, and AI security. 

---

## ✨ What It Does

Turns Claude into a **senior security researcher** that reasons like both attacker and defender, then writes findings in clear human prose.

Covers eight domains:

| Domain | Scope |
|---|---|
| 🌐 **Web App Security** | XSS, SQLi, SSRF, XXE, IDOR, CSRF, JWT, OAuth, deserialization, API security |
| 🔌 **Network Security** | Recon, MITM, wireless, DDoS, firewalls, TLS, DNS, OSI/TCP-IP internals |
| ☁️ **Cloud & Infra** | AWS / GCP / Azure, containers, Kubernetes, CI/CD, IaC |
| 🔐 **Crypto & Code Review** | AES/RSA/ECC pitfalls, hashing, language-specific flaws, secrets, SCA |
| 📋 **GRC & Compliance** | ISO 27001, SOC 2, NIST CSF, PCI-DSS, GDPR, HIPAA, CIS Controls, CMMC |
| 🎯 **Risk & Vuln Mgmt** | Risk registers, CVSS/EPSS, SLA tiers, TPRM, audit readiness |
| 📝 **Documentation** | Pentest reports, threat models (STRIDE/PASTA), policies, exec summaries |
| 🤖 **AI Security** | Prompt injection, agentic AI, MCP, RAG, Claude Code, OWASP LLM Top 10 |

---

## 🚀 Usage

**Just ask.** The skill triggers automatically when your prompt matches a security domain.

### Quick examples

```text
"Review this login endpoint for auth flaws" → Web app review, narrative findings
"Build a risk register for a 50-person SaaS" → GRC mode, framework-mapped
"Threat model my agentic AI feature"         → STRIDE + AI-specific boundaries
"Write a pentest report for the VPN finding" → Formal report, exec + technical
"Is our S3 setup secure? [config pasted]"    → Direct review, real issues first
```

### How to get the best output

🎯 **Be specific about audience.** Say "for the board" or "for our engineers" and tone adjusts.

💻 **Paste actual code, configs, or logs.** The skill grounds findings in what you share.

🧭 **Signal the lens you want.** Phrases like *"how would an attacker…"*, *"help me understand…"*, or *"for an audit"* shift the framing.

📂 **Ask for a format directly.** "Give me a threat model" or "write an executive summary" routes to the right template.

---

## 🎨 Tone Calibration

The skill reads your phrasing and shifts tone automatically:

| You say… | You get… |
|---|---|
| *"explain to my team"* | Clear, structured, professional |
| *"help me understand"* | Warm, first-principles teaching |
| *"is this secure?"* + code | Direct, no-nonsense, real issues first |
| *"how would an attacker…"* | Technically precise, attacker mindset |
| CTF / challenge | Collaborative, step-by-step reasoning |
| Executive audience | Business-impact framed, no jargon |
| Compliance / audit | Formal, control-centric, framework-mapped |

---

## 📚 Reference Library

The skill auto-loads deeper reference material when relevant:

- `pentest-report-template.md` — structured pentest deliverables
- `threat-model-guide.md` — STRIDE, PASTA, data flow diagrams
- `webapp-checklist.md` — web app review coverage
- `network-security-checklist.md` — network review coverage
- `policy-template.md` — security policy drafting
- `risk-management.md` — risk identification, treatment, frameworks
- `grc-and-vuln-management.md` — compliance, vuln programs, TPRM
- `ai-security.md` — LLM, agentic AI, Claude Code deployments
- `executive-summary.md` — stakeholder-ready summaries

---

## ⚖️ Ethical Guardrails

This is a **defensive** security skill. Please use it to:

✅ Explain attack classes so you can defend against them <br>
✅ Review your code, configs, and architectures for real flaws <br>
✅ Produce reports, threat models, and remediation roadmaps <br>

Please don't use it to:

❌ Generate weaponized exploits for real production systems <br>
❌ Help attack systems you don't have authorization to test <br>
❌ Hand over ready-to-paste payloads divorced from defensive context <br>

