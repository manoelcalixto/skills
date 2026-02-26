# Superpower Skills (.superpower) Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add `skills/.superpower/` containing 14 “superpowers” skills adapted to the Agent Skills spec and Codex CLI workflows.

**Architecture:** Copy skills from `/home/k3/git/superpowers/skills`, reorganize supporting files into `scripts/`, `references/`, and `assets/`, then adapt `SKILL.md` instructions to Codex CLI (`update_plan`, `exec_command`, `apply_patch`, sub-agent tools) and validate with `quick_validate.py`.

**Tech Stack:** Markdown, YAML, shell commands, Python validator (`skills/.system/skill-creator/scripts/quick_validate.py`).

---

### Task 1: Create `.superpower` root and verify inputs

**Files:**
- Create: `skills/.superpower/`

**Step 1: Verify source skill list**

Run:
- `ls -1 /home/k3/git/superpowers/skills | sort`

Expected:
- 14 directories matching the design doc list.

**Step 2: Create destination root**

Run:
- `mkdir -p skills/.superpower`

Expected:
- `skills/.superpower` exists.

---

### Task 2: Add MIT license text for reuse

**Files:**
- Read: `/home/k3/git/superpowers/LICENSE`

**Step 1: Capture MIT license content**

Run:
- `cat /home/k3/git/superpowers/LICENSE`

Expected:
- MIT license text.

**Step 2: Decide per-skill license strategy**

Implementation rule:
- Every `skills/.superpower/<skill>/LICENSE.txt` contains the MIT license text (copied verbatim).

---

### Task 3: Import and adapt `brainstorming`

**Files:**
- Create: `skills/.superpower/brainstorming/SKILL.md`
- Create: `skills/.superpower/brainstorming/agents/openai.yaml`
- Create: `skills/.superpower/brainstorming/LICENSE.txt`

**Step 1: Copy SKILL.md**

Run:
- `mkdir -p skills/.superpower/brainstorming`
- `cp /home/k3/git/superpowers/skills/brainstorming/SKILL.md skills/.superpower/brainstorming/SKILL.md`

Expected:
- File exists in destination.

**Step 2: Adapt SKILL.md body for Codex CLI**

Edits (high level):
- Replace “TodoWrite” with `update_plan`.
- Ensure it does not block legitimate repo inspection (allow `exec_command` exploration).
- Keep the “hard gate” spirit but align with Codex workflows.

**Step 3: Add MIT license**

Run:
- `cp /home/k3/git/superpowers/LICENSE skills/.superpower/brainstorming/LICENSE.txt`

**Step 4: Create agents metadata**

Create `skills/.superpower/brainstorming/agents/openai.yaml` with:
- `display_name`, `short_description`, `default_prompt` (quoted strings).

**Step 5: Validate**

Run:
- `python3 skills/.system/skill-creator/scripts/quick_validate.py skills/.superpower/brainstorming`

Expected:
- “Skill is valid!”

---

### Task 4: Import and adapt remaining “SKILL-only” skills

Scope (same file set as Task 3):
- `dispatching-parallel-agents`
- `executing-plans`
- `finishing-a-development-branch`
- `receiving-code-review`
- `using-git-worktrees`
- `using-superpowers`
- `verification-before-completion`
- `writing-plans`

For each skill:

**Step 1: Copy `SKILL.md`**
- `mkdir -p skills/.superpower/<skill>`
- `cp /home/k3/git/superpowers/skills/<skill>/SKILL.md skills/.superpower/<skill>/SKILL.md`

**Step 2: Adapt content**
- Replace “TodoWrite” with `update_plan`.
- Replace “Task tool / Skill tool / Claude Code” with Codex CLI equivalents (`spawn_agent`, `send_input`, `wait`, `exec_command`, `apply_patch`).
- Replace `superpowers:<name>` references with `$<skill-name>`.

**Step 3: Add MIT license**
- `cp /home/k3/git/superpowers/LICENSE skills/.superpower/<skill>/LICENSE.txt`

