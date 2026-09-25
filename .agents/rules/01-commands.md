---
trigger: always_on
---

# Command Permissions

## Run automatically — never ask first
- `npm run build`
- `npm run lint`
- `python -m graphify --version`
- `python -m graphify query "<question>"`
- `python -m graphify . --code-only --update`
- `python -m graphify cluster-only .`

## Always ask for approval first
- `npm install` / `npm uninstall` / any dependency add, remove, or upgrade
- Any command that modifies `package.json`, `package-lock.json`, or env/config files
- Git commands that change history or remote state: `commit`, `push`, `merge`, `reset`, etc.
- Any destructive or irreversible command: deleting files/folders, dropping DB collections, etc.

Do not deviate from this list. If a command isn't listed above, treat it as "ask first."
