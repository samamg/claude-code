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
├── CLAUDE.md                 # Primary project instructions (operational rules)
├── DESIGN.md                 # Visual design system contract & tokens
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
- Follow `@DESIGN.md` for all UI/UX styling.
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

### D. Starter `DESIGN.md` Template (Design System Contract)
```markdown
# Visual Design Contract (DESIGN.md)

## Aesthetic & Tokens
- **Vibe**: Modern Kinetic Minimalism
- **Colors**: Canvas `#FAF9F5` (Dark: `#141413`), Primary `#D97757`, Subtle `#E8E6DC`
- **Typography**: Headings: `Poppins`, Body: `Inter`, Mono: `JetBrains Mono`
- **Grid**: Strict 4pt/8pt scale (`4px`, `8px`, `16px`, `24px`, `32px`, `48px`)
- **Constraints**: No arbitrary pixel values; always support light/dark modes; ensure WCAG AA contrast.
```


---

## 6. Popular Plugins Quick Reference

### A. Superpowers (`superpowers`)
- **Install**: `claude plugin install superpowers@claude-plugins-official`
- **Key Commands**:
  - `/superpowers:brainstorm` - Socratic inquiry & requirement clarification.
  - `/superpowers:write-plan` - Granular 2–5 min atomic task planning.
  - `/superpowers:test-driven` - Strict Red-Green-Refactor TDD workflow.
  - `/superpowers:debug-systematic` - 4-phase root-cause debugging pipeline.
  - `/superpowers:review` - Automated code review & severity grading.
  - `/superpowers:help` - Show active skills and commands.
- **Guide**: [SUPERPOWERS_PLUGIN.md](./SUPERPOWERS_PLUGIN.md)

### B. Frontend Design (`frontend-design`)
- **Install**: `claude plugin install frontend-design@claude-plugins-official`
- **Capabilities**: Generates production-grade UI/UX code with distinctive typography, spatial depth, and bespoke aesthetic frameworks.
- **Guide**: [FRONTEND_DESIGN_PLUGIN.md](./FRONTEND_DESIGN_PLUGIN.md)

