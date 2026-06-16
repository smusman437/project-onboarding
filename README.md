# Project Onboarding Skills

**Go from zero knowledge to advanced project understanding with minimal tokens per session** — persistent state files + bounded slash commands replace full-repo re-scans every chat.

Installable via [skills.sh](https://skills.sh) for Cursor, Codex, GitHub Copilot, Windsurf, and other Agent Skills-compatible tools.

---

## What is init?

This repo uses **init** in three related ways. They apply at different stages.

### 1. Package init (for contributors)

When **adding a new skill to this package**, scaffold a `SKILL.md` with the skills CLI:

```bash
npx skills init my-new-skill --path skills/my-new-skill
```

This creates a skill directory with valid YAML frontmatter. Edit the generated `SKILL.md`, add templates if needed, and document the slash trigger in this README.

**Purpose:** Standardize new skill authoring so every skill in `skills/` follows the same structure skills.sh expects.

If you change repo-root `templates/`, sync copies into `skills/start-onboarding/templates/` before publishing.

### 2. Project init (for users)

When **onboarding onto any target codebase**, install this package then bootstrap local state:

```bash
npx skills add <owner>/project-onboarding
```

Then in your target repo, run:

```
/start
```

The `/start` skill:
- Creates `docs/onboarding/` with state files and per-repo doc stubs (detects monorepo, multi-repo, or single-repo)
- Installs `.cursor/rules/project-knowledge-first.mdc` so every agent session reads onboarding docs before analyzing code

No manual file creation required.

**Purpose:** One-time setup so `/continue`, `/audit`, `/learn`, and `/quick-ref` can resume across sessions without re-exploring the entire codebase.

### 3. Project-Knowledge-First rule (auto-installed)

`/start` copies the bundled Cursor rule to `.cursor/rules/project-knowledge-first.mdc` in your target repo. It sets `alwaysApply: true` and enforces:

- Read `docs/onboarding/` state files before analyzing code
- Use onboarding docs as primary context
- Explore only repos/modules relevant to the task
- Plan before implementation; minimal diffs
- Run `/update-docs` after discovering new knowledge

**Purpose:** Everyday agent work (not just slash commands) stays doc-first and token-efficient.

---

## Install

```bash
npx skills add <owner>/project-onboarding
```

Replace `<owner>` with your GitHub org or username after publishing.

Install all skills:

```bash
npx skills add <owner>/project-onboarding --skill '*'
```

---

## Slash commands

| Skill | Slash | When to use |
|-------|-------|-------------|
| start-onboarding | `/start` | First time on a repo — bootstrap `docs/onboarding/` + install rule |
| audit-project | `/audit` | Measure completion % and find documentation gaps |
| continue-onboarding | `/continue` | Explore the next highest-priority area (one per session) |
| learn-module | `/learn` | Deep-dive one module in mentor mode |
| update-docs | `/update-docs` | Capture new knowledge after completing work |
| quick-reference | `/quick-ref` | Answer "where is X?" from state files, no full scan |

All slash-triggered skills use `disable-model-invocation: true` — invoke them explicitly.

---

## 5-minute quickstart

1. **Install** — `npx skills add <owner>/project-onboarding`
2. **Init project** — `/start` (creates `docs/onboarding/` + `.cursor/rules/project-knowledge-first.mdc`)
3. **Measure gaps** — `/audit` (completion % + prioritized tiers)
4. **Explore one area** — `/continue` (updates `progress.md`)
5. **Go deep** — `/learn auth` (or any module name)

Come back tomorrow and run `/continue` again — it reads `progress.md` and picks up where you left off.

---

## Repo section guide

| Path | Purpose |
|------|---------|
| `skills/` | Installable slash-command workflows. Each subdirectory is one skill with a `SKILL.md` entry point. skills.sh discovers skills here. |
| `skills/start-onboarding/templates/` | Bundled scaffolds copied into target repos during `/start` (also at repo-root `templates/` for contributors). |
| `skills/start-onboarding/rules/` | Bundled Cursor rule installed to `.cursor/rules/` during `/start`. |
| `rules/` | Source of truth for `project-knowledge-first.mdc` (sync to `skills/start-onboarding/rules/` before publish). |
| `templates/` | Contributor source for state file scaffolds — sync to `skills/start-onboarding/templates/` on change. |
| `templates/per-repo/` | Six-file doc set (overview, architecture, request-flow, common-patterns, integrations, important-files) per discovered repo/module. |
| `AGENTS.md` | Routing summary for non-Cursor agents — slash map, state file locations, token budget rules. |

---

## State file roles

Created in **your target repo** at `docs/onboarding/` (path overridable in `project-memory.md`):

| File | Role |
|------|------|
| `project-memory.md` | Source of truth — config, stack, conventions, project-wide facts |
| `progress.md` | Session log and resume pointer for `/continue` |
| `onboarding-audit.md` | Gap analysis, tier scores, completion % — owned by `/audit` |
| `open-questions.md` | Blocked items and unanswered questions only |
| `repositories-overview.md` | Index of discovered repos/modules with links to per-repo docs |
| `repos/<slug>/*.md` | Domain-specific docs for one repo or module |

**Rule:** Each file has one role. Do not duplicate the same facts across files.

---

## Token efficiency

| Without onboarding skills | With onboarding skills |
|---------------------------|------------------------|
| Re-explain the repo every chat | Read 2–4 state files (~few KB) |
| Full-tree scans per question | One bounded area per `/continue` |
| Context lost between sessions | `progress.md` compounds over days/weeks |

Each skill enforces: read state first → explore one area → write back → teach summary.

---

## Example session

```
You: /start

Agent: Detected single-repo Node/Express app.
       Created docs/onboarding/ with 5 state files + repos/api/ stubs.
       Installed .cursor/rules/project-knowledge-first.mdc
       Next: run /audit

You: /audit

Agent: Overall completion: 12%. Tier 1 (overview): 40%. Tier 3 (request flow): 0%.
       Top gap: no request flow documented for API routes.
       Next: /continue

You: /continue

Agent: Explored src/routes/ and src/middleware/ (11 files).
       Updated repos/api/request-flow.md and progress.md.
       Key finding: auth middleware at src/middleware/auth.ts.
       Next /continue target: database layer (Tier 3).

You: /learn auth

Agent: [Teaching summary with file pointers]
       Updated architecture.md and important-files.md for auth module.
```

Next day:

```
You: /continue

Agent: [Reads progress.md — skips auth, explores database layer]
```

---

## Publish to skills.sh (maintainers)

### 1. Prepare repo

- Sync `templates/` → `skills/start-onboarding/templates/`
- Sync `rules/` → `skills/start-onboarding/rules/`
- Ensure all 6 skills have valid YAML frontmatter

### 2. Push to public GitHub

```bash
cd /path/to/project-onboarding
git init
git add .
git commit -m "Initial publishable project-onboarding skills suite"
gh repo create project-onboarding --public --source=. --push
```

Update install commands in this README with your real `owner/project-onboarding`.

### 3. Test on a fresh repo

```bash
cd /path/to/some-unknown-repo
npx skills add YOUR_GITHUB_USER/project-onboarding -y --skill '*'
```

In Cursor Agent chat:

```
/start
/audit
/continue
```

Verify:
- `docs/onboarding/` exists with state files and per-repo stubs
- `.cursor/rules/project-knowledge-first.mdc` exists
- `.agents/skills/start-onboarding/templates/` contains bundled templates
- Second `/continue` resumes from `progress.md`

Test on one monorepo and one simple single-repo app.

### 4. Verify discovery

```bash
npx skills add YOUR_GITHUB_USER/project-onboarding --list
npx skills find onboarding
```

skills.sh indexes public GitHub repos with `skills/*/SKILL.md` — no separate submission form. Discovery is organic via installs and README quality.

### 5. Optimize discoverability

- GitHub repo topics: `agent-skills`, `cursor`, `onboarding`, `skills-sh`
- Skill descriptions include: onboarding, zero knowledge, new developer, codebase exploration

---

## License

MIT — see [LICENSE](LICENSE)
