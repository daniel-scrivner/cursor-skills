```
 ██████╗██╗   ██╗██████╗ ███████╗ ██████╗ ██████╗ 
██╔════╝██║   ██║██╔══██╗██╔════╝██╔═══██╗██╔══██╗
██║     ██║   ██║██████╔╝███████╗██║   ██║██████╔╝
██║     ██║   ██║██╔══██╗╚════██║██║   ██║██╔══██╗
╚██████╗╚██████╔╝██║  ██║███████║╚██████╔╝██║  ██║
 ╚═════╝ ╚═════╝ ╚═╝  ╚═╝╚══════╝ ╚═════╝ ╚═╝  ╚═╝
              ███████╗██╗  ██╗██╗██╗     ██╗     ███████╗
              ██╔════╝██║ ██╔╝██║██║     ██║     ██╔════╝
              ███████╗█████╔╝ ██║██║     ██║     ███████╗
              ╚════██║██╔═██╗ ██║██║     ██║     ╚════██║
              ███████║██║  ██╗██║███████╗███████╗███████║
              ╚══════╝╚═╝  ╚═╝╚═╝╚══════╝╚══════╝╚══════╝
```

# Cursor Skills

> **A curated collection of AI-powered workflows for Cursor IDE**

These skills transform Cursor from a code assistant into a full development workflow automation system — from PRD creation to task breakdown to autonomous implementation to PR management.

---

## What Are Cursor Skills?

