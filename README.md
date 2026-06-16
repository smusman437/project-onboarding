# Project Onboarding Skills

**Go from zero knowledge to advanced project understanding with minimal tokens per session** — persistent state files, bounded slash commands, and an always-on doc-first rule replace full-repo re-scans every chat.

| | |
|---|---|
| **GitHub** | [github.com/smusman437/project-onboarding](https://github.com/smusman437/project-onboarding) |
| **skills.sh** | [skills.sh/smusman437/project-onboarding](https://www.skills.sh/smusman437/project-onboarding) |
| **Skills** | 9 installable slash commands |
| **Agents** | Cursor, Codex, GitHub Copilot, Windsurf, Claude Code, and more |

Works on any codebase — monorepo, multi-repo, or single-repo — with no prior project knowledge required.

---

## Install all skills (recommended)

Use `--skill '*'` to install every skill in one command (no picker prompt):

```bash
npx skills add smusman437/project-onboarding --skill '*' -y
```

Cursor only:

```bash
npx skills add smusman437/project-onboarding --skill '*' -y -a cursor
```

Global (available in all projects):

```bash
npx skills add smusman437/project-onboarding --skill '*' -y -g
```

Verify the package before installing:

```bash
npx skills add smusman437/project-onboarding --list
```

Confirm after install:

```bash
npx skills list
```

You should see short names: `start`, `audit`, `continue`, `learn`, `update-docs`, `quick-ref`, `ticket`, `review`, `prepare-commits`. Type `/start` in Cursor — not `/start-onboarding`.

### Updating from an older install (renamed skills)

If you previously installed skills with long names (`start-onboarding`, `audit-project`, etc.), remove them and reinstall:

```bash
npx skills remove start-onboarding audit-project continue-onboarding learn-module quick-reference ticket-analysis review-work update-docs prepare-commits -y
npx skills add smusman437/project-onboarding --skill '*' -y
npx skills update -y
```

Then reload Cursor: **Cmd+Shift+P → Developer: Reload Window**.

Then bootstrap your target repo in Cursor Agent:

```
/start
```

---

## What is init?

This package uses **init** in three ways:

### 1. Package init (contributors)

Scaffold a new skill when extending this repo:

```bash
npx skills init my-new-skill --path skills/my-new-skill
```

If you edit repo-root `templates/` or `rules/`, sync copies into `skills/start/templates/` and `skills/start/rules/` before pushing.

### 2. Project init (users)

After installing skills, run `/start` in any target repo. It:

- Creates `docs/onboarding/` with state files and per-repo doc stubs
- Detects stack and layout (monorepo / multi-repo / single-repo)
- Installs `.cursor/rules/project-knowledge-first.mdc`

### 3. Project-Knowledge-First rule (auto-installed)

`/start` copies an always-on Cursor rule that enforces:

- Read `docs/onboarding/` before analyzing code
- Explore only repos/modules relevant to the task
- Plan before implementation; prefer minimal diffs
- Run `/update-docs` after discovering new knowledge

---

## Slash commands (9 skills)

| Skill | Slash Command | Purpose |
|-------|---------------|---------|
| start | `/start` | Bootstrap onboarding docs |
| audit | `/audit` | Measure onboarding completeness |
| continue | `/continue` | Explore the next priority area |
| learn | `/learn` | Deep-dive into a specific module |
| update-docs | `/update-docs` | Capture new discoveries |
| quick-ref | `/quick-ref` | Find information quickly |
| ticket | `/ticket` | Analyze work before implementation |
| review | `/review` | Review changes before merging |
| prepare-commits | `/prepare-commits` | Organize commits logically |

> **Note:** The skill folder name matches the slash command — e.g. type `/start` to invoke the `start` skill.

All slash skills use `disable-model-invocation: true` — invoke them explicitly in chat.

---

## 5-minute quickstart

```
1. npx skills add smusman437/project-onboarding --skill '*' -y
2. /start          → bootstrap docs/onboarding/ + rule
3. /audit          → see completion % and gaps
4. /continue       → explore one area, update progress.md
5. /learn auth     → deep-dive a module (optional)
```

**Daily workflow:**

```
/ticket            → plan from issue (paste Jira/Linear/GitHub ticket)
... implement ...
/review            → pre-merge check
/prepare-commits   → commit groups with messages
/update-docs       → capture new knowledge
/continue          → next onboarding area
/quick-ref         → quick lookup anytime
```

Come back tomorrow — `/continue` reads `progress.md` and picks up where you left off.

---

## Example session

```
You: /start

Agent: Detected single-repo Node/Express app.
       Created docs/onboarding/ with state files + repos/api/ stubs.
       Installed .cursor/rules/project-knowledge-first.mdc
       Next: /audit

You: /audit

Agent: Overall completion: 12%. Top gap: no request flow documented.
       Next: /continue

You: /continue

Agent: Explored src/routes/ (11 files). Updated progress.md.
       Next target: database layer.

You: /ticket PROJ-123 [paste ticket]

Agent: [Requirements, affected modules, implementation plan — no code]

You: /review

Agent: [Pre-merge findings: patterns, edge cases, test gaps]

You: /prepare-commits

Agent: [2 proposed commit groups with messages and git commands]
```

---

## State files (created in your target repo)

Default path: `docs/onboarding/` (overridable in `project-memory.md`).

| File | Role |
|------|------|
| `project-memory.md` | Source of truth — config, stack, conventions |
| `progress.md` | Session log; resume pointer for `/continue` |
| `onboarding-audit.md` | Gap analysis, tier scores, completion % |
| `open-questions.md` | Blocked items and unanswered questions |
| `repositories-overview.md` | Index of discovered repos/modules |
| `repos/<slug>/overview.md` | High-level purpose of one repo/module |
| `repos/<slug>/architecture.md` | Layers, components, dependencies |
| `repos/<slug>/request-flow.md` | End-to-end flow trace |
| `repos/<slug>/common-patterns.md` | Conventions and patterns |
| `repos/<slug>/integrations.md` | External systems and APIs |
| `repos/<slug>/important-files.md` | "Start here" file index |

Each file has one role — do not duplicate facts across files.

---

## Token efficiency

| Without these skills | With these skills |
|----------------------|-------------------|
| Re-explain the repo every chat | Read 2–4 state files (~few KB) |
| Full-tree scan per question | One bounded area per `/continue` |
| Context lost between sessions | `progress.md` compounds over days/weeks |
| No doc-first discipline | Always-on Project-Knowledge-First rule |

Each onboarding skill enforces: read state first → explore one area → write back → teach summary.

---

## Repo structure

```
project-onboarding/
├── README.md
├── AGENTS.md                 # Cross-agent routing
├── LICENSE
├── rules/
│   └── project-knowledge-first.mdc
├── skills/                   # 9 installable skills (folder name = slash command)
│   ├── start/
│   │   ├── SKILL.md
│   │   ├── templates/        # Bundled — copied to target repo on /start
│   │   └── rules/            # Bundled — installed on /start
│   ├── continue/
│   ├── audit/
│   ├── learn/
│   ├── update-docs/
│   ├── quick-ref/
│   ├── ticket/
│   ├── review/
│   └── prepare-commits/
└── templates/                # Contributor source (sync to skills/start/)
    └── per-repo/
```

---

## skills.sh listing

Your package is **installable immediately** via the CLI. The [skills.sh](https://skills.sh) page is created after the first install telemetry event.

### Confirm package works (always reliable)

```bash
npx skills add smusman437/project-onboarding --list
```

Expected: `Found 9 skills`

### Get listed on skills.sh

1. Run an install (triggers anonymous telemetry):
   ```bash
   npx skills add smusman437/project-onboarding --skill '*' -y
   ```
2. Wait 15–30 minutes, then open:
   [skills.sh/smusman437/project-onboarding](https://www.skills.sh/smusman437/project-onboarding)
3. Install count increases with each user who runs `npx skills add`

> **Note:** Homepage search on [skills.sh](https://skills.sh) ranks by total installs. New skills with few installs may not appear in top search results even when indexed — use the direct URL or `--list` to verify.

### Related package

Also see [smusman437/prompt-master](https://www.skills.sh/smusman437/prompt-master) for tool-specific prompt building and repair.

---

## Contributing

1. Edit skill in `skills/<name>/SKILL.md`
2. If changing templates/rules, sync to `skills/start/`
3. Test locally:
   ```bash
   npx skills add /path/to/project-onboarding --list
   npx skills add /path/to/project-onboarding --skill '*' -y
   ```
4. Push to GitHub; run install once to refresh skills.sh index

---

## License

MIT — see [LICENSE](LICENSE)
