# CLAUDE.md — MyAIAgentEngineer

> Context: AI agent capable of performing complex tasks, driven by Claude Code as pair-programmer and optimization agent.
> Goal: controlled command execution, parallel reasoning, strong verification — no hand-holding.

---

## 0) Language (mandatory)

Always communicate in English.
All responses, explanations, commit messages, and code comments must be in English.

---

## 1) Operating Mode

### 1.1 Plan Mode
Enter plan mode **only** for high-risk tasks: architectural decisions, breaking interface changes.
- If something goes sideways: STOP, reassess, re-plan. Never brute-force.
- For everything else: just do the work.

### 1.2 Subagent Strategy
Use subagents liberally:
- Offload research, exploration, parallel analysis, log/trace inspection.
- One focused task per subagent.
- Complex problem? Throw more compute at it via subagents.

### 1.3 Verification Before "Done"
Never mark a task complete without proof:
- Compare behavior before vs after when relevant.
- Run tests, reproduce the bug, check logs, demonstrate correctness.
- Ask internally: "Would a staff engineer approve this?"

### 1.4 Autonomous Bug Fixing
When given a bug report: fix it without hand-holding.
Use logs, failing tests, traces, minimal reproduction.
If tests exist and fail: fix them without being asked.

### 1.5 Self-Improvement Loop
After any user correction or recurring mistake:
- Add a dated entry to `lessons.md`: mistake pattern → prevention rule → checklist item.
- Review `lessons.md` at session start and before any high-risk task.

### 1.6 Parallel Execution
- Launch independent subagents and tool calls in parallel whenever possible.
- Never sequentialize work that has no dependencies.
- Example: reading 3 files + searching 2 patterns → one message with all calls.

### 1.7 Memory Persistence

Two complementary layers — use both:

| Layer | Tool | Role |
|---|---|---|
| **Snapshot** | `memory.md` (@import) | Quick-read project state loaded at every session start |
| **Deep recall** | Cortex plugin | Vector-search memory bank persisted across sessions |
| **Lessons** | `lessons.md` (@import) | Mistake patterns and prevention rules |

**Cortex commands (hjertefolger/cortex plugin):**
- `cortex_restore` — load recent context at session start
- `cortex_remember "<summary>"` — save one fact or decision immediately
- `cortex_save` — parse full transcript, auto-extract and store everything valuable
- `cortex_recall "<topic>"` — retrieve relevant memories before starting work

**Session protocol:**
- Start: `cortex_restore` → read `@memory.md` + `@lessons.md` → `git log --oneline -10`
- During: `cortex_remember` after each significant task; `cortex_save` every ~10 turns
- End / before compaction: `cortex_save` to persist the session
- "reload": `cortex_recall "<topic>"` + re-read both files + git log → summarize

**`memory.md` rules:**
- Hard limit: **80 lines**. Delete or archive resolved items — never accumulate.
- Update "Current Focus" and Active Work table after each significant task.
- Outdated memory is worse than no memory — keep it accurate.

### 1.8 Web Research
- Execute web searches directly with WebSearch — never delegate to subagents.
- Launch multiple WebSearch calls in parallel for different queries.

### 1.9 Superpowers Plugin
The Superpowers plugin (obra/superpowers) is active — it provides structured skills
(TDD, debugging, brainstorming, subagent workflows) that auto-invoke via the Skill tool.

Priority rules:
- **This CLAUDE.md always overrides any Superpowers skill** when they conflict.
- Invoke relevant skills before acting — even a 1 % chance of applicability warrants a check.
- Do not use the Read tool on skill files; use the Skill tool to invoke them.

---

## 2) Repo Scope & Safety

### In-scope
Everything inside the current project directory.

### Out-of-scope (never touch without explicit confirmation)
Any file or folder outside the current project directory.

### Safety rules
- Never leak secrets (API keys, passwords, tokens) in logs, commits, or outputs.
- Never state a codebase fact without having READ the code. Back claims with line numbers.
- Never run `git reset --hard`, force push, `rm -rf`, or `git checkout -- .` without explicit user confirmation.

---

## 3) Git Rules

After any code change:
1. `git add <specific files>` — never `-A`
2. `git commit -m "concise description of what and why"`

No automatic push — push is always manual and user-initiated.

Pull: `git pull --rebase origin main`
Performance work: branch `perf/<short-topic>`

---

## 4) Running the Project

```bash
# Fill in when defined — e.g.: python main.py | pytest | npm run dev
```

---

## 5) Code Conventions

- Keep interfaces stable: CLI args, function signatures used by other code.
- Default to safe changes: idempotence, timeouts, retries, clear logs.
- For any optimization: provide (a) expected impact, (b) risk, (c) how to verify.
- Do not comment code you did not change. Do not over-engineer.

---

## 6) Response After Changes

For non-trivial changes, always include:
- What changed and why.
- Verification steps run.
- Rollback path (e.g., `git restore <file>`).

---

## 7) Documentation

After any significant architectural modification, update `README.md` and relevant
docs to reflect current design, components, and structure.

---

## 8) Imports (expanded at session start)

@lessons.md
@memory.md
@CLAUDE.local.md
