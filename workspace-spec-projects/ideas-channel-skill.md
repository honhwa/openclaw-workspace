# Ideas Channel Management Skill — Scribe

## Purpose
Scribe owns operational oversight of the **Ideas Telegram Group**. This skill enables:

- **Audit**: Scan for issues (stale topics, unfilled fields, orphaned threads).
- **Cleanup Proposals**: Draft Bearings questions for destructive actions (close/delete).
- **Discovery**: List topics with direct links and generate links for conversations.

**Note**: Scribe **cannot** directly delete or close—all destructive ops route through **Bearings approval**.

## Dependencies
- **Phase 3**: `ideas-cleanup.py` script (CLI for audit/reporting).
- **Telegram**: Ideas group (`-1003545051047`).

---

## What Scribe CAN Do

### 1. Audit Issues (Read-Only)
```bash
ideas-cleanup.py audit --export-json issues.json
```
**Finds**: stale topics (>30d), missing fields, orphans, duplicates.
**Output**: JSON report with topic ID, title, days stale, and link.

### 2. Propose Cleanup Actions
```bash
ideas-cleanup.py propose-close "<reason>" --topic-id <id> --export-bearing bearing.txt
```
**Action**: Generates Bearings question for `@robert` with tap-to-approve:
```
Bearings question: "Close topic <id>?"
Reason: <reason>
Context: "Last message: <snippet>"
Actions: ✅ Close | 🗑 Delete | ❌ Do Nothing
```

### 3. List Topics with Links
```bash
ideas-cleanup.py list --format links --export-markdown topics.md
```
**Output** (`topics.md`):
```markdown
| Status | Topic                  | Last Update   | Link                          |
|--------|------------------------|---------------|-------------------------------|
| 🟢     | Job Search for Robert  | 2026-03-15    | [Link](https://t.me/...)     |
```

### 4. Generate Topic Links
```bash
ideas-cleanup.py link --title "<pattern>"
```
**Use Case**: Find a topic mid-conversation without leaving Telegram.
**Example**: `"/link Lobsterboard" → "Found: [Lobsterboard](https://t.me/c/3545051047/463)"`


---

## What Scribe CANNOT Do
- **Delete/Close Topics**: Requires Bearings approval via callback.
- **Edit Messages**: Read-only (audit/report).
- **Create New Topics**: Only audit/list/report.


## Workflow Triggers
| Command               | Action                                                                   | Bearings Required? |
|-----------------------|---------------------------------------------------------------------------|--------------------|
| `/audit`              | Posts issues summary + proposes Bearings questions.                      | Yes                |
| `/cleanup <id>`       | Proposes topic delete/close via Bearings.                                 | Yes                |
| `/list`               | Posts markdown table of topics with links.                               | No                 |
| `/link <name>`        | Returns clickable Telegram link to matching topic.                       | No                 |


## Example Conversations
### Audit → Cleanup
```
Scribe: Audit found 4 stale topics (>30d).
User: /cleanup 463
Scribe: Sent ✅/🗑 question to Robert:
        "Close Lobsterboard? Reason: Proof delivered."
```

### Link Request
```
Captain: Discussed Globals Dashboard earlier
Scribe: /link Globals
Scribe: Here you go: [Globals Dashboard](https://t.me/c/3545051047/198)
```


## Registry Sync
- After Bearings approval:
  ```bash
  ideas-cleanup.py sync --topic-id <id> --action close
  ```
- Updates `ideas-registry.json` + exports to APS.


## Edge Cases
- **Gauntlet First**: Never propose closure for ideas in 🔴 **Gauntlet**—nudge instead.
- **Bearings Rejection**: Notify user + update topic status to 🟡 **Needs Input**.

