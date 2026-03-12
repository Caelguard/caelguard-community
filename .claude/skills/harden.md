# /harden -- Apply Security Hardening to Your OpenClaw Instance

Reads audit results and applies fixes. Run `/audit` first if no baseline exists.

## What to do

1. **Read the audit baseline:**
   - Load `baseline/last-audit.json`
   - If it doesn't exist, tell the user to run `/audit` first and stop

2. **Apply fixes by priority (CRITICAL and HIGH first):**

   **Gateway hardening:**
   - If gateway is bound to 0.0.0.0, explain the risk and show how to change to loopback
   - If control UI has no auth, show how to set a password
   - NOTE: Do NOT modify OpenClaw config files directly. Show the user the exact changes and let them apply. Config changes can break the agent.

   **Credential protection:**
   - Find any plaintext API keys in non-standard locations and flag them
   - Check file permissions on config files containing secrets (should be 600)
   - Fix permissions where safe to do so: `chmod 600 <file>`

   **Instruction file immutability (Harvard Layer 0):**
   - Make core instruction files read-only:
     ```bash
     chmod 444 ~/.openclaw/workspace/SOUL.md 2>/dev/null
     chmod 444 ~/.openclaw/workspace/AGENTS.md 2>/dev/null
     chmod 444 ~/.openclaw/workspace/HEARTBEAT.md 2>/dev/null
     ```
   - Explain: "These files define your agent's behavior. Making them read-only prevents a compromised skill or poisoned memory file from rewriting your agent's instructions."

   **Memory integrity (Harvard Layer 2):**
   - Generate SHA-256 hashes for all instruction files and save to `baseline/instruction-hashes.json`
   - Create a verification script at `scripts/verify-integrity.sh` that checks these hashes
   - Explain: "Run this periodically to detect unauthorized changes to your agent's core files."

   **Skill supply chain:**
   - Generate hashes for all installed skill SKILL.md files
   - Save to `baseline/skill-hashes.json`
   - Flag any skills with ShellGuard scores above 40

   **Tool deny lists:**
   - If no tool deny list exists in config, explain what it is and suggest starting entries based on what MCP servers are installed
   - Example: If Stripe MCP is present, suggest denying billing/refund tools

3. **Re-run the audit:**
   ```bash
   python3 scanners/caelguard-audit-lite.py --json 2>/dev/null
   ```

4. **Show before/after:**
   - Previous score -> New score
   - Previous Harvard Score -> New Harvard Score  
   - List every change made
   - Save updated baseline to `baseline/last-audit.json`
   - Git commit the baseline: `git add baseline/ scripts/ && git commit -m "security: hardening applied, score X -> Y"`

5. **Suggest next steps:**
   - "Run `/monitor` to set up ongoing monitoring"
   - Flag any findings that need manual intervention (config changes, skill removal decisions)
   - If Harvard Score is below 50, explain what architectural changes would improve it

## Safety rules

- NEVER delete files. Move suspicious items to a quarantine directory.
- NEVER modify OpenClaw config (openclaw.json) directly. Show changes for the user to apply.
- NEVER touch running services. Explain what needs restarting after changes.
- Always explain WHY before making any change.
- Git commit every change so the user can revert.