**Step 4: Add `agents/openai.yaml`**
- Create `skills/.superpower/<skill>/agents/openai.yaml`.

**Step 5: Validate**
- `python3 skills/.system/skill-creator/scripts/quick_validate.py skills/.superpower/<skill>`

---

### Task 5: Import and adapt skills with supporting reference files

#### 5.1 `test-driven-development`

**Files:**
- Create: `skills/.superpower/test-driven-development/references/testing-anti-patterns.md`

Steps:
- Copy `SKILL.md`
- Move `testing-anti-patterns.md` → `references/`
- Update `SKILL.md` links to `references/testing-anti-patterns.md`
- Add MIT `LICENSE.txt` and `agents/openai.yaml`
- Validate with `quick_validate.py`

#### 5.2 `requesting-code-review`

**Files:**
- Create: `skills/.superpower/requesting-code-review/references/code-reviewer.md`

Steps:
- Copy `SKILL.md`
- Move `code-reviewer.md` → `references/`
- Update references inside `SKILL.md`
- Replace “Task tool” instructions with Codex CLI sub-agent workflow
- Add MIT `LICENSE.txt` and `agents/openai.yaml`
- Validate

#### 5.3 `subagent-driven-development`

**Files:**
- Create: `skills/.superpower/subagent-driven-development/references/code-quality-reviewer-prompt.md`
- Create: `skills/.superpower/subagent-driven-development/references/implementer-prompt.md`
- Create: `skills/.superpower/subagent-driven-development/references/spec-reviewer-prompt.md`

Steps:
- Copy `SKILL.md`
- Move prompt files → `references/`
- Update references inside `SKILL.md` (paths + Codex tool usage)
- Add MIT `LICENSE.txt` and `agents/openai.yaml`
- Validate

#### 5.4 `systematic-debugging`

**Files:**
- Create: `skills/.superpower/systematic-debugging/scripts/find-polluter.sh`
- Create: `skills/.superpower/systematic-debugging/references/*.md`
- Create: `skills/.superpower/systematic-debugging/references/condition-based-waiting-example.ts`

Steps:
- Copy `SKILL.md`
- Move `find-polluter.sh` → `scripts/`
- Move all other helper files → `references/`
- Update any file links inside `SKILL.md`
- Replace `superpowers:<...>` references with `$<...>`
- Add MIT `LICENSE.txt` and `agents/openai.yaml`
- Validate

#### 5.5 `writing-skills`

**Files:**
- Create: `skills/.superpower/writing-skills/scripts/render-graphs.js`
- Create: `skills/.superpower/writing-skills/assets/graphviz-conventions.dot`
- Create: `skills/.superpower/writing-skills/references/*.md`

Steps:
- Copy `SKILL.md`
- Move `render-graphs.js` → `scripts/`
- Move `graphviz-conventions.dot` → `assets/`
- Move other docs → `references/`
- Update references inside `SKILL.md`
- Rewrite “Claude Search Optimization” sections to be agent-agnostic and Codex-friendly
- Add MIT `LICENSE.txt` and `agents/openai.yaml`
- Validate

---

### Task 6: Repository-wide checks

**Files:**
- Modify (optional): `README.md`

**Step 1: Ensure no legacy wording remains**

Run:
- `rg -n "TodoWrite|Task tool|Skill tool|Claude Code|superpowers:" skills/.superpower -S`

Expected:
- No matches, or only in clearly-labeled historical examples (preferred: zero).

**Step 2: Validate every skill**

Run:
- `for d in skills/.superpower/*; do python3 skills/.system/skill-creator/scripts/quick_validate.py \"$d\" || exit 1; done`

Expected:
- All “Skill is valid!”

**Step 3 (Optional): Mention `.superpower` in README**

If desired, add a short note that `.superpower` contains workflow/process skills and is not auto-installed.

---

### Task 7: Final verification and summary

**Step 1: List new skills**
- `find skills/.superpower -maxdepth 2 -name SKILL.md -print`

**Step 2: Summarize changes**
- List skills added
- Note validation status
- Call out any deliberate deviations (e.g., no icons)

