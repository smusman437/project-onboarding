# Agent Routing — Project Onboarding Skills

Cross-agent guide for [smusman437/project-onboarding](https://github.com/smusman437/project-onboarding).

## Install all skills

```bash
npx skills add smusman437/project-onboarding --skill '*' -y
```

## Slash command map

Skill folder names match slash commands — type `/start` to invoke the `start` skill.

| Slash Command | Skill folder | Purpose |
|---------------|--------------|---------|
| `/start` | start | Bootstrap onboarding docs |
| `/audit` | audit | Measure onboarding completeness |
| `/continue` | continue | Explore the next priority area |
| `/learn` | learn | Deep-dive into a specific module |
| `/update-docs` | update-docs | Capture new discoveries |
| `/quick-ref` | quick-ref | Find information quickly |
| `/ticket` | ticket | Analyze work before implementation |
| `/qa-review` | qa-review | Validate implementation vs ticket requirements |
| `/review` | review | Review changes before merging (code quality) |
| `/prepare-commits` | prepare-commits | Organize commits logically |

## Workflow order

```
/start → /audit → /continue (repeat) → /learn (as needed)
/ticket → implement → /qa-review → /review → /prepare-commits → /update-docs
/quick-ref anytime for lookups
```

## State file locations

Default: `docs/onboarding/` (override via `project-memory.md` → `docs_path`).

| File | Read by | Written by |
|------|---------|------------|
| project-memory.md | most skills | start, update-docs |
| progress.md | continue, audit, learn | continue, learn, update-docs |
| onboarding-audit.md | continue, audit, learn, ticket, qa-review | audit, continue, update-docs |
| open-questions.md | continue, update-docs, ticket | continue, update-docs |
| repositories-overview.md | audit, quick-ref, ticket | start, update-docs |
| repos/<slug>/*.md | continue, learn, quick-ref, ticket | continue, learn, update-docs |

## Token budget (onboarding skills)

1. Read 2–4 state files before source code
2. Explore **one** repo/module/domain per invocation
3. Write back to state files before ending
4. Never full-repo scan

## Prerequisites

If `docs/onboarding/` does not exist, redirect user to `/start` before onboarding slash commands.

## Cross-agent notes

- Skills use standard `SKILL.md` YAML frontmatter (`name`, `description`)
- Slash-triggered skills set `disable-model-invocation: true`
- Templates and rules bundled in `start` skill directory
