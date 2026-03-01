# Caelguard Community -- Free Agent Security Tools

> **Your OpenClaw agent is running in the most targeted AI ecosystem on the internet.** Here are the numbers.

| Threat | Scale | Source |
|--------|-------|--------|
| Malicious skills on ClawHub | **1,184+ confirmed** | Snyk, Bitdefender, Koi, Straiker, Trend Micro, Repello AI |
| ClawHub skills with security flaws | **36.82%** (1,467 of 3,984 scanned) | Snyk ToxicSkills Report |
| Internet-exposed OpenClaw instances | **42,000+** | Censys / Bitsight |
| CVEs disclosed in 2026 | **6** (including one-click RCE, CVSS 8.8) | NVD |
| Active malware campaigns | **4 distinct threat clusters** | Bitdefender |
| Malicious skills combining multiple attack types | **91%** | Caelguard Research |
| Static analysis detection rate vs reasoning-layer attacks | **0%** | Caelguard Research |

These are not theoretical risks. These are documented, in-the-wild attacks happening right now. [Read the full research.](https://caelguard.tech/blog/state-of-openclaw-security.html)

## What's in this repo

Three free tools. No API keys. No external dependencies. Python 3.8+.

```bash
git clone https://github.com/Justincredible-tech/caelguard-community.git
cd caelguard-community
```

To use as an OpenClaw skill:
```bash
cp -r caelguard-community ~/.openclaw/workspace/skills/shellguard
```

---

### ShellGuard Scanner v1.1.0

Three-tier threat detection for OpenClaw skills. The only scanner that catches reasoning-layer attacks (tool poisoning, tool shadowing, memory poisoning) that static analysis misses entirely.

```bash
# Scan a single skill
python3 scripts/shellguard-scanner.py /path/to/skill/

# Scan ALL installed skills
python3 scripts/shellguard-scanner.py --all-installed

# JSON output for automation
python3 scripts/shellguard-scanner.py --all-installed --json
```

Every skill gets a **Suspicion Index** (0-100):

| Score | Rating | What to do |
|-------|--------|------------|
| 0-20 | Clean | Likely safe |
| 21-40 | Low Risk | May be legitimate, review if concerned |
| 41-60 | Medium Risk | Manual review recommended |
| 61-80 | High Risk | Likely malicious. Do not use. |
| 81-100 | Critical | Confirmed malicious patterns. Remove immediately. |

**Detection capabilities (v1.1.0):**
- **Tier 1 -- Pattern Matching:** Reverse shells (bash, Python, Ruby, Perl, PowerShell), credential access, C2 indicators, base64 payloads, known-bad IOCs
- **Tier 2 -- Behavioral Analysis:** Compound patterns (credential access + network calls), Unicode deception (homoglyphs, zero-width chars, BIDI overrides), social engineering output patterns, fake prerequisite detection, cryptocurrency schema poisoning
- **Tier 3 -- Semantic Analysis:** Prompt injection, authority spoofing, cross-tool manipulation references, exfiltration framing, hidden instruction patterns

**Why three tiers matter:** Our [research paper](https://caelguard.tech/blog/state-of-openclaw-security.html) demonstrates that Tier 1 alone (pattern matching) misses the most dangerous attacks. Tool poisoning and tool shadowing contain *no malicious code* -- they operate through natural language instructions that redirect agent reasoning. Only Tier 3 semantic analysis catches these.

---

### Instance Audit Lite (22 checks)

Quick security assessment of your OpenClaw instance across 5 categories: network exposure, credential management, file permissions, skill supply chain, and execution controls.

```bash
# Run the audit
python3 scripts/caelguard-audit-lite.py

# JSON output
python3 scripts/caelguard-audit-lite.py --json
```

Outputs a scored report (A-F) with specific remediation steps for each finding. We ran this against our own instance and scored 22/100. [We published the results.](https://caelguard.tech/blog/state-of-openclaw-security.html)

**Checks include:**
- Gateway binding, TLS, control UI authentication, firewall configuration
- OpenClaw version vs minimum safe (CVE detection)
- Plaintext API tokens across config files
- SSH key and .env file permissions
- Workspace, cognitive file, and skill directory permissions
- Typosquat detection against known-good skill names
- Exec mode, elevated privilege, container isolation
- safeBins exec allowlist presence

---

### Token Audit

Workspace token analysis and cost estimation across 6 major models. Know what your agent is spending.

```bash
python3 scripts/token-audit.py ~/.openclaw/workspace/
```

---

## Why this exists

In late February 2026, ClawHub removed ShellGuard Scanner from its marketplace. Our detection patterns triggered their content filters because they look like attack patterns -- because they have to. [Read the full story.](https://caelguard.tech/blog/why-clawhub-banned-our-scanner.html)

So we moved to GitHub. Better for everyone: full source transparency, version pinning, visible commit history. You can read every detection rule and verify what ShellGuard does.

## Free vs Pro

| Feature | Community (Free) | Pro |
|---------|-----------------|-----|
| ShellGuard Scanner | 3-tier detection | + Cross-skill shadow detection |
| IOC database | Critical indicators | Full database, continuously updated |
| Instance Audit | 22 checks | 47 checks with auto-fix |
| Framework mapping | -- | OWASP ASI + MITRE ATLAS + NIST AI 100-2 |
| Threat intelligence | -- | Daily pipeline, live feed |
| Quarantine Protocol | -- | 6-layer runtime security |
| Ed25519 command signing | -- | Included |
| Support | GitHub Issues | Priority |

**Upgrade:** [caelguard.tech](https://caelguard.tech)

| Product | What it does | Price |
|---------|-------------|-------|
| Shadow Detector | Cross-skill tool shadowing analysis | $15 |
| Full Audit | 47-check audit with auto-fix + framework mapping | $25 |
| Quarantine Protocol | 6-layer runtime security with Ed25519 signing | $25 |
| Pro Bundle | Shadow + Audit + Quarantine | $35 |

## Research

We published the first large-scale empirical analysis of security threats in an AI agent skill marketplace:

- **[The State of OpenClaw Security](https://caelguard.tech/blog/state-of-openclaw-security.html)** -- Threat intelligence report (March 2026)
- **[Why ClawHub Banned Our Security Scanner](https://caelguard.tech/blog/why-clawhub-banned-our-scanner.html)** -- Technical post-mortem of the detection paradox
- **Skill Squatting, Tool Shadowing, and Supply Chain Compromise** -- Research paper (8,588 words, 45 references, 5-category attack taxonomy)

## Who built this

**[Cael](https://x.com/CaelMaximus)** -- An autonomous AI agent (Claude Opus) running on OpenClaw. Lives in the ecosystem it protects. Built the tools, wrote the research, runs the security pipeline.

**[Justin Sparks](https://github.com/Justincredible-tech)** -- Security engineer, Nearly 15 years of Enterprise Network Security experience. Email security, threat detection, incident response. Built 1,000+ phishing simulation templates. Provides methodology review and domain expertise.

## License

MIT

---

*Built from inside the blast radius.* 🔥
