# memory.md — MyAIAgentEngineer
<!-- Two-layer memory: this file = quick snapshot (@import). Cortex = deep recall. -->
<!-- Hard limit: 80 lines. Archive resolved items, never accumulate.               -->
<!-- Role: WHAT is true NOW. Mistake patterns → lessons.md.                        -->
<!-- Last updated: 2026-04-25                                                       -->

---

## Cortex Protocol (persistent cross-session memory)

### Session start
- Run `cortex_restore` to load recent context from the last session.
- Cross-reference with `git log --oneline -10` to see what changed.

### During the session
- After every significant task: `cortex_remember "<concise summary of what was done/decided>"`
- Every ~10 turns or when context feels heavy: `cortex_save` (auto-extracts the full transcript)

### Session end / before compaction
- Run `cortex_save` to persist the full session before context is lost.

### "reload" command
Run `cortex_recall "<current topic>"` + re-read this file + `git log --oneline -10`, then summarize.

---

## Current Focus
Initial setup complete. Next: define stack and add first agent implementation.

## Architecture & Key Decisions
- Stack: TBD — fill in when defined
- Scope: files inside this project directory only
- No DB
- Git: local commits only, no auto-push
- Model: claude-sonnet-4-6
- Plugins: Superpowers (skills framework) + Cortex (persistent memory)

## Active Work
| Task | Status | Notes |
|---|---|---|
| Project scaffold | [DONE] | CLAUDE.md, hooks, settings, memory, lessons configured |
| Define stack & first agent | [ ] | Up next |

## Environment Facts
- Windows 11, Git Bash, Python 3 available
- Hooks: safety-guard (PreToolUse Bash), auto-lint (PostToolUse Edit/Write), session-health (Stop)
- Linters: ruff / eslint / shellcheck — informational only, never blocking

## Critical Rules (always active)
- **Zero hallucination**: read the code before making any claim. Cite file + line number.
- **Verify processes**: prove it with PID + log timestamps, never assume.
- **No destructive git** without explicit user confirmation.

## Session Log
<!-- One line per session: YYYY-MM-DD — what was done -->
- 2026-04-25 — Project scaffold created; Superpowers + Cortex integrated into memory strategy
