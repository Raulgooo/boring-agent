---
description: Run SEED guided project ideation, graduation, launch, status, or custom type setup
argument-hint: [ideate|graduate|launch|status|add-type] [project-name-or-idea]
allowed-tools: [Read, Write, Glob, Grep, Edit, Bash, AskUserQuestion]
---

# SEED

The user invoked `/seed $ARGUMENTS`.

Read the first existing SEED skill file from this list, then route the request using the command and routing rules in that skill:

1. `.codex/skills/seed/SKILL.md`
2. `C:\Users\raulg\.codex\skills\seed\SKILL.md`

If no subcommand is provided, treat the request as `/seed ideate`.

Preserve the user's arguments exactly as the project idea, project name, or subcommand context.
