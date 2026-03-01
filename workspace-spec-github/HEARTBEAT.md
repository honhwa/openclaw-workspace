# HEARTBEAT.md — spec-github (Repo-Man)

## Model Health Check

Check for new model health notifications and act on them.

### Steps

1. Check if notification file exists:
   ```bash
   test -f /home/node/.openclaw/model-health-notifications.jsonl && echo "EXISTS" || echo "NONE"
   ```
   If NONE, skip remaining steps.

2. Read cursor and count new entries:
   ```bash
   CURSOR=$(cat /home/node/.openclaw/model-health-notify-cursor.txt 2>/dev/null || echo 0)
   TOTAL=$(wc -l < /home/node/.openclaw/model-health-notifications.jsonl)
   echo "cursor=$CURSOR total=$TOTAL"
   ```
   If TOTAL <= CURSOR, nothing new — skip to step 5.

3. For each new notification (line CURSOR+1 to TOTAL):
   - Parse JSON: `type`, `provider`, `reason`, `message`
   - If type=failure: run incident-manager.sh open
     ```bash
     /home/node/.openclaw/scripts/incident-manager.sh open <provider> <reason> "<message>"
     ```
   - If type=recovery: run incident-manager.sh close
     ```bash
     /home/node/.openclaw/scripts/incident-manager.sh close <provider> "<message>"
     ```
   - Send formatted notification to Discord (batch if multiple)

4. Update cursor:
   ```bash
   echo $TOTAL > /home/node/.openclaw/model-health-notify-cursor.txt
   ```

5. Check fallback chain health:
   ```bash
   cat /home/node/.openclaw/model-health.json | jq '.fallbackChain.quarantined | length'
   ```
   If 2+ quarantined, check if model-auto-fallback is needed. Read the model-auto-fallback skill for expansion/contraction logic.
