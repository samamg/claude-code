# Claude Code Quick Reference & Cheat Sheet

A rapid-lookup reference guide for commands, flags, keyboard shortcuts, settings, and templates in Claude Code.

---

## 1. CLI Commands & Startup Flags

| Command / Flag | Description | Example |
| :--- | :--- | :--- |
| `claude` | Start an interactive session in current directory | `claude` |
| `claude "<prompt>"` | Start a session with an initial prompt | `claude "fix the tests in auth.test.ts"` |
| `claude -p "<prompt>"` | **Headless Print Mode**: Run non-interactively and exit | `claude -p "summarize recent git commits"` |
| `claude --continue` | Re-attach to the most recent session | `claude --continue` |
| `claude --resume [id]` | Resume a specific session ID or open the session picker | `claude --resume` |
| `claude --fork-session` | Clone conversation history into a new branch/session ID | `claude --resume my-session --fork-session` |
| `claude --model <name>` | Select model (`sonnet`, `opus`, `haiku`) | `claude --model opus` |
| `claude --worktree <branch>` | Run in an isolated git worktree branch | `claude --worktree feat/refactor-db` |
| `claude --teleport` | Pull a cloud / web session directly into local terminal | `claude --teleport` |

---

## 2. Interactive Hotkeys & Controls

| Shortcut | Action | Description |
| :--- | :--- | :--- |
| `Enter` | **Send** | Submit current prompt to Claude. |
| `Shift + Enter` | **Newline** | Insert a multiline break without submitting. |
| `Esc` | **Interrupt** | Stop Claude's current action immediately. |
| `Esc` twice | **Undo Checkpoint** | Reverts file changes to the previous checkpoint snapshot. |
| `Shift + Tab` | **Toggle Mode** | Cycle through `Auto` ➔ `Manual` ➔ `Accept Edits` ➔ `Plan`. |
| Typing while busy + `Enter` | **Queue Message** | Queues your instruction; Claude reads it upon finishing the current tool step. |

---

## 3. Essential Slash Commands

| Command | Usage | Description |
| :--- | :--- | :--- |
| `/init` | `/init` | Analyzes project files and creates a starter `CLAUDE.md`. |
| `/doctor` | `/doctor` | Runs health check on PATH, dependencies, permissions, and network. |
| `/diff` | `/diff` | Opens live visual diff viewer for current unstaged modifications. |
| `/context` | `/context` or `/context all` | Inspects current token usage, loaded files, rules, and MCP tools. |
| `/compact` | `/compact [focus topic]` | Manually compacts context while retaining specified instructions. |
| `/clear` | `/clear` | Clears conversation history to start fresh in current session. |
| `/rewind` | `/rewind` | Interactively step back to a prior conversation turn or file state. |
| `/model` | `/model` | Switch the active reasoning model on the fly. |
| `/cost` | `/cost` | View token consumption and estimated costs for the session. |
| `/mcp` | `/mcp` | List connected MCP servers, tools, and connection health. |
| `/config` | `/config [key] [value]` | Inspect or update Claude Code settings interactively. |
| `/desktop` | `/desktop` | Transfer the current terminal session to the Claude Desktop App. |

---

## 4. Directory Structure & File Hierarchy

```text
my-project/
├── CLAUDE.md                 # Primary project instructions (always loaded)
└── .claude/                  # Project-level configuration
    ├── settings.json         # Permissions, hooks, and project settings
    ├── rules/                # Scoped rules (*.md) with path filters
    │   └── backend-rules.md
    ├── skills/               # Reusable project skills & slash commands
    │   └── deploy.md
    └── agents/               # Custom subagent definitions
        └── reviewer.md

~/.claude/                    # User-level global configuration
├── CLAUDE.md                 # Global user preferences (always loaded)
├── settings.json             # Global permissions, MCP servers, preferences
├── skills/                   # Globally available skills
└── projects/                 # Session history transcripts & auto-memory
```

---

## 5. Configuration & Template Snippets

### A. Minimal `CLAUDE.md` Template
```markdown
# Project Name & Architecture

## Quick Commands
- Dev: `npm run dev`
- Test: `npm test`
- Lint: `npm run lint`

## Conventions
- Use TypeScript strict mode.
- Prefer functional components and hooks for UI.
- All database operations must be wrapped in transactions where appropriate.
```

### B. `.claude/settings.json` (Permissions + Hooks + MCP)
```json
{
  "permissions": {
    "allow": [
      "Bash:npm test*",
      "Bash:npm run lint*",
      "ReadFile:*"
    ],
    "deny": [
      "Bash:rm -rf *",
      "WriteFile:.env*"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "EditFile|WriteFile",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\""
      }
    ]
  },
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost:5432/app_dev"]
    }
  }
}
```

### C. Custom Skill Template (`.claude/skills/review-pr.md`)
```markdown
---
name: review-pr
description: Perform a comprehensive code review on the current branch diff
disable-model-invocation: true
---

# Pull Request Review Workflow

1. Inspect git diff: `git diff origin/main...HEAD`
2. Check for:
   - Security vulnerabilities and input validation gaps.
   - Missing unit test coverage.
   - Code readability, naming consistency, and maintainability.
3. Output a structured Markdown review with severity badges:
   - 🔴 Critical
   - 🟡 Warning
   - 🟢 Suggestion
```
