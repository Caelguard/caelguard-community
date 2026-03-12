# /update-iocs -- Update Threat Intelligence Database

Pull the latest IOC (Indicators of Compromise) database and check if any match your installed skills.

## What to do

1. **Fetch the latest IOC database:**
   ```bash
   curl -sL https://raw.githubusercontent.com/Caelguard/caelguard-community/main/ioc/ioc-database.json -o ioc/ioc-database.json.new
   ```

2. **Compare against current:**
   - If `ioc/ioc-database.json` exists, diff the two:
     - Count new hashes added
     - Count new C2 domains added
     - Count new typosquat names added
   - Replace old with new: `mv ioc/ioc-database.json.new ioc/ioc-database.json`

3. **Cross-reference against YOUR skills:**
   - Load `baseline/skill-hashes.json` (run `/audit` first if it doesn't exist)
   - Check every installed skill's hash against the IOC hash list
   - Check every installed skill's name against the typosquat list
   - Report matches with severity and recommended action

4. **Re-scan if new IOCs found:**
   If any new IOCs were added:
   ```bash
   python3 scanners/shellguard-scanner.py --all-installed --json 2>/dev/null
   ```
   Compare results to previous scan. Flag any score changes.

5. **Report:**
   - "IOC database updated: X new hashes, Y new domains, Z new typosquat names"
   - "Cross-referenced against your N installed skills: [results]"
   - "No matches found" or "WARNING: [details of matches]"

6. **Git commit:**
   ```bash
   git add ioc/
   git commit -m "ioc: updated threat intelligence database"
   ```

## Pro upgrade note

The community IOC database is updated periodically. Pro users get:
- Daily automated IOC updates from the Caelguard threat intelligence pipeline
- Real-time alerts when new IOCs match installed skills
- Extended database with behavioral indicators (not just hashes and domains)
