---
name: ai-native-project
description: Set up a rigorous, transcript-aware AI-native project architecture from scratch. Use this skill whenever the user wants to start a new project using Claude Code, especially for client work, assignments, or any multi-session effort where the AI transcripts are a deliverable or need to look professional. Triggers on phrases like "start a new project", "set up the architecture", "how should we organize this", "I need to build X with Claude", "project setup", "let's plan this properly" -- even if the user doesn't explicitly mention architecture. If the user is about to jump straight into coding or building without any structure, proactively suggest this skill.
---

You are setting up an AI-native project architecture. This foundation makes every subsequent session faster, more consistent, and impressive to anyone reading the transcripts.

The goal: a system where Claude operates like a senior engineer on a well-run team - spec before code, adversarial review before handoff, living memory across sessions, and clear orchestration decisions that match task complexity. The spec and plan files are more important than the Claude-guidance files. When in doubt, invest more in the planning layer.

---

## Step 1: Understand the user and the project

**This step is mandatory on first run.** Use AskUserQuestion to collect what you need before creating a single file. Two rounds max - batch related questions together.

**Round 1 - User profile (if not already known):**

Ask about:
- Coding/programming experience level: none / beginner / comfortable / professional developer
- Preferred explanation style: "explain everything" / "explain non-obvious things only" / "just the code"
- Any specific tech stack familiarity that's relevant to this project

Why this matters: note the answers in CLAUDE.md under a `## Communication Style` section. Every future session reads this and calibrates accordingly - no more over-explaining to a senior dev or under-explaining to a first-timer.

```markdown
## Communication Style
- Level: [none / beginner / comfortable / professional]
- Style: [explain everything / explain non-obvious only / just the code]
- Notes: [e.g. "knows Python well, new to React", "non-technical founder - avoid jargon"]
```

**Round 2 - Project context:**

1. **What are you building?** (product, audience, purpose)
2. **Who will read the transcripts?** Just you, a client, a hiring committee?
3. **Time budget** - a few hours, days, weeks?
4. **Final deliverables** - what gets shipped?
5. **Any hard constraints** - technical, legal, scope.

If the user has already given you this in the conversation, skip asking and proceed - never ask for information you already have.

---

## Step 2: Classify the work type

Before scaffolding, classify the work - this determines which gates and processes apply:

- **DEFINED**: Requirements are clear. Spec-first mandatory. Full gate sequence applies.
- **EXPLORATORY**: Requirements emerge through building. Timeboxed spike first, then spec from findings. Lighter gates.
- **RESEARCH**: No spec, evidence doc only. Adversarial review on the findings doc.

Most projects are DEFINED at the macro level and EXPLORATORY at the sub-task level. Note the classification in CLAUDE.md's verification protocol.

---

## Step 3: Create the project constitution (CLAUDE.md)

CLAUDE.md is the rulebook. Every future session reads it first. Keep it tight.

**Size discipline (critical):** Target 80 lines, hard max 150. Past ~150 lines, Claude silently ignores large blocks. Ask for every line: "Would removing this cause Claude to make mistakes?" If not, cut it. Use progressive disclosure - CLAUDE.md is an index pointing to `docs/` files, not a self-contained monolith.

**Maintenance rule (Two-Strikes):** Only add a rule to CLAUDE.md after the *second* occurrence of the same mistake. First occurrence = noise; twice = pattern. Prune rules when they're no longer needed.

**Required sections:**

```markdown
# [Project Name] - Claude Project Constitution

[1-2 sentence context: what you're building, for whom, the single success metric]

## Communication Style
- Level: [none / beginner / comfortable / professional]
- Style: [explain everything / explain non-obvious only / just the code]
- Notes: [e.g. "knows Python well, new to React"]

## Core Rules
0. **Living Memory:** Read `memory.md` right after this file. Update whenever a decision is made or state changes.
1. **Spec-First:** Never write code before aligning on the spec. Raise disagreements before building.
2. **Never Assume - Always Verify:** If something is unclear, ask before acting. Never infer what the user wants from ambiguous input. For anything sensitive (data deletion, destructive ops, external calls, auth changes, billing), stop and double-check explicitly before proceeding.
3. **Gate Before Handoff:** Every DEFINED deliverable passes the gate sequence before reaching the user.
4. **Single Source of Truth:** Each fact lives in exactly one file. Everything else links.
5. **Small PRs:** Every change ships as a focused, reviewable PR - one concern per PR, reviewable in under 10 minutes. No bundling unrelated changes. No "misc cleanup" PRs.
6. **Methodical pace:** Complete and verify one step before starting the next. Never start step N+1 while step N is unverified.
7. **Context Discipline:** Use /compact proactively at ~60% context usage. Use /clear between unrelated tasks.
8. **[Project-specific rule - only after it's proven needed]**

## Project Map
- `memory.md` - Session-to-session state and decisions (read after this file)
- `docs/implementation/ROADMAP.md` - Global phase status board
- `docs/specs/` - Approved designs (YYYY-MM-DD-topic.md)
- `docs/plans/<phase>/` - plan.md + plan-memory.md per phase
- `docs/cookbooks/` - Reusable QA and review procedures
- `deliverables/` - Final outputs only

## Verification Protocol
[Gate tier per artifact type: FULL (4 gates) for runnable artifacts, LIGHTWEIGHT (review only) for docs/specs]

## Hard Constraints
[Non-negotiables - only things Claude can't infer from the code]

## Sensitive Operations (always double-check before doing)
- Any deletion of data, files, or database records
- External API calls that cost money or send messages
- Auth / permissions changes
- Anything that affects other users or environments
- Destructive git operations
```

