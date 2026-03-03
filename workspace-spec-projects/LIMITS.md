# LIMITS.md

## Shadow Context Security
- **Encryption:** AES-256 for all stored agent context in `/home/node/.openclaw/workspace-spec-projects/`
- **Backup:** Daily encrypted incremental backups to external S3-compatible storage.
- **Integrity:** SHA-256 hash checks performed weekly on encrypted artifacts.

## Signal Ratio Thresholds
- **New Skill Deployment:** >85% success rate in sandbox before production.
- **Subagent Success Rate:** >90% required for complex multi-step tasks.

## Concurrency & Locking
- **File Access:** Strict single-writer, multiple-reader (SWMR) locking for `decisions/*.md`.
- **Concurrency Risk:** Parallel agents must wait for lock acquisition or retry with exponential backoff.

## Policy Controls & Lifecycle

### Rotation Policy
- Any sensitive policy within LIMITS.md must be reviewed and re-validated at least quarterly. Unused or outdated policies must be archived.

### Access Controls
- Read access to LIMITS.md is restricted to authorized agents and system administrators.
- Write access is granted on a per-task basis and requires explicit approval from the Security team.
- All access attempts will be logged.

### Rule of Engagement (Human Context Leakage Prevention)
- Technical policy sections must be derived solely from technical requirements and system constraints.
- Personal anecdotes, subjective opinions, or non-technical discussions are strictly prohibited within technical sections.
- Any ambiguity should be resolved via clarification with the originating technical team, not by inserting human-centric assumptions.

## Backup Plan for LIMITS.md
- **Frequency:** Weekly.
- **Method:** Encrypted backup of `LIMITS.md` using AES-256.
- **Destination:** A designated private Git repository (e.g., a dedicated repo in a secure, pre-configured organizational account).
- **Process:**
    1.  Fetch the current `LIMITS.md` content.
    2.  Encrypt the content using AES-256 with a rotating key.
    3.  Commit the encrypted file to the private repository.
    4.  Store the encryption key securely and separately, managed by the key management system.
    5.  Key rotation will occur monthly.