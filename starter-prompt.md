# AI-Native Project Starter Prompt

Copy and paste this to kick off a new project with the full architecture.

---

## Option A - Minimal (just the intent)

```
I want to start a new project: [describe what you're building in 1-2 sentences].
Set up the project architecture for this.
```

Claude will invoke the `ai-native-project` skill automatically and guide you through the rest.

---

## Option B - Full kickoff (more context upfront, fewer questions)

```
I want to start a new AI-native project and set up the full architecture before we write any code.

Project: [what you're building]
For: [who will use it / who will read the work]
Time budget: [a few hours / days / weeks]
Deliverables: [what gets shipped at the end]
Hard constraints: [anything non-negotiable - stack, budget, legal, etc.]
My coding level: [none / beginner / comfortable / professional developer]

Set up the full project architecture - CLAUDE.md, memory.md, docs structure, ROADMAP, and guide me through the first planning phase.
```

---

## Option C - Resuming an existing project

```
Resume the project. Read CLAUDE.md and memory.md first, then tell me where we left off.
```

Or in the terminal: `claude -c` to continue the last session directly.

---

## What gets created

```
your-project/
+-- CLAUDE.md              <- rules, constraints, communication style, sensitive-ops list
+-- memory.md              <- living session-to-session brain
+-- docs/
|   +-- specs/             <- approved designs before any code is written
|   +-- plans/<phase>/     <- plan.md + plan-memory.md (live execution tracker)
|   +-- implementation/
|   |   +-- ROADMAP.md     <- global phase status board
|   +-- cookbooks/         <- reusable QA and review procedures
+-- deliverables/          <- final outputs only
```

Every future session starts by reading this structure. No more lost context.

---

## Open-source / team use

1. Remove `## Communication Style` from CLAUDE.md (it's personal)
2. Add a `CONTRIBUTING.md` explaining the session-start ritual and PR discipline
3. `.claude/settings.json` hooks are per-developer - add `settings.local.json` to `.gitignore`
4. `memory.md` can be gitignored (personal) or committed (shared team context)

---

*Built with the `ai-native-project` skill. Inspired by Andrej Karpathy, Simon Willison, Harper Reed, and Anthropic's Claude Code best practices.*
