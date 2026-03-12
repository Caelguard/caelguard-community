# /audit -- Security Audit for Your OpenClaw Instance

Run a comprehensive security audit tailored to THIS specific OpenClaw installation.

## What to do

1. **Discover the environment:**
   - Find the OpenClaw workspace (usually `~/.openclaw/workspace/`)
   - Read the agent config at `~/.openclaw/agents/*/agent/openclaw.json` (DO NOT output any API keys or secrets -- just note what's configured)
   - List installed skills: `ls ~/.openclaw/workspace/skills/`
   - Check for MCP servers in the config
   - Check gateway binding: `grep -r "bind" ~/.openclaw/agents/*/agent/openclaw.json`
   - Get OpenClaw version: `openclaw --version 2>/dev/null || cat /usr/lib/node_modules/openclaw/package.json | grep version`

2. **Run the scanners:**
   ```bash
   # Run the instance audit
   python3 scanners/caelguard-audit-lite.py --json 2>/dev/null
   
   # Scan all installed skills
   python3 scanners/shellguard-scanner.py --all-installed --json 2>/dev/null
   ```

3. **Generate the baseline:**
   - Create `baseline/` directory if it doesn't exist
   - Write `baseline/last-audit.json` with:
     - Timestamp
     - OpenClaw version
     - List of installed skills with SHA-256 hashes of their SKILL.md files
     - List of MCP servers discovered
     - Audit score and individual check results
     - ShellGuard results per skill
   - If a previous baseline exists, DIFF against it and highlight what changed

4. **Calculate the Harvard Score:**
   Check these and score 0-100:
   - Are instruction files (SOUL.md, AGENTS.md) read-only? (0 or 25 pts)
   - Are external data sources processed in isolated sessions? (0-25 pts based on config)
   - Do memory files have integrity monitoring? (0 or 25 pts)
   - Is there separation between data-processing and tool-calling contexts? (0-25 pts)

5. **Present results to the user:**
   - Overall security score (0-100, letter grade A-F)
   - Harvard Score (0-100)
   - Top 5 findings by severity with plain-language explanations
   - Comparison to previous baseline (if exists)
   - Ask: "Want me to run `/harden` to fix the critical and high findings?"

## Important

- NEVER output API keys, tokens, passwords, or secrets. Note their PRESENCE, not their VALUES.
- If a scanner fails, explain the error and suggest fixes.
- Be honest about scores. A low score is useful information, not a failure.
- Save all results to `baseline/` so future audits can track improvement.