---

## Step 4: Create the planning artifacts (spec.md -> plan)

The planning layer is more valuable than the instruction layer. Before any implementation, create:

**spec.md** - the product spec, written in a fresh brainstorming session. Ask one question at a time to develop a thorough spec. Close with: "Compile our findings into a developer-ready specification." Commit this to `docs/specs/`.

**prompt_plan.md** (for larger projects) - a sequence of LLM prompts that implement the spec step by step. Generated by a reasoning model from the spec.

**todo.md** - a checklistable task breakdown. Checked off as work completes.

The spec is the authoritative reference when implementation diverges from intent. "The spec is the godhead" - Harper Reed.

---

## Step 5: Create memory.md

`memory.md` is the session-to-session brain. Lives at the repo root. Read right after CLAUDE.md every session. Updated continuously - not just at session end.

**Size discipline:** Keep under 150 lines. When it exceeds that, archive the decisions log to `docs/archive/memory-YYYY-MM-DD.md` and reset to current state only.

**Format:**

```markdown
# Project Memory - [Name] (living document)

> Read at session start after CLAUDE.md. Update whenever decisions are made or state changes.
> Keep curated - prune stale entries, never paste transcripts here.
> Rules live in CLAUDE.md only - this file contains state, not rules.

## Current state (last verified: YYYY-MM-DD)
- Phase: [current phase]
- [Exact resume point - updated every session end]

## Active task (WIP = 1)
[Single active task - the thing being worked on right now]

## Decisions log (last verified: YYYY-MM-DD)
| Date | Decision | Rationale |
|------|----------|-----------|
| YYYY-MM-DD | [Decision] | [Why] |

## Learnings and gotchas (last verified: YYYY-MM-DD)
- [Non-obvious things that would bite a future cold session]

## Open questions
- [Unresolved things needing a decision]
```

Rules for memory.md:
- Use absolute dates (not "last Tuesday")
- Add "last verified: YYYY-MM-DD" to each major section - a cold session can see which state claims are fresh
- State and decisions only - rules belong in CLAUDE.md
- The active task field enforces WIP=1: only one thing in flight at a time

---

## Step 6: Skills audit (before significant work begins)

Before starting any significant phase - especially implementation - scan the available skills and note which ones apply in plan-memory.md. This prevents reinventing patterns that already exist as proven procedures.

**How to audit:**
Check your available skills list (shown in system context) against the work ahead:

| Work type | Skills to check for |
|-----------|-------------------|
| UI/UX, frontend, components | `frontend-design`, `web-design-guidelines`, `frontend-patterns` |
| Debugging a broken feature | `systematic-debugging` |
| Code review / quality gate | `code-quality-reviewer`, `adversarial-review` |
| Writing user-facing copy | `copy-review`, `elements-of-style` |
| Research / competitive analysis | `deep-research`, `growth-researcher` |
| Image / creative generation | `creative-capture`, `gen-image` |
| Building new skills | `skill-creator` |
| Landing pages / conversion | `roast-my-landing-page`, `cro-checklist` |

If a relevant skill exists, note it in the plan:
```markdown
## Skills in use this phase
- `frontend-design` - for component styling decisions
- `systematic-debugging` - if we hit a regression
```

Invoke the skill at the right moment during execution - not just once at the start. A debugging skill is most valuable when you actually hit the bug, not when you're still planning.

---

## Step 6b: Scaffold the docs structure

```
docs/
  specs/           # YYYY-MM-DD-topic.md - approved designs
  plans/           # One folder per phase:
  |                #   plan.md (implementation plan)
  |                #   plan-memory.md (live execution tracker)
  implementation/
  |   ROADMAP.md   # Global status board only
  cookbooks/       # QA procedures, review procedures
  archive/         # Archived memory snapshots
deliverables/      # Final outputs only
```

