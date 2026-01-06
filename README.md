```
 ____      _       _       _     
|  _ \ __ _| |_ __ | |__   | |    
| |_) / _` | | '_ \| '_ \  | |    
|  _ < (_| | | |_) | | | | |_|    
|_| \_\__,_|_| .__/|_| |_| (_)    
             |_|                  
```

# Cursor Skills

> **A collection of Cursor AI skills for autonomous coding workflows**

These skills implement the [Ralph Wiggum technique](https://www.ghuntley.com/ralph/) — a deceptively simple approach to AI-driven coding automation that outperforms complex agent orchestration systems.

---

## The Philosophy

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   "What if I told you that the way to get autonomous AI        │
│    coding to work is with a for loop?"                         │
│                                                                 │
│                                    — Matt Pocock                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

Instead of complex agent swarms and mesh orchestrators, Ralph uses:

- **A simple bash loop** that runs the AI repeatedly
- **A task list (PRD)** that tracks what needs to be done
- **A progress file** that accumulates learnings
- **Git commits** as the source of truth

The AI picks one task, implements it, tests it, commits it, and loops until done.

---

## Available Skills

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

| Skill | Mode | Best For | Dependencies |
|-------|------|----------|-------------|
| [`@ralph-hybrid`](skills/ralph-hybrid.mdc) | Autonomous | Overnight runs, AFK coding | Claude CLI |
| [`@ralph-manual`](skills/ralph-manual.mdc) | Interactive | Learning, steering, complex work | Just Cursor |

---

## How It Works

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
                                    │
                                    ▼
                           <promise>COMPLETE</promise>
```

### Memory Persists Through:

| Storage | Purpose |
|---------|--------|
| `prd.json` | Task status — what's done, what's next |
| `progress.txt` | Learnings — patterns, gotchas, conventions |
| Git commits | Source of truth — one commit per task |

---

## Quick Start

### Option A: Autonomous Mode (`@ralph-hybrid`)

```bash
# 1. In Cursor, invoke the skill to scaffold files
@ralph-hybrid

# 2. Edit the task list
vim scripts/ralph/prd.json

# 3. Run the loop (requires Claude CLI)
./scripts/ralph/ralph.sh 25   # Up to 25 iterations

# 4. Go to sleep 😴
```

### Option B: Interactive Mode (`@ralph-manual`)

```bash
# 1. Create your PRD and progress files (skill will guide you)
@ralph-manual

# 2. Invoke repeatedly — each call does ONE task
@ralph-manual   # Implements US-001, commits, stops
@ralph-manual   # Implements US-002, commits, stops
@ralph-manual   # Implements US-003, commits, stops
# ... review between each if desired
```

---

## File Structure

When you invoke either skill, it creates:

```
scripts/ralph/
├── ralph.sh        # Bash loop (hybrid mode only)
├── prompt.md       # Instructions for each iteration
├── prd.json        # Your task list
└── progress.txt    # Accumulated learnings
```

### prd.json — The Task List

```json
{
  "branchName": "ralph/auth-feature",
  "description": "Add user authentication",
  "userStories": [
    {
      "id": "US-001",
      "title": "Add login form",
      "acceptanceCriteria": [
        "Email and password fields exist",
        "Form validates email format",
        "typecheck passes",
        "tests pass"
      ],
      "priority": 1,
      "passes": false,
      "notes": ""
    }
  ]
}
```

### progress.txt — The Learning Log

```markdown
# Ralph Progress Log

Started: 2024-01-15
Branch: ralph/auth-feature

## Codebase Patterns
- Migrations: Always use IF NOT EXISTS  
- API hooks: Use makeApiMutation from @web-monorepo/fetchers
- Tests: Run `make test` not `pnpm test`

---

## 2024-01-15 - US-001: Add login form
- **Implemented**: Login form with email/password fields
- **Files changed**: src/components/LoginForm.tsx, src/components/LoginForm.test.tsx
- **Learnings**:
  - Form validation uses zod schema
  - Submit handler is in useLoginVM hook
```

---

## Critical Success Factors

