# Superpower Skills (.superpower) — Design

**Date:** 2026-02-26  
**Repo:** `/home/k3/git/skills`  
**Source repo:** `/home/k3/git/superpowers`  
**Spec reference:** `agentskills` spec (local) at `/home/k3/git/agentskills/docs/specification.mdx`

## Goal

Add a new skills category `skills/.superpower/` to this repository and adapt all “superpowers” skills from `/home/k3/git/superpowers/skills` to:

- Follow the Agent Skills directory specification (required `SKILL.md`; optional `scripts/`, `references/`, `assets/`).
- Follow the same packaging conventions as existing skills in this repo (`skills/.curated` and `skills/.system`), including `agents/openai.yaml` and per-skill `LICENSE.txt`.
- Make the skill instructions **Codex CLI-native**, replacing:
  - “TodoWrite” → `update_plan`
  - “Task tool” / “Claude Code” / “Skill tool” references → `spawn_agent`, `send_input`, `wait`, `exec_command`, `apply_patch`, and “read files from disk”.

## Non-goals

- Change the behavior/meaning of the superpowers workflows (keep intent and steps).
- Add icons or brand assets unless required (most existing `.system` skills omit icons).
- Modify `.curated` or `.system` skills.
- Modify the `skill-installer` behavior (default remains `.curated`).

## Inputs

The source skills directory contains 14 skills:

- `brainstorming`
- `dispatching-parallel-agents`
- `executing-plans`
- `finishing-a-development-branch`
- `receiving-code-review`
- `requesting-code-review`
- `subagent-driven-development`
- `systematic-debugging`
- `test-driven-development`
- `using-git-worktrees`
- `using-superpowers`
- `verification-before-completion`
- `writing-plans`
- `writing-skills`

Some skills contain additional files that must be organized per spec (examples: `*.sh`, `*.js`, `*.dot`, prompt templates, supporting markdown).

## Proposed structure

Create:

```
skills/
  .superpower/
    <skill-name>/
      SKILL.md
      LICENSE.txt
      agents/openai.yaml
      scripts/        # optional
      references/     # optional
      assets/         # optional
```

### Resource mapping rules

- Executable helpers → `scripts/`
  - Example: `systematic-debugging/find-polluter.sh`
  - Example: `writing-skills/render-graphs.js`
- Supporting docs, prompts, long-form guidance → `references/`
  - Example: `requesting-code-review/code-reviewer.md`
  - Example: `systematic-debugging/*.md`
- Static templates/data/diagrams → `assets/`
  - Example: `writing-skills/graphviz-conventions.dot`

All references inside `SKILL.md` must be updated to the new relative paths (e.g. `references/defense-in-depth.md`).

### Codex CLI adaptations

In `SKILL.md` bodies:

- Replace “TodoWrite” checklists with `update_plan` usage.
- Replace “Task tool” instructions with Codex sub-agent calls:
  - `spawn_agent` to start an agent
  - `send_input` to provide context
  - `wait` to block for completion
- Replace “superpowers:<name>” / “superpowers:<skill>” references with `$<skill-name>` to match the `name:` field constraints from the spec (no `:`).
- Prefer repository-relative paths and commands in backticks.

## Licenses

The upstream repo `/home/k3/git/superpowers` is licensed under **MIT** (`/home/k3/git/superpowers/LICENSE`). To avoid changing terms or mixing licenses incorrectly:

- Each skill under `skills/.superpower/<skill>/` will include `LICENSE.txt` containing the MIT license text.
- (Optional but recommended) `SKILL.md` frontmatter will include a `license` field pointing at MIT terms (e.g. `license: MIT (see LICENSE.txt)`).

This keeps licensing explicit and per-skill, consistent with this repo’s existing “license per skill” convention (even though `.curated` and `.system` skills mostly use Apache-2.0).

## Validation

Use the repo’s existing validator:

- `python3 skills/.system/skill-creator/scripts/quick_validate.py skills/.superpower/<skill>`

Validation goals:

- `SKILL.md` frontmatter contains only allowed keys (`name`, `description`, optional `license`, etc.)
- `name` is hyphen-case and <= 64 chars
- `description` is <= 1024 chars and contains no `<` / `>` characters

## Open questions / decisions

- **Icons:** default to none (consistent with existing `.system` skills).
- **README updates:** optional; can mention `.superpower` exists without changing installer defaults.