Cursor Skills are `.mdc` files that define reusable AI behaviors. When you invoke a skill with `@skill-name`, Cursor loads those instructions and follows the defined workflow.

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   @generate-prd → @generate-tasks → @ralph-manual → @pr-workflow │
│                                                                 │
│   ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐   │
│   │  PRD    │────▶│  Tasks  │────▶│  Code   │────▶│  Merge  │   │
│   │  Doc    │     │  List   │     │  Impl   │     │  PR     │   │
│   └─────────┘     └─────────┘     └─────────┘     └─────────┘   │
│                                                                 │
│   Full feature lifecycle, AI-assisted at every stage            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Available Skills

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| [`@generate-prd`](#generate-prd) | Create Product Requirements Documents | Starting a new feature |
| [`@generate-tasks`](#generate-tasks) | Break PRDs into actionable task lists | After PRD is approved |
| [`@ralph-hybrid`](#ralph-wiggum-technique) | Autonomous coding loop (Claude CLI) | Overnight/AFK work |
| [`@ralph-manual`](#ralph-wiggum-technique) | Human-in-the-loop task runner | Learning, steering, review |
| [`@pr-workflow`](#pr-workflow) | Comprehensive PR lifecycle management | Every pull request |

---

## Quick Start

### Installation

Copy the skills to your Cursor rules directory:

```bash
# Clone the repository
git clone https://github.com/daniel-scrivner/cursor-skills.git

# For a single project
mkdir -p .cursor/rules/actions
cp cursor-skills/skills/*.mdc .cursor/rules/actions/

# For a workspace (shared across all projects)
mkdir -p /path/to/workspace/.cursor/rules/actions
cp cursor-skills/skills/*.mdc /path/to/workspace/.cursor/rules/actions/
```

### Usage

In Cursor chat, simply type `@` followed by the skill name:

```
@generate-prd I want to add dark mode to the app
```

---

## Skill Details

### @generate-prd

**Create detailed Product Requirements Documents through structured conversation.**

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  User: "I want to add keyboard shortcuts"                       │
│                           │                                     │
│                           ▼                                     │
│  ┌─────────────────────────────────────────────┐               │
│  │  AI asks 3-5 clarifying questions           │               │
│  │  with lettered options (A, B, C, D)         │               │
│  └─────────────────────────────────────────────┘               │
│                           │                                     │
│                           ▼                                     │
│  User: "1A, 2C, 3B"                                             │
│                           │                                     │
│                           ▼                                     │
│  ┌─────────────────────────────────────────────┐               │
│  │  AI generates complete PRD with:            │               │
│  │  • Goals & success metrics                  │               │
│  │  • User stories                             │               │
│  │  • Functional requirements (P0/P1/P2)       │               │
│  │  • Non-goals (explicit scope boundaries)    │               │
│  │  • Technical considerations                 │               │
│  └─────────────────────────────────────────────┘               │
│                           │                                     │
│                           ▼                                     │
│  Saved to: tasks/prd-keyboard-shortcuts.md                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Key Features:**
- Asks clarifying questions before writing (prevents rework)
- Uses numbered questions with lettered options for easy response
- Generates junior-developer-readable documentation
- Includes explicit scope boundaries (what's IN and OUT)

[View full skill →](skills/generate-prd.mdc)

---

### @generate-tasks

**Break requirements into structured, actionable task lists.**

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  Input: PRD or feature description                              │
│                           │                                     │
│                           ▼                                     │
│  ┌─────────────────────────────────────────────┐               │
│  │  Phase 1: Generate parent tasks             │               │
│  │                                             │               │
│  │  - [ ] 0.0 Create feature branch            │               │
│  │  - [ ] 1.0 Create base component            │               │
│  │  - [ ] 2.0 Implement logic                  │               │
│  │  - [ ] 3.0 Add styling                      │               │
│  │  - [ ] 4.0 Write tests                      │               │
│  │  - [ ] 5.0 Create PR and merge              │               │
│  └─────────────────────────────────────────────┘               │
│                           │                                     │
│              User confirms: "Go"                                │
│                           │                                     │
│                           ▼                                     │
│  ┌─────────────────────────────────────────────┐               │
│  │  Phase 2: Break down into sub-tasks         │               │
│  │                                             │               │
│  │  - [ ] 1.0 Create base component            │               │
│  │    - [ ] 1.1 Create file structure          │               │
│  │    - [ ] 1.2 Add TypeScript interfaces      │               │
│  │    - [ ] 1.3 Implement render method        │               │
│  │    - [ ] 1.4 Export from index              │               │
│  └─────────────────────────────────────────────┘               │
│                           │                                     │
│                           ▼                                     │
│  Saved to: tasks/tasks-keyboard-shortcuts.md                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Key Features:**
- Two-phase generation (parent tasks → sub-tasks)
- Always includes branch creation and PR tasks
- References @pr-workflow for merge process
- Written for junior developers (explicit, actionable)

[View full skill →](skills/generate-tasks.mdc)

---

### @pr-workflow

**Comprehensive PR lifecycle management — from creation to merge.**

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  PHASE 1: Before Creating PR                                    │
│  ───────────────────────────                                    │
│  ✓ Run typecheck (pnpm/make)                                   │
│  ✓ Run tests                                                    │
│  ✓ Security scan (no hardcoded paths/secrets)                  │
│  ✓ Find and use PR template                                     │
│                                                                 │
│  PHASE 2: Autonomous Monitoring                                 │
│  ──────────────────────────────                                 │
│  ┌─────────────────────────────────────────────┐               │
│  │  Poll every 30-60s:                         │               │
│  │  • Check mergeable_state (not just status!) │               │
│  │  • Read all bot comments                    │               │
│  │  • Address every piece of feedback          │               │
│  │  • Push fixes, reply to comments            │               │
│  │  • Wait for re-validation                   │               │
│  └─────────────────────────────────────────────┘               │
│                           │                                     │
│                           ▼                                     │
│  PHASE 3: Merge (only when mergeable_state = "clean")           │
│  ────────────────────────────────────────────────────           │
│  ✓ All checks passed                                            │
│  ✓ Zero unaddressed comments                                    │
│  ✓ All review threads resolved                                  │
│  → Execute merge                                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Key Features:**
- Uses GitHub MCP tools for full automation
- Understands the difference between `mergeable` and `mergeable_state`
- Addresses ALL bot feedback (never ignores comments)
- Polls until checks complete (no premature merges)

**Critical Insight:**
```
❌ mergeable: true      → Only means "no git conflicts"
✅ mergeable_state: "clean" → Means "all checks passed, safe to merge"
```

[View full skill →](skills/pr-workflow.mdc)

---

## Ralph Wiggum Technique

> "What if I told you that the way to get autonomous AI coding to work is with a for loop?"
> — Matt Pocock

The Ralph Wiggum technique is a deceptively simple approach to autonomous AI coding that outperforms complex agent orchestration systems.

### The Philosophy

Instead of complex agent swarms and mesh orchestrators:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  SIMPLE BEATS COMPLEX                                           │
│                                                                 │
│  ┌─────────────────────────────────────────────┐               │
│  │  while tasks_remain:                        │               │
│  │      task = pick_next_task()                │               │
│  │      implement(task)                        │               │
│  │      if tests_pass():                       │               │
│  │          commit()                           │               │
│  │          mark_done(task)                    │               │
│  │          log_learnings()                    │               │
│  └─────────────────────────────────────────────┘               │
│                                                                 │
│  That's it. That's the whole thing.                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Two Variants

```
┌────────────────────┐     ┌────────────────────┐
│                    │     │                    │
│   @ralph-hybrid    │     │   @ralph-manual    │
│                    │     │                    │
│   ┌──────────┐     │     │   ┌──────────┐     │
│   │ Bash     │     │     │   │ Cursor   │     │
│   │ Loop     │────▶│     │   │ Chat     │────▶│
│   │ + Claude │     │     │   │ (invoke  │     │
│   │ CLI      │     │     │   │ repeat)  │     │
│   └──────────┘     │     │   └──────────┘     │
│                    │     │                    │
│   Fully Autonomous │     │   Human-in-Loop   │
│   AFK / Overnight  │     │   Review & Steer  │
│                    │     │                    │
└────────────────────┘     └────────────────────┘
```

| Aspect | @ralph-hybrid | @ralph-manual |
|--------|---------------|---------------|
| **How it runs** | Bash loop + Claude CLI | Cursor chat invocations |
| **Autonomy** | Loops until all done | One task per invocation |
| **Human oversight** | Review at the end | Review after each task |
| **Best for** | AFK work, overnight runs | Learning, steering, complex tasks |
| **Dependencies** | Claude CLI required | Just Cursor |

### How It Works

```
                         ┌─────────────────────┐
                         │     prd.json        │
                         │  ┌───────────────┐  │
                         │  │ US-001 [ ]    │  │
                         │  │ US-002 [ ]    │  │
                         │  │ US-003 [ ]    │  │
                         │  └───────────────┘  │
                         └──────────┬──────────┘
                                    │
                                    ▼
┌──────────────┐           ┌─────────────────┐           ┌──────────────┐
│              │           │                 │           │              │
│  progress.   │◀─────────▶│   AI AGENT      │◀─────────▶│    Git       │
│  txt         │  learnings│                 │  commits  │    Repo      │
│              │           │  1. Read PRD    │           │              │
│  Patterns:   │           │  2. Pick task   │           │  feat: US-001│
│  - Use X     │           │  3. Implement   │           │  feat: US-002│
│  - Avoid Y   │           │  4. Test        │           │  feat: US-003│
│              │           │  5. Commit      │           │              │
└──────────────┘           │  6. Mark done   │           └──────────────┘
                           │  7. Loop        │
                           │                 │
                           └────────┬────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     prd.json        │
                         │  ┌───────────────┐  │
                         │  │ US-001 [✓]    │  │
                         │  │ US-002 [✓]    │  │
                         │  │ US-003 [✓]    │  │
                         │  └───────────────┘  │
                         └─────────────────────┘
```

### Memory Persistence

| Storage | Purpose |
|---------|---------|
| `prd.json` | Task status — what's done, what's next |
| `progress.txt` | Learnings — patterns, gotchas, conventions |
| Git commits | Source of truth — one commit per task |

### Task Sizing (Critical)

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  ❌ TOO BIG                    ✅ RIGHT SIZE                    │
│  ──────────                    ────────────                     │
│                                                                 │
│  "Build entire auth system"    "Add login form UI"              │
│  "Implement user dashboard"    "Add email validation"           │
│                                "Add auth API hook"              │
│                                "Add error handling"             │
│                                                                 │
│  Must fit in ONE context window!                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Two-Phase Completion

Both Ralph skills use thorough completion verification:

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  PHASE 1: Task Completion                                        │
│  ────────────────────────                                        │
│  ✓ All stories in prd.json have passes: true                    │
│                                                                  │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  PHASE 2: PR Readiness (via @pr-workflow)                        │
│  ─────────────────────                                           │
│  ✓ All inline comments addressed AND responded to               │
│  ✓ All CI tests passing (rerun flakes with gh CLI)              │
│  ✓ No merge conflicts                                            │
│  ✓ PR contains only relevant changes                             │
│  ✓ No shortcuts or faked results                                 │
│                                                                  │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  THEN: <promise>COMPLETE</promise>                               │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

### Quick Start (Ralph)

**Option A: Autonomous Mode (@ralph-hybrid)**

```bash
# 1. Invoke to scaffold files
@ralph-hybrid

# 2. Edit task list
vim scripts/ralph/prd.json

# 3. Run (requires Claude CLI)
./scripts/ralph/ralph.sh 25

# 4. Go to sleep 😴
```

**Option B: Interactive Mode (@ralph-manual)**

```bash
# 1. Create PRD and progress files
@ralph-manual

# 2. Invoke repeatedly
@ralph-manual   # Does US-001, commits, stops
@ralph-manual   # Does US-002, commits, stops
@ralph-manual   # Does US-003, commits, stops
```

[View @ralph-hybrid →](skills/ralph-hybrid.mdc) | [View @ralph-manual →](skills/ralph-manual.mdc)

---

## Complete Workflow Example

Here's how the skills chain together for a typical feature:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  1. DEFINE                                                      │
│  ─────────                                                      │
│  @generate-prd I want to add keyboard shortcuts to the app      │
│                           │                                     │
│                           ▼                                     │
│  Output: tasks/prd-keyboard-shortcuts.md                        │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  2. PLAN                                                        │
│  ──────                                                         │
│  @generate-tasks based on tasks/prd-keyboard-shortcuts.md       │
│                           │                                     │
│                           ▼                                     │
│  Output: tasks/tasks-keyboard-shortcuts.md                      │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  3. IMPLEMENT (choose one)                                      │
│  ─────────────────────────                                      │
│                                                                 │
│  Option A: Autonomous                                           │
│  @ralph-hybrid → ./scripts/ralph/ralph.sh 25                    │
│                                                                 │
│  Option B: Interactive                                          │
│  @ralph-manual (invoke repeatedly)                              │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  4. MERGE                                                       │
│  ───────                                                        │
│  @pr-workflow (or built into Ralph's completion)                │
│                           │                                     │
│                           ▼                                     │
│  ✅ Feature shipped!                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## File Structure

```
cursor-skills/
├── README.md
├── LICENSE
└── skills/
    ├── generate-prd.mdc      # PRD creation
    ├── generate-tasks.mdc    # Task breakdown
    ├── ralph-hybrid.mdc      # Autonomous coding
    ├── ralph-manual.mdc      # Interactive coding
    └── pr-workflow.mdc       # PR management
```

After installation in your project:

```
your-project/
├── .cursor/
│   └── rules/
│       └── actions/
│           ├── generate-prd.mdc
│           ├── generate-tasks.mdc
│           ├── ralph-hybrid.mdc
│           ├── ralph-manual.mdc
│           └── pr-workflow.mdc
└── tasks/                    # Generated by skills
    ├── prd-feature-name.md
    └── tasks-feature-name.md
```

---

## Best Practices

### 1. Start with PRDs
Don't jump straight to coding. A good PRD prevents wasted effort.

### 2. Break Tasks Small
Each task should fit in one context window. When in doubt, split it.

### 3. Let Learnings Compound
Ralph gets smarter with each task as patterns accumulate in `progress.txt`.

### 4. Trust the Completion Promise
Don't mark things done until they're actually done. The two-phase verification exists for a reason.

### 5. Review the Diffs
Even with autonomous coding, review what was generated before merging.

---

## Credits

- **[Geoffrey Huntley](https://www.ghuntley.com/ralph/)** — Creator of the Ralph Wiggum technique
- **[Matt Pocock](https://x.com/mattpocockuk)** — Popularized and refined the approach
- **[Anthropic](https://anthropic.com/)** — Claude, the AI that powers these workflows

---

## License

MIT — Use freely, attribution appreciated.

---

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  "The dev branch is always wackier than the main branch.       │
│   We are experimenting with stuff here — some of it works      │
│   and some doesn't work. But in a couple years' time, we       │
│   will coalesce around some kind of shared understanding       │
│   of how to use these tools effectively."                      │
│                                                                 │
│                                        — Matt Pocock            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```
