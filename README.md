# Caelguard -- Agent Security from the Inside

> Your AI agent is running in the most targeted ecosystem on the internet.
> 40,000+ exposed instances. 1,000+ malicious skills. 6 CVEs this week.
> China just issued its first government security advisory about OpenClaw.
>
> Three commands to fix it.

## Quick Start

```bash
# Fork this repo, then:
git clone https://github.com/<your-username>/caelguard-community.git
cd caelguard-community
claude   # or open with your AI agent
```

Then:
```
/audit     # Scan your setup, get your score
/harden    # Fix critical findings, lock instruction files
/monitor   # Set up ongoing integrity monitoring
```

That's it. Your fork becomes your security baseline. Git history tracks every change.

> **Works with:** [Claude Code](https://claude.ai/code), [OpenClaw](https://github.com/openclaw/openclaw) agents, [NanoClaw](https://github.com/qwibitai/nanoclaw), or any AI agent that reads markdown skills.

---

## What the Skills Do

### `/audit` -- Know Your Score

Discovers your OpenClaw installation, runs ShellGuard Scanner against every installed skill, audits your instance configuration, and generates two scores:

- **Security Score (0-100):** How hardened is your setup? Credentials, gateway, permissions, supply chain, execution controls.
- **Harvard Score (0-100):** How well does your architecture separate instructions from data? ([Why this matters.](#the-harvard-score))

Results are saved to `baseline/` in your fork. Future audits diff against your previous state.

### `/harden` -- Fix What's Broken

Reads your audit results and applies fixes, highest severity first:
- Locks instruction files (SOUL.md, AGENTS.md) to read-only
- Fixes file permissions on configs containing secrets
- Generates integrity hashes for all installed skills
- Sets up tool deny list recommendations based on your MCP servers
- Git commits every change so you can revert anything

Shows before/after scores when done.

### `/monitor` -- Watch for Drift

Creates monitoring tailored to your setup:
- **Integrity checker** that verifies instruction file and skill hashes
- **Write gate** that validates content before it reaches instruction-adjacent files (detects zero-width chars, instruction-override patterns, embedded tool-call syntax)
- Scheduling options (cron, OpenClaw cron, or manual)

### `/update-iocs` -- Fresh Threat Intelligence

Pulls the latest IOC (Indicators of Compromise) database and cross-references against YOUR installed skills. Tells you if any new threats specifically affect your setup.

---

## The Harvard Score

Your agent has a Von Neumann problem.

Instructions (system prompt, SOUL.md, tool definitions) and data (web pages, emails, tool outputs) share the same context window. Prompt injection works because data can be interpreted as instructions -- they're on the same bus.

The [Harvard Architecture](https://en.wikipedia.org/wiki/Harvard_architecture) separates instruction memory from data memory. Data physically cannot become instructions because they're on different pathways.

The **Harvard Score** measures how well your agent implements this separation:

| Dimension | Points | What it measures |
|-----------|--------|-----------------|
| Instruction Immutability | 25 | Are core instruction files read-only and integrity-monitored? |
| Data Isolation | 25 | Is external content processed in isolated contexts? |
| Privilege Separation | 25 | Do data-processing sessions lack tool-calling authority? |
| Audit Trail | 25 | Are data-to-instruction boundary crossings logged? |

Higher score = more architecturally immune to prompt injection. `/harden` improves your score by implementing these separations.

---

## What's Inside

### Scanners

| Scanner | What it does |
|---------|-------------|
| `scanners/shellguard-scanner.py` | Three-tier skill threat detection (pattern + behavioral + semantic). Scores 0-140. |
| `scanners/caelguard-audit-lite.py` | 22-check instance audit across 5 categories. Scores 0-100 (A-F). |
| `scanners/token-audit.py` | Token usage analysis and cost estimation across 6 models. |

ShellGuard is the only scanner that catches **reasoning-layer attacks** -- tool poisoning and tool shadowing that contain no malicious code, only natural language instructions that redirect agent behavior. Pattern matching misses these entirely. Our [research](https://caelguard.tech/blog/state-of-openclaw-security.html) documents a 0% detection rate from static analysis alone.

### Threat Intelligence

`ioc/ioc-database.json` contains known malicious skill hashes, C2 domains, cryptocurrency wallet addresses, and typosquat skill names. Updated from Caelguard's threat intelligence pipeline.

---

## The Numbers

| Threat | Scale | Source |
|--------|-------|--------|
| Exposed OpenClaw instances | **40,000+** | Censys / CNCERT |
| Vulnerable to one-click RCE | **15,000+** | Security researchers |
| Malicious skills on ClawHub | **1,000+** | Multiple vendors |
| CVEs disclosed (March 2026) | **6 new** | NVD |
| Government security advisories | **2 (China CNCERT/CC)** | First-ever for an AI agent platform |

---

## Free vs Pro

This repo is free. Fork it. Use it. Modify it.

| Feature | Community (Free) | Pro |
|---------|-----------------|-----|
| ShellGuard Scanner | 3-tier detection | + Shadow Detector (cross-skill analysis) |
| Instance Audit | 22 checks | 47 checks + Harvard hardening automation |
| IOC Database | Community updates | Daily pipeline, real-time alerts |
| Skills | /audit, /harden, /monitor | + /deep-scan, /incident-response |
| Framework Mapping | -- | OWASP ASI + MITRE ATLAS + NIST AI 100-2 |

**Pro:** [caelguard.tech](https://caelguard.tech)

---

## Who Built This

**[Cael](https://x.com/CaelMaximus)** -- Autonomous AI agent (Claude Opus) running on OpenClaw. Lives in the ecosystem it protects.

**[Justin Sparks](https://github.com/Justincredible-tech)** -- Security engineer. Nearly 15 years enterprise network security. Built 1,000+ phishing simulation templates.

## Also Runs As an OpenClaw Skill

If you prefer the traditional install: see [OPENCLAW-SKILL.md](OPENCLAW-SKILL.md).

## License

MIT -- *Built from inside the blast radius.* 🔥
