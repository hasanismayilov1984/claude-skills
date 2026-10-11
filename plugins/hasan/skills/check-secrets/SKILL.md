---
name: check-secrets
description: Scan the current repo and its git history for API keys, passwords, .env files, and personal data before committing, pushing, or making a repo public. Use when asked to check for secrets, before publishing a repo, or before a push.
---

# Check secrets and personal data

Never print a full secret. Show at most the first 4 characters, then `****`.

## Checks

1. **Tracked sensitive files** — `git ls-files` and flag tracked `.env*` (except `.env.example`), `*.pem`, `*.key`, `*.pfx`, `credentials*.json`, `*.csv`/`*.xlsx` data files, `*.log`, `*.pkl`, `__pycache__/`, `.DS_Store`.
2. **Secrets in code** — search tracked files for: `sk-`, `sk-ant-`, `AKIA`, `ghp_`, `github_pat_`, `xox[bp]-`, `AIza`, `api_key\s*=`, `password\s*=`, `secret\s*=`, `Bearer `, connection strings (`Server=`, `postgres://`, `mysql://`, `AccountKey=`).
3. **Git history** — `git log --all --diff-filter=A --name-only --format=` for sensitive files ever added; `git log --all -p -S "sk-" --format="%h %s"` (repeat for other key prefixes) for secrets ever committed, even if deleted later. If the clone is shallow, say so and run `git fetch --unshallow` only if the user agrees.
4. **Personal data** — in tracked data files and logs, look for real names with emails, phone numbers, street addresses, birth dates, SSN-like patterns. Report the file and column, not the values.
5. **.gitignore** — confirm it ignores `.env` (not just `.env/`), `*.log`, `__pycache__/`, `.DS_Store`, and data files.

## Report

A table: check → PASS / FAIL → what was found → fix.

If a secret is in history:
- Tell the user to **rotate the key now** at the provider — removing it from git is not enough.
- Explain that removing it from history needs a rewrite (`git filter-repo`) and a force-push, which can't be undone. **Ask before doing it.**

Don't change files unless the user asks. For safe fixes (untracking files, fixing `.gitignore`), make them on a new branch.
