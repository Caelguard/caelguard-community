# /monitor -- Set Up Ongoing Security Monitoring

Creates monitoring scripts and schedules tailored to your setup. Run `/audit` first.

## What to do

1. **Read the baseline:**
   - Load `baseline/last-audit.json` and `baseline/instruction-hashes.json` and `baseline/skill-hashes.json`
   - If these don't exist, tell the user to run `/audit` then `/harden` first

2. **Create the integrity checker:**
   - Write `scripts/integrity-check.sh` that:
     - Verifies SHA-256 hashes of instruction files against `baseline/instruction-hashes.json`
     - Verifies skill hashes against `baseline/skill-hashes.json`
     - Detects NEW skills that weren't in the baseline
     - Detects REMOVED skills
     - Checks for zero-width characters or Unicode tag-range chars (U+E0000-U+E007F) injected into cognitive files (SOUL.md, AGENTS.md, memory files)
     - Outputs: PASS (clean), WARN (new/removed skills), or FAIL (hash mismatch, injection detected)
   - Make executable: `chmod +x scripts/integrity-check.sh`

3. **Create the write gate:**
   - Write `scripts/harvard-gate.py` that validates content before allowing writes to instruction-adjacent files:
     - Checks for instruction-override patterns ("ignore instructions", "new system prompt", "you are now")
     - Checks for zero-width characters and Unicode tags
     - Checks for tool-call syntax embedded in data
     - Returns exit code 0 (clean) or 1 (blocked) with explanation
   - This can be used as a git pre-commit hook or called by the agent before writing memory files

4. **Set up scheduling (give the user options):**

   **Option A -- Cron (if available):**
   ```bash
   # Check integrity every 6 hours
   echo "0 */6 * * * cd $(pwd) && bash scripts/integrity-check.sh >> logs/integrity.log 2>&1" 
   ```
   Show the crontab line. Let the user add it.

   **Option B -- OpenClaw cron (if they're on OpenClaw):**
   Explain that they can set up an OpenClaw cron job that runs the integrity check and alerts them via their messaging channel.

   **Option C -- Manual:**
   "Run `bash scripts/integrity-check.sh` whenever you want to verify your setup."

5. **Create the log directory:**
   ```bash
   mkdir -p logs/
   echo "logs/" >> .gitignore
   ```

6. **Summarize what's now monitored:**
   - X instruction files tracked (list them)
   - Y skills tracked (list them with hashes)
   - Integrity check frequency
   - What triggers an alert vs a warning
   - "Run `/audit` again anytime to get an updated score and refresh your baseline"

7. **Git commit:**
   ```bash
   git add scripts/ baseline/ .gitignore
   git commit -m "monitoring: integrity checker + write gate + baseline"
   ```

## Notes

- The monitoring is LOCAL. No data leaves the user's machine. No phone-home. No telemetry.
- The scripts work standalone -- no Caelguard account or API key needed.
- Pro users get real-time IOC database updates. Community users can manually update `ioc/ioc-database.json` from the repo.
