<div align="center">

![AI-Native Project Architecture](assets/hero.png)

# ai-native-project

**A Claude Code skill that sets up a rigorous, transcript-aware AI-native project architecture from scratch.**

Spec-first. Memory-driven. Adversarially reviewed. Session-to-session continuity built in.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-8B5CF6)](https://docs.anthropic.com/claude-code)
[![Skill Version](https://img.shields.io/badge/version-1.0.0-06B6D4)](#)

[What it does](#what-it-does) - [Quick start](#quick-start) - [What gets created](#what-gets-created) - [The philosophy](#the-philosophy) - [Install](#install)

</div>

---

## What it does

Every time you start a new project with Claude Code, you're one missed context reset away from Claude forgetting everything. This skill fixes that.

When you trigger it, Claude sets up a complete AI-native project architecture before writing a single line of code:

- **CLAUDE.md** - the project constitution. Rules, constraints, your communication style, sensitive-ops checklist. Claude reads it first every session.
- **memory.md** - the living brain. Session-to-session state, decisions, open questions. Pruned, never bloated.
- **docs/** - a structured folder system: specs before implementation, phase-level plan trackers, QA cookbooks.
- **Adversarial review gates** - 4-persona review (Spec Fidelity, New Hire, Security, Simplicity) offered at the two moments that matter: after spec, and after implementation.
- **Implementation mode choice** - before any big build, Claude asks: this session, agent teams, or dynamic workflow?

The result: Claude operates like a senior engineer on a well-run team across every session, not just the first one.

---

## Quick start

### Option A - Minimal

```
I want to start a new project: [describe what you're building].
Set up the project architecture for this.
```

Claude detects the intent and invokes the skill automatically.

### Option B - Full kickoff (fewer questions)

```
I want to start a new AI-native project and set up the full architecture before we write any code.

Project: [what you're building]
For: [who will use it / who will read the work]
Time budget: [a few hours / days / weeks]
Deliverables: [what gets shipped at the end]
Hard constraints: [anything non-negotiable]
My coding level: [none / beginner / comfortable / professional developer]

Set up the full project architecture.
```

### Option C - Resume existing project

```
Resume the project. Read CLAUDE.md and memory.md first, then tell me where we left off.
```

Or in the terminal: `claude -c` to continue the last session directly.

See [`starter-prompt.md`](starter-prompt.md) for all three options with copy-paste prompts.

---

## What gets created

```
your-project/
+-- CLAUDE.md              <- rules, constraints, your communication style, sensitive-ops list
+-- memory.md              <- living session-to-session brain (state only, never bloated)
+-- docs/
|   +-- specs/             <- approved designs, written before any code
|   +-- plans/
|   |   +-- phase-name/
|   |       +-- plan.md          <- implementation plan
|   |       +-- plan-memory.md   <- live execution tracker (WIP=1, exact resume point)
|   +-- implementation/
|   |   +-- ROADMAP.md     <- global phase status board
|   +-- cookbooks/         <- reusable QA and review procedures
+-- deliverables/          <- final outputs only
```

Every future session reads this structure first. No more lost context.

---

## The 11 steps

| Step | What happens |
|------|--------------|
| 1 | AskUserQuestion - coding level, explanation style, project context |
| 2 | Work type classification: DEFINED / EXPLORATORY / RESEARCH |
| 3 | CLAUDE.md constitution with Communication Style + 8 core rules |
| 4 | Harper Reed planning pipeline: spec.md -> prompt_plan.md -> todo.md |
| 5 | memory.md with WIP=1 discipline and "last verified" dates |
| 6 | Skills audit - map work type to available skills before building |
| 6b | docs/ scaffold + directory-scoped CLAUDE.md guidance |
| 7 | Implementation mode gate: this session / agent teams / dynamic workflow |
| 8 | Plan lifecycle with plan-memory.md (3-field resume point) |
| 9 | Gate sequence: FULL (4 gates) or LIGHTWEIGHT (review only) + adversarial review |
| 10 | Session-start ritual with automation hooks |
| 11 | Orchestration patterns: solo, Ralph-loop, agent team, research agent |

---

## The adversarial review

Offered at two moments - not automatic, always a choice:

1. **After spec is written**: "Want me to adversarially review this before you approve it?"
2. **After implementation**: "Done. Want me to review this before I hand it to you?"

Four personas, each with a different cognitive mode:

| Persona | What they check | Verdict |
|---------|----------------|---------|
| Spec Fidelity Checker | Spec vs. implementation - scope creep, missing requirements | BLOCK / CONCERNS / CLEAN |
| New Hire | Usability from the doc alone, no assumed context | BLOCK / CONCERNS / CLEAN |
| Security Auditor | XSS, injection, data exposure, auth issues | BLOCK / CONCERNS / CLEAN |
| Simplicity Auditor | Overengineering, coupling, abstraction without payoff | SIMPLER_AVAILABLE / OK |

---

## The philosophy

**Planning layer > instruction layer.** A great spec.md beats a perfectly crafted CLAUDE.md. Invest more in planning than in instructions.

**Context is your primary resource.** Fresh, focused context beats long accumulated sessions. Design for intentional resets.

**Never assume - always verify.** Unclear = ask. Sensitive operation = double-check explicitly. The skill bakes this into CLAUDE.md for every project.

**Tests first, review second.** Automated tests Claude can run are more reliable than review gates. Gates are the second line of defense.

**Two-Strikes Rule.** Only add a CLAUDE.md rule after the second occurrence of the same mistake. First = noise, twice = pattern.

Inspired by the workflows of [Harper Reed](https://harper.blog/2025/02/16/my-llm-codegen-workflow-atm/), [Simon Willison](https://simonwillison.net/), and [Andrej Karpathy](https://karpathy.ai/).

---

## Install

### Via Claude Code skills

```bash
# Copy to your Claude Code skills directory
git clone https://github.com/FuzulsFriend/ai-native-project.git \
  ~/.claude/skills/ai-native-project
```

Then trigger it with any of the prompts in [starter-prompt.md](starter-prompt.md).

### Manual install

1. Download `SKILL.md` from this repo
2. Place it at `~/.claude/skills/ai-native-project/SKILL.md`
3. Restart Claude Code

---

## Files in this repo

| File | Purpose |
|------|---------|
| [`SKILL.md`](SKILL.md) | The skill itself - all 11 steps |
| [`starter-prompt.md`](starter-prompt.md) | Copy-paste prompts to trigger the skill |
| [`examples/CLAUDE.md.example`](examples/CLAUDE.md.example) | Example CLAUDE.md for a SaaS project |
| [`examples/memory.md.example`](examples/memory.md.example) | Example memory.md mid-project |
| [`examples/plan-memory.md.example`](examples/plan-memory.md.example) | Example plan-memory.md mid-phase |

---

## Team / open-source use

1. Remove `## Communication Style` from CLAUDE.md (it's personal to each developer)
2. Add a `CONTRIBUTING.md` explaining the session-start ritual and PR discipline
3. `.claude/settings.json` hooks are per-developer - add `settings.local.json` to `.gitignore`
4. `memory.md` can be gitignored (personal state) or committed (shared team context)

---

<div align="center">

Built with Claude Code. Inspired by the best AI-native engineering workflows out there.

**[Get the starter prompt](starter-prompt.md)** - **[Read the full skill](SKILL.md)**

</div>
