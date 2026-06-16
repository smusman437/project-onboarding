# Project Onboarding Skills

**Go from zero knowledge to advanced project understanding with minimal tokens per session** — persistent state files + bounded slash commands replace full-repo re-scans every chat.

Installable via [skills.sh](https://skills.sh) for Cursor, Codex, GitHub Copilot, Windsurf, and other Agent Skills-compatible tools.

**Repository:** [github.com/smusman437/project-onboarding](https://github.com/smusman437/project-onboarding)

---

## Install (all skills at once)

```bash
npx skills add smusman437/project-onboarding --skill '*' -y
```

Install for Cursor only:

```bash
npx skills add smusman437/project-onboarding --skill '*' -y -a cursor
```

Install globally (all your projects):

```bash
npx skills add smusman437/project-onboarding --skill '*' -y -g
```

Verify discovery:

```bash
npx skills add smusman437/project-onboarding --list
```

Confirm local install:

```bash
npx skills list
```

---

## What is init?

This repo uses **init** in three related ways. They apply at different stages.

### 1. Package init (for contributors)

When **adding a new skill to this package**, scaffold a `SKILL.md` with the skills CLI:

```bash
npx skills init my-new-skill --path skills/my-new-skill
```

If you change repo-root `templates/`, sync copies into `skills/start-onboarding/templates/` before publishing.

### 2. Project init (for users)

Install all skills, then bootstrap local state in your target repo:

```bash
npx skills add smusman437/project-onboarding --skill '*' -y
```

Then in Cursor Agent:

```
/start
```

The `/start` skill:
- Creates `docs/onboarding/` with state files and per-repo doc stubs
- Installs `.cursor/rules/project-knowledge-first.mdc` so every agent session reads onboarding docs before analyzing code

### 3. Project-Knowledge-First rule (auto-installed)

`/start` copies the bundled Cursor rule to `.cursor/rules/project-knowledge-first.mdc`. It sets `alwaysApply: true` and enforces doc-first, bounded exploration, plan-before-edit, and `/update-docs` after new discoveries.

---

## Slash commands

### Onboarding loop

| Skill | Slash | When to use |
|-------|-------|-------------|
| start-onboarding | `/start` | First time on a repo — bootstrap `docs/onboarding/` + install rule |
| audit-project | `/audit` | Measure completion % and find documentation gaps |
| continue-onboarding | `/continue` | Explore the next highest-priority area (one per session) |
| learn-module | `/learn` | Deep-dive one module in mentor mode |
| update-docs | `/update-docs` | Capture new knowledge after completing work |
| quick-reference | `/quick-ref` | Answer "where is X?" from state files, no full scan |

### Development workflow

| Skill | Slash | When to use |
|-------|-------|-------------|
| ticket-analysis | `/ticket` | Analyze a ticket — requirements, impact, plan (no code) |
| review-work | `/review` | Review diffs before merge — patterns, edge cases, tests |
| prepare-commits | `/prepare-commits` | Group changes into commit-ready batches with messages |

All slash-triggered skills use `disable-model-invocation: true` — invoke them explicitly.

---

## 5-minute quickstart

1. **Install all skills** — `npx skills add smusman437/project-onboarding --skill '*' -y`
2. **Init project** — `/start`
3. **Measure gaps** — `/audit`
4. **Explore one area** — `/continue`
5. **Go deep** — `/learn auth` (or any module name)

Daily workflow:

```
/ticket <paste issue>  → plan work
... implement ...
/review                → pre-merge check
/prepare-commits       → commit groups
/update-docs           → capture new knowledge
/continue              → next onboarding area
```

---

## Repo section guide

| Path | Purpose |
|------|---------|
| `skills/` | Installable slash-command workflows (9 skills). skills.sh discovers skills here. |
| `skills/start-onboarding/templates/` | Bundled scaffolds for `/start` (sync from repo-root `templates/`). |
| `skills/start-onboarding/rules/` | Bundled Cursor rule for `/start`. |
| `rules/` | Source of truth for `project-knowledge-first.mdc`. |
| `templates/` | Contributor source for state file scaffolds. |
| `AGENTS.md` | Cross-agent routing — slash map, state files, token budget. |

---

## State file roles

Created in **your target repo** at `docs/onboarding/`:

| File | Role |
|------|------|
| `project-memory.md` | Source of truth — config, stack, conventions |
| `progress.md` | Session log and resume pointer for `/continue` |
| `onboarding-audit.md` | Gap analysis and completion % |
| `open-questions.md` | Blocked items only |
| `repositories-overview.md` | Index of discovered repos/modules |
| `repos/<slug>/*.md` | Per-repo domain docs |

---

## Verify live on skills.sh

1. **GitHub public:** [github.com/smusman437/project-onboarding](https://github.com/smusman437/project-onboarding)
2. **CLI lists 9 skills:**
   ```bash
   npx skills add smusman437/project-onboarding --list
   ```
3. **Search:** `npx skills find onboarding` or browse [skills.sh](https://skills.sh)
4. **Local install check:** `npx skills list` shows all 9 skills after install

---

## License

MIT — see [LICENSE](LICENSE)
