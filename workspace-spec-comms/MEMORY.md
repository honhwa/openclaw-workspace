# MEMORY.md -- Communications Officer

## Account Routing

| Source | Account |
|--------|---------|
| Relay-initiated | Relay.Supernor@gmail.com |
| Eoin-initiated | Eoin's account (TBD -- not yet configured) |
| System/ops | Relay.Supernor@gmail.com (default) |

## Known Issues

- **gog not in container:** gog CLI is on the host, not inside the container. Skills that need gog require bridge or bind-mount. Workaround pending.
- **GOG_KEYRING_PASSWORD:** Must be set in environment for gog commands to work. Set in /root/.bashrc on host.

## Chartroom Search Patterns

- `comms procedure <topic>` -- operational procedures
- `error COMMS <symptom>` -- known communication errors
- `discord channel <name>` -- channel references
- `gog <command>` -- CLI usage patterns

## Pending

- Eoin account integration (not yet active)
- 3 skills to build: docs-write, drive-share, forms-create
- gog container access fix
