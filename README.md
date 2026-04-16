# 🛡️ Security Researcher Skill

> A senior-level security researcher skill for Claude — spans offensive, defensive, GRC, and AI security. 

---

## ✨ What It Does

Turns Claude into a **senior security researcher** that reasons like both attacker and defender.

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

## 📦 Installing This Skill in Claude
 
You can install this skill on **Claude.ai** (web/desktop app) or in **Claude Code** (CLI). Both take about a minute.
 
### 🌐 Option A — Claude.ai (web or desktop)
 
**1. Download the skill folder.** Clone or download this repo as a ZIP from GitHub:
 
```bash
git clone https://github.com/aman-tiwari1507/security_researcher_skill.git
```
 
**2. Package it as a ZIP.** Zip the **`security-researcher/` folder itself**. When unzipped, you should see `security-researcher/SKILL.md`, not a loose `SKILL.md` at the root. This allows for the SKILL.md file along with references to get uploaded. 
 
**3. Upload to Claude.**
- Open Claude.ai → **Settings** → **Customize** → **Skills**
- Click the **`+`** button → **Create skill** → **Upload ZIP**
- Select your `security-researcher.zip`
  
**4. Toggle it on.** The skill appears in your Skills list. Flip the toggle and you're live.
 
**5. Test it.** Start a new chat and ask something security-flavoured — *"Review this login flow for auth issues"* or *"Build me a risk register for a fintech startup"*. The skill triggers automatically based on your prompt.
 
> **Note:** Skills require the feature to be enabled on your account. On Team / Enterprise plans, an admin may need to enable it at the org level first. Uploaded skills are private to your account unless you explicitly share them.
 
### 💻 Option B — Claude Code (CLI)
 
Drop the skill folder into your personal skills directory:
 
```bash
# Personal (available in every project)
mkdir -p ~/.claude/skills
cp -r security-researcher ~/.claude/skills/
 
# Or project-scoped (checked into the repo for your team)
mkdir -p .claude/skills
cp -r security-researcher .claude/skills/
```
 
Claude Code watches these directories and picks up the skill within the current session — no restart needed. Invoke it by typing `/security-researcher`.
 
### ✅ Quick checks if the skill isn't triggering
 
- **ZIP structure** — unzipping should give you a `security-researcher/` folder containing `SKILL.md`, not a loose `SKILL.md`
- **Name field** — the `name:` in `SKILL.md` frontmatter must be lowercase with hyphens only (`security-researcher` ✓, `Security Researcher` ✗)
- **Toggle** — confirm the skill is toggled **on** in your Skills list
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

| You say | You get |
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

## 📜 License & Disclaimer

Released under the [MIT License](LICENSE).

This skill provides defensive security education. Users are responsible for
ensuring authorization before testing any systems. The maintainers accept no
liability for misuse.

