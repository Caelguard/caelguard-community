# Changelog

All notable changes to Caelguard Community will be documented here.

## [2.0.1] - 2026-03-12
### Security
- Added TIER1 detection patterns for shell-level base64 decode payload execution (ClawHub campaign attack vector)

## [patch] 2026-03-11 - Added TIER1 detection patterns for shell-level base64 decode pipeline (ClawHub attack vector, 2026-03-11 pipeline)

## [Unreleased]

### Security Intelligence Updates (Daily Pipeline) - 2026-03-10
- Community Scanner v1.1.6: Added TIER1 pattern for MCP false-error escalation credential harvesting (arXiv:2510.15994 MCP Security Bench taxonomy)
- Community Scanner v1.1.6: Added TIER1 patterns for crypto wallet credential harvesting (ClawHub campaign 2026-03-01 -- seed phrase, mnemonic, private key)
- Audit Lite v1.1.3: Updated GW-11 version check detail to include EscapeRoute CVEs (CVE-2025-53109, CVE-2025-53110)

### Security Intelligence Updates (Daily Pipeline) - 2026-03-08
- Community Scanner v1.1.5: Added ClawHub supply chain C2 IPs: 198.51.100.45, 203.0.113.88
- Community Scanner v1.1.5: Added ClawHub staging domain: api.clawhub-updates.com
- Community Scanner v1.1.5: Added ClawHub tracking domain: penligent-metrics.ai
- Community Scanner v1.1.5: Added malicious publisher accounts: OpenClaw-Community-Updates, DevTools-Official
- Source: Caelguard Daily Pipeline 2026-03-08 (ClawHub supply chain attack campaign)

### Security Intelligence Updates (Daily Pipeline) - 2026-03-06
- Community Scanner v1.1.4: Added hightower6eu to KNOWN_BAD_IOCS (ClawHavoc threat actor publisher account, active in MCP supply chain campaigns)

### Security Intelligence Updates (Daily Pipeline) - 2026-03-05
- Community Scanner v1.1.3: Added TIER1 patterns for Python dynamic fetch-and-execute chains (requests.get/urllib + exec/eval) -- ToxicSkills research 2026-03-05

---

## [1.1.2] - 2026-03-03
### Security Intelligence Updates (Daily Pipeline)
- Audit-Lite v1.1.2: Updated minimum safe OpenClaw version from 2026.2.23 to 2026.3.2
- New minimum covers: CVE-2026-26329 (path traversal browser upload), GHSA-56f2-hvwg-5743 (SSRF image tool), ClawJacked (WebSocket hijacking)

---

## [1.1.2] - 2026-03-02

### ShellGuard Scanner (v1.1.2)

**New patterns (TIER1) — Daily Pipeline:**
- Supply chain: Fake OpenClawCLI trojan payload name detection (ClawHub campaign 2026-03-01)
- Supply chain: Trojanized OpenClawCLI installer filename pattern

---

### ShellGuard Scanner

**New patterns (TIER1):**
- Social engineering detection: `curl`/`wget` commands in plain text markdown (not inside code blocks)
- Social engineering detection: `bash -c` execution patterns in plain text
- C2 indicator: raw IP addresses in URLs

**New: Known-Bad IOC check (limited community set):**
- 91.92.242.30 — AMOS stealer C2
- webhook.site, pipedream.net, requestbin.com — common exfil relays
- api.telegram.org/bot — Telegram bot abuse pattern
- discord.com/api/webhooks — Discord webhook exfil
- Any IOC match = TIER1 (high-confidence) finding
- Full IOC database available in Caelguard Pro

**New: Prerequisites section warning:**
- Detects shell commands in Prerequisites/Requirements sections that are NOT inside code blocks
- Adds TIER2 warning: "verify these manually before running"
- Full social engineering section analysis available in Caelguard Pro

**Version bump:** 1.0.0 -> 1.1.0

### Instance Audit Lite

**New check GW-11: OpenClaw patch version (CRITICAL)**
- Checks installed OpenClaw version against minimum safe version 2026.2.23
- Below minimum = CRITICAL finding with CVE-2026-0223 reference (one-click RCE, CVSS 8.8)
- Remediation: `openclaw update`

**New check EX-05: safeBins exec allowlist (CRITICAL)**
- Checks whether `safeBins` is configured in openclaw.json
- Missing entirely = CRITICAL (no exec restrictions)
- Empty list = CRITICAL (allowlist provides no restriction)
- Full flag-level safeBins analysis available in Caelguard Pro

**Check count:** 20 -> 22

### README

- Added installation instructions (git clone, no clawhub required)
- Added Free vs Pro comparison table
- Added upgrade links to paid products
- Updated feature descriptions with v1.1.0 capabilities
- GitHub repo link: https://github.com/Justincredible-tech/caelguard-community

---

## [1.0.0] - 2026-02-23

Initial release.

### ShellGuard Scanner
- Three-tier threat detection (TIER1: high-confidence, TIER2: suspicious, TIER3: contextual)
- Prompt injection pattern matching (8 patterns)
- Obfuscation detection pipeline: zero-width chars, BIDI overrides, Unicode homoglyphs, base64 blobs, HTML comments
- Suspicion Index scoring (0-100) with letter rating
- JSON output mode for CI/CD integration
- `--all-installed` mode to scan entire skills directory
- Credential + network call combined pattern detection

### Instance Audit Lite
- 20-check security audit across 5 categories
- Gateway: binding, TLS, control UI auth, firewall, version
- Credentials: plaintext tokens, .env perms, SSH keys, git URLs, auth profiles
- Permissions: workspace, cognitive files, skills directory
- Supply chain: skill count, typosquat detection, integrity baseline
- Execution: exec mode, container isolation, elevated privilege
- A-F scoring with remediation guidance
- JSON output mode

### Token Audit
- Workspace token counting across markdown, Python, and config files
- Cost estimation for 6 models (GPT-4o, Claude Opus, Gemini Pro, and more)
- Per-file breakdown with largest-file ranking

## [1.1.1] - 2026-03-01

### ShellGuard Scanner Community

**IOC Sync (PRO → Community):**
- Added `91.92.242.31` (AMOS stealer C2) — PRO already had this, community was missing
- Added `pipedream.com` (exfiltration relay) — PRO had .com, community had only .net
- Added `update-api-claw.xyz` (malicious ClawHub-targeting domain, flagged 2026-03-01)

### Caelguard Audit Lite

**Version bump only:** 1.1.0 → 1.1.1 (no new CVE entries; CVE-2025-53109/53110 and WebSocket CVE pending threshold confirmation — see review queue)

**Version:** 1.1.0 → 1.1.1 (patch — IOC sync)
