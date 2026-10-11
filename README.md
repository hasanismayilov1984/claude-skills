# claude-skills

My reusable [Claude Code](https://claude.com/claude-code) skills, packaged as a plugin. Install once and use them in any repo.

| Command | What it does |
|---|---|
| `/hasan:keywords` | Asks for a job description, extracts its keywords, scores my resume against them, and rewrites a tailored copy — adding only keywords I confirm I have |
| `/hasan:run-code-review` | Reviews the current branch for bugs, security, SQL, DAX, and LLM-specific issues |
| `/hasan:check-secrets` | Scans files and git history for API keys and personal data before pushing or publishing |

## Install

In a terminal:

```bash
claude plugin marketplace add hasanismayilov1984/claude-skills
claude plugin install hasan@hasan-skills
```

Update later with `claude plugin marketplace update hasan-skills`.

## Structure

```
.claude-plugin/marketplace.json     # marketplace catalog
plugins/hasan/
├── .claude-plugin/plugin.json      # plugin manifest
└── skills/<skill-name>/SKILL.md    # one folder per skill
```

## Adding a skill

1. Create `plugins/hasan/skills/<new-skill>/SKILL.md` with `name` and `description` frontmatter.
2. Run `claude plugin validate .`
3. Commit, push, then run `claude plugin marketplace update hasan-skills`.