### 1. Size Tasks Correctly

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  ❌ TOO BIG                    ✅ RIGHT SIZE                │
│  ──────────                    ────────────                 │
│                                                             │
│  "Build entire auth system"    "Add login form UI"          │
│  "Implement user dashboard"    "Add email validation"       │
│                                "Add auth API hook"          │
│                                "Add error handling"         │
│                                                             │
│  Must fit in ONE context window!                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 2. Write Explicit Acceptance Criteria

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  ❌ VAGUE                      ✅ EXPLICIT                  │
│  ─────────                     ──────────                   │
│                                                             │
│  "Users can log in"            "Email field validates       │
│  "Form works correctly"         format on blur"             │
│                                "Error message appears        │
│                                 below field on invalid"     │
│                                "Submit disabled until        │
│                                 form is valid"              │
│                                "typecheck passes"            │
│                                "tests pass"                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3. Robust Feedback Loops

Ralph needs fast, reliable feedback:

```bash
# These MUST work reliably
make typecheck   # or: pnpm typecheck
make test        # or: pnpm test
```

Without working tests and typechecks, broken code compounds.

### 4. Learnings Compound

By story 10, Ralph knows all the patterns from stories 1-9.

```
 Story 1:  "Hmm, this codebase uses zod for validation..."
 Story 5:  "I know to use makeApiMutation for API calls"
 Story 10: "I understand the full auth flow and all patterns"
```

---

## Two-Phase Completion

These skills use a thorough **two-phase completion check**:

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  PHASE 1: Task Completion                                    │
│  ────────────────────────                                    │
│  ✓ All stories in prd.json have passes: true                │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  PHASE 2: PR Readiness                                       │
│  ─────────────────────                                       │
│  ✓ All inline comments addressed AND responded to           │
│  ✓ All CI tests passing (rerun flakes with gh CLI)          │
│  ✓ No merge conflicts                                        │
│  ✓ PR contains only relevant changes                         │
│  ✓ No shortcuts or faked results                             │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  THEN: <promise>COMPLETE</promise>                           │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## Installation

### For Cursor IDE

Copy the skill files to your Cursor rules directory:

```bash
# For a single project
mkdir -p .cursor/rules/actions
cp skills/*.mdc .cursor/rules/actions/

# For a workspace (shared across projects)
mkdir -p /path/to/workspace/.cursor/rules/actions
cp skills/*.mdc /path/to/workspace/.cursor/rules/actions/
```

### Claude CLI (for @ralph-hybrid)

Install the Claude CLI for autonomous mode:

```bash
# Install Claude CLI
npm install -g @anthropic-ai/claude-code

# Or via Homebrew
brew install anthropic/tap/claude
```

---

## Monitoring Progress

```bash
# Check task status
cat scripts/ralph/prd.json | jq '.userStories[] | {id, title, passes}'

# Count completed vs remaining
cat scripts/ralph/prd.json | jq '[.userStories[] | select(.passes == true)] | length'
cat scripts/ralph/prd.json | jq '[.userStories[] | select(.passes == false)] | length'

# View accumulated learnings
cat scripts/ralph/progress.txt

# Check recent commits
git log --oneline -10
```

---

## When NOT to Use Ralph

- **Exploratory work** — when you don't know what you're building yet
- **Major refactors** — without clear, testable criteria
- **Security-critical code** — anything needing careful human review
- **Ambiguous requirements** — when acceptance criteria can't be explicit

---

## Credits

- **[Geoffrey Huntley](https://www.ghuntley.com/ralph/)** — Creator of the Ralph Wiggum technique
- **[Matt Pocock](https://x.com/mattpocockuk)** — Popularized and refined the approach
- **[Anthropic](https://anthropic.com/)** — Claude, the AI that powers this

---

## License

MIT

---

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  "The dev branch is always wackier than the main branch.   │
│   We are experimenting with stuff here — some of it works  │
│   and some doesn't work. But in a couple years' time, we   │
│   will coalesce around some kind of shared understanding   │
│   of how to use these tools effectively."                  │
│                                                             │
│                                        — Matt Pocock        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```
