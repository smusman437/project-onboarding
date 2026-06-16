# Agent Routing — Project Onboarding Skills

Cross-agent guide for [smusman437/project-onboarding](https://github.com/smusman437/project-onboarding).

## Install all skills

```bash
npx skills add smusman437/project-onboarding --skill '*' -y
```

## Slash command map

| Slash Command | Skill | Purpose |
|---------------|-------|---------|
| `/start` | start-onboarding | Bootstrap onboarding docs |
| `/audit` | audit-project | Measure onboarding completeness |
| `/continue` | continue-onboarding | Explore the next priority area |
| `/learn` | learn-module | Deep-dive into a specific module |
| `/update-docs` | update-docs | Capture new discoveries |
| `/quick-ref` | quick-reference | Find information quickly |
| `/ticket` | ticket-analysis | Analyze work before implementation |
| `/review` | review-work | Review changes before merging |
| `/prepare-commits` | prepare-commits | Organize commits logically |

## Workflow order

```
/start → /audit → /continue (repeat) → /learn (as needed)
/ticket → implement → /review → /prepare-commits → /update-docs
/quick-ref anytime for lookups
```

## State file locations

Default: `docs/onboarding/` (override via `project-memory.md` → `docs_path`).

| File | Read by | Written by |
|------|---------|------------|
| project-memory.md | most skills | start, update-docs |
| progress.md | continue, audit, learn | continue, learn, update-docs |
| onboarding-audit.md | continue, audit, learn, ticket | audit, continue, update-docs |
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
- Templates and rules bundled in `start-onboarding` skill directory