**ROADMAP.md** - the single global status board:

```markdown
# Project ROADMAP

| Phase | Spec | Plan | Status | Notes |
|-------|------|------|--------|-------|
| 0: Scaffold | link | link | done | |
| 1: [name] | link | link | in-progress | |
```

Statuses: `pending` / `in-progress` / `blocked` / `done`

Initialize git, set `.gitattributes` to pin text files to LF. See `docs/cookbooks/project-init.md` for the git setup steps.

**Directory-scoped CLAUDE.md files** - create these when a subfolder grows large enough to have its own concerns:

```
project/
+-- CLAUDE.md              <- project-level rules (global)
+-- src/
|   +-- CLAUDE.md          <- "in this folder: React components, Tailwind only, no inline styles"
|   +-- api/
|       +-- CLAUDE.md      <- "in this folder: all routes need auth middleware, validate with Zod"
+-- scripts/
    +-- CLAUDE.md          <- "in this folder: Node.js only, no external deps, must be idempotent"
```

**When to create one:** A subfolder earns its own CLAUDE.md when it has 10+ files, its own tech stack or conventions that differ from the root, or when Claude keeps making the same mistakes in that area. Use the Two-Strikes Rule: second mistake in the same folder = time for a local CLAUDE.md.

**What goes in a directory CLAUDE.md:** Only what's specific to that folder. Stack choice, naming conventions, invariants, gotchas. Keep it under 30 lines.

---

## Step 7: Implementation mode gate (before any non-trivial work)

Before starting any significant implementation - anything larger than a small isolated change - stop and ask the user how they want to proceed. Use AskUserQuestion with these three options:

**Option A - This session (you watch)**
Claude implements step by step in this conversation. You see every change as it happens and can redirect at any point. Best for: smaller features, anything you want tight control over, learning how something works.

**Option B - Agent teams (parallel)**
Multiple specialized agents work on different parts simultaneously. Claude fans out the work, each agent handles one concern, results are merged and reviewed. Best for: well-defined work with clear independent parts. Faster, but requires a solid spec so agents don't conflict.

**Option C - Dynamic workflow (orchestrated)**
A structured multi-phase orchestration: research -> design -> implement -> verify -> synthesize. Each phase completes before the next starts. Results are adversarially verified before you see them. Best for: complex multi-phase work where correctness matters more than speed.

Ask this gate question any time a plan contains more than ~3 interdependent steps or will touch more than 5 files. For small changes (a single component, a config tweak, a bug fix), skip the gate and proceed.

When the user chooses, note it in plan-memory.md under `## Execution mode`.

---

## Step 8: Plan lifecycle - how work gets tracked

```
Brainstorm -> spec.md written
  -> [ask user: "Want me to adversarially review this spec before you approve it?"]
  -> (if yes) Adversarial review -> fixes -> re-check
  -> User approval -> docs/plans/<phase>/plan.md
  -> plan-memory.md (created, updated live during execution)
  -> Build completes
  -> [ask user: "Implementation is done. Want an adversarial review before I hand this to you?"]
  -> (if yes) Gate sequence -> fixes -> re-check
  -> Commit -> ROADMAP updated
```

**Always ask, never assume.** Don't auto-run adversarial review - offer it at the two natural checkpoints above. The user may skip it for small changes or time-pressured iterations. Note the choice in plan-memory.md.

**plan-memory.md template:**

```markdown
# Plan Memory: [Phase Name]

**Status:** in-progress
**Started:** YYYY-MM-DD
**Last updated:** YYYY-MM-DD
**Task size:** S / M / L
**Execution mode:** [this session / agent teams / dynamic workflow]
**Ralph Loop Status:** not-running / running / stopped

## ACTIVE TASK (WIP = 1)
[The single task currently in progress - only one item here at a time]

## Resume point
LAST COMPLETED: [specific file, function, or action]
OPEN DECISION: [question blocking next step, or "none"]
NEXT ACTION: [specific, concrete, single step]

## Completed steps
- [step] - done YYYY-MM-DD

## Remaining steps
- [ ] [step]

## Skills in use this phase
- [skill name] - [why / when to invoke]

## Decisions made mid-plan
| Date | Decision | Rationale |
|------|----------|-----------|

## Working notes (volatile - not SSoT, pruned at phase end)
[Scratch space for current blockers, mid-task state, hunches - can be messy]

## Blockers
- [None / list blockers]
```

---

## Step 9: Gate sequence and PR discipline

**When to offer the gate (two checkpoints):**

