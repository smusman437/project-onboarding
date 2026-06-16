# Agent Routing — Project Onboarding Skills

This file helps non-Cursor agents (Codex, GitHub Copilot, Windsurf, Claude Code, etc.) use the onboarding skill suite consistently.

## Install

```bash
npx skills add <owner>/project-onboarding
```

Skills install to agent-specific paths automatically (e.g. `.cursor/skills/`, `.agents/skills/`).

## Slash command map

| User says | Load skill | Action |
|-----------|------------|--------|
| `/start` | start-onboarding | Bootstrap `docs/onboarding/` + install `.cursor/rules/project-knowledge-first.mdc` |
| `/audit` | audit-project | Gap report + completion % |
| `/continue` | continue-onboarding | Explore one next-priority area |
| `/learn [module]` | learn-module | Mentor deep-dive on one module |
| `/update-docs` | update-docs | Capture new knowledge to state files |
| `/quick-ref [query]` | quick-reference | Lookup from state files, no full scan |

## State file locations

Default path: `docs/onboarding/` (override via `project-memory.md` → `docs_path`).

| File | Read by | Written by |
|------|---------|------------|
| project-memory.md | all skills | start, update-docs |
| progress.md | continue, audit, learn | continue, learn, update-docs |
| onboarding-audit.md | continue, audit, learn | audit, continue, update-docs |
| open-questions.md | continue, update-docs | continue, update-docs |
| repositories-overview.md | audit, quick-ref | start, update-docs |
| repos/<slug>/*.md | continue, learn, quick-ref | continue, learn, update-docs |

## Token budget (all skills)

1. Read 2–4 state files before source code
2. Explore **one** repo/module/domain per invocation
3. Write back to state files before ending
4. Never full-repo scan
5. Skip Tier 7 (DevOps) unless user has access

## Workflow order

```
/start → /audit → /continue (repeat) → /learn (as needed) → /update-docs (after work)
/quick-ref anytime for lookups
```

## Prerequisites

If `docs/onboarding/` does not exist, redirect user to `/start` before any other command.

## Cross-agent notes

- Skills use standard `SKILL.md` YAML frontmatter (`name`, `description`)
- Slash-triggered skills set `disable-model-invocation: true` — require explicit invocation
- Templates and rules are bundled inside the installed `start-onboarding` skill (`templates/`, `rules/`); `/start` copies scaffolds into the target repo and installs the Project-Knowledge-First rule