1. **After a spec or plan is written** - before user approval: "I've written the spec. Want me to run an adversarial review on it first?"
2. **After implementation is complete** - before handing off: "Done. Want me to adversarially review this before you take it?"

If the user says no, proceed and note the skip in plan-memory.md. Never run review silently without asking.

**PR discipline:**
- One PR = one concern. A button style change and a data model fix are two PRs.
- Every PR reviewable in under 10 minutes. If it can't be, split it.
- PR description: what changed, why, and how to verify it works.
- Never bundle cleanup with a feature change.

**FULL GATE** (runnable artifacts: apps, APIs, scripts, HTML):
1. Adversarial review (4 personas)
2. Automated QA (evidence table, not just "pass")
3. User manual QA
4. Commit (with memory.md and ROADMAP.md updated)

**LIGHTWEIGHT GATE** (documents: specs, plans, write-ups):
1. Adversarial review only
2. Commit

**Pre-implementation gate:** Before any task touching 3+ files, run in plan mode and get explicit user approval before touching files.

### Adversarial review - 4 personas

Each uses a genuinely different cognitive mode:

**1. Spec Fidelity Checker** - reads deliverable against spec line by line. Finds scope creep, missing requirements, wrong interpretation. Verdict: BLOCK / CONCERNS / CLEAN.

**2. New Hire** - tries to use the deliverable from the doc alone, no assumed context. Finds ambiguities, missing context, broken funnels. Verdict: BLOCK / CONCERNS / CLEAN.

**3. Security Auditor** - threat models the deliverable. Finds XSS, injection, policy violations, data exposure. Verdict: BLOCK / CONCERNS / CLEAN.

**4. Simplicity Auditor** - asks "what is the simplest implementation that satisfies the spec?" Finds overengineering, unnecessary coupling, abstraction without payoff. Verdict: SIMPLER_AVAILABLE / APPROPRIATE_COMPLEXITY.

**Re-review rule:** BLOCK verdicts require full re-review of blocked items after fixes. CONCERNS verdicts require targeted re-check only.

---

## Step 10: Session-start ritual

Read in this order (most task-specific first, sharpest attention last):

1. **plan-memory.md** (active phase) - exact resume point + active task
2. **memory.md** - current state + recent decisions
3. **CLAUDE.md** - rules and constraints
4. **ROADMAP.md** - only if changing phases or unsure of status

**Faster options:**
- `claude -c` resumes the most recent session with conversation memory. Try this first.
- `/compact` at ~60% context usage (preserve: active constraints, current bugs, architectural decisions)
- `/clear` between unrelated tasks

**Automate with hooks** - create `.claude/settings.json`:
```json
{
  "hooks": {
    "SessionStart": [{
      "matcher": "",
      "hooks": [{"type": "command", "command": "echo '=== SESSION START ===' && cat plan-memory.md 2>/dev/null && cat memory.md"}]
    }],
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{"type": "command", "command": "echo 'File written - update plan-memory.md if step completed'"}]
    }]
  }
}
```

---

## Step 11: Orchestration - match complexity to tool

| Task type | Mode | When |
|-----------|------|------|
| Focused single-concern work | Solo | Default for most tasks |
| Algorithm/engine tuning | Ralph-loop | Corpus + expectations + unit test loop |
| 3+ independent parallel files | Agent team | Brief each agent fully, use git worktrees |
| Pre-design decisions | Research agent | OBSERVED / REPORTED / INFERRED output format |

**Ralph-loop:** Author engine as pure function. Write corpus + expectations.md. Loop unit tests until all pass. Embed verbatim. Add extraction diff to QA cookbook. Target 40-60% context window utilization per iteration.

**Agent team:** One scoped task per agent. Brief like a new hire. Spec compliance review first, then code quality. Git worktrees for file isolation. Never merge until both reviews pass.

**Research agent evidence labels:**
- **OBSERVED:** directly seen in primary source
- **REPORTED:** claimed by secondary source
- **INFERRED:** model's own reasoning

---

## Key principles

**Planning layer > instruction layer.** Invest more in spec.md, prompt_plan.md, and todo.md than in CLAUDE.md. Planning artifacts survive session resets; instructions just reduce drift.

**Context is your primary resource.** Fresh focused context beats long accumulated sessions. Favor task-scoped sessions with intentional resets.

**Declarative over imperative.** Give Claude success criteria, not step-by-step procedures. LLMs loop toward goals better than they follow procedures.

**SSoT has a scope.** Apply it to stable facts (rules, API contracts, architecture decisions). For volatile state (today's blocker, scratch notes), enforce lightly - friction drives staleness.

**Tests first, review second.** Automated tests Claude can run are more reliable than review gates. Gates are the second line of defense.
