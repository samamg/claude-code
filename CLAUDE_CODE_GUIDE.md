# Complete Learning Guide to Claude Code

> **Claude Code** is an agentic coding tool developed by Anthropic that reads your codebase, plans solutions, edits files, executes shell commands, runs tests, and integrates seamlessly with your development ecosystem across terminal, IDE, desktop, and web surfaces.

---

## Table of Contents
1. [Module 1: Introduction & Architecture](#module-1-introduction--architecture)
2. [Module 2: Installation, Setup & Authentication](#module-2-installation-setup--authentication)
3. [Module 3: Core Everyday Workflows](#module-3-core-everyday-workflows)
4. [Module 4: Terminal UI & Interactive Navigation](#module-4-terminal-ui--interactive-navigation)
5. [Module 5: Memory System & Context Management](#module-5-memory-system--context-management)
6. [Module 6: Extensibility Layer (Skills, Subagents, MCP, Hooks, Plugins)](#module-6-extensibility-layer)
7. [Module 7: Permissions, Sandboxing & Safety](#module-7-permissions-sandboxing--safety)
8. [Module 8: Multi-Agent Coordination & Parallel Work](#module-8-multi-agent-coordination--parallel-work)
9. [Module 9: Programmatic Automation with Claude Agent SDK](#module-9-programmatic-automation-with-claude-agent-sdk)
10. [Module 10: Best Practices, Cost Control & Troubleshooting](#module-10-best-practices-cost-control--troubleshooting)

---

## Module 1: Introduction & Architecture

### What is Claude Code?
Unlike traditional inline autocomplete tools that only suggest the next few lines of code within an open buffer, **Claude Code** operates as an **autonomous agentic coding partner**. It has project-wide awareness and direct access to your local developer environment.

```mermaid
flowchart LR
    subgraph Inline["Inline AI Completion"]
        Buffer["Active File Buffer"] --> LLM1["AI Suggestion"]
    end

    subgraph Agentic["Claude Code (Agentic)"]
        FS["Entire Codebase"] & Terminal["Terminal / Shell"] & Git["Git Repository"] & MCP["MCP / Tools"] --> Harness["Claude Agent Harness"]
        Harness <--> Reason["Reasoning Engine (Sonnet / Opus)"]
        Harness --> Actions["Multi-file Edits, Command Execution, Verification"]
    end
```

### The Agentic Loop
When assigned a task, Claude Code iteratively moves through three main phases until the objective is accomplished:

```mermaid
flowchart TD
    Start(["User Prompt"]) --> LoopStart
    
    subgraph AgenticLoop ["The Agentic Loop"]
        LoopStart["1. Gather Context<br>(Search files, read code, inspect errors, check git)"]
        --> Action["2. Take Action<br>(Draft changes, edit files, execute shell commands, run builds)"]
        --> Verify["3. Verify Results<br>(Run test suites, inspect linter output, check exit codes)"]
        --> Check{"Task Complete?"}
        Check -- No / Fix needed --> LoopStart
    end
    
    Check -- Yes --> Done(["Complete / User Notification"])
    
    User["User Interrupt / Feedback (Esc / Enter)"] -.->|Steer anytime| AgenticLoop
```

1. **Gather Context**: Claude searches files (`grep`, `glob`, file reading), inspects git history, and reads compiler/linter errors to build an accurate mental model.
2. **Take Action**: Claude writes multi-file edits, renames assets, creates new components, and runs commands.
3. **Verify Results**: Claude runs automated test suites, checks formatting, and evaluates runtime logs. If tests fail, it diagnoses the cause and self-corrects.

### Supported Surfaces
Claude Code can be accessed across multiple environments sharing the same underlying agentic engine:

| Surface | Best For | Features |
| :--- | :--- | :--- |
| **Terminal CLI** | Power users & command line workflows | Full Unix composability, pipeline support (`tail -n 50 app.log \| claude -p ...`), speed. |
| **VS Code / Cursor** | Editor-first development | Inline diff inspection, `@-mentions`, interactive plan review, sidebar panel. |
| **JetBrains IDEs** | IntelliJ, PyCharm, WebStorm | Interactive diff viewer, selection context sharing. |
| **Desktop App** | Standalone GUI development | Multi-session side-by-side management, visual diffs, iOS simulator integration, git branch isolation. |
| **Web (`claude.ai/code`)** | Cloud & remote coding | Zero local setup, offloaded compute, cloud routines, mobile companion support. |
| **Remote Control** | Hybrid flexibility | Control your local terminal session remotely from a browser or smartphone. |

---

## Module 2: Installation, Setup & Authentication

### 1. Installation

#### Native Installation (Recommended)
Native installs auto-update in the background to ensure you always have the latest capabilities and security patches.

- **Linux, macOS, WSL:**
  ```bash
  curl -fsSL https://claude.ai/install.sh | bash
  ```

- **Windows PowerShell:**
  ```powershell
  irm https://claude.ai/install.ps1 | iex
  ```

- **Windows CMD:**
  ```batch
  curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
  ```

#### Package Managers
- **macOS / Linux (Homebrew):**
  ```bash
  # Stable release channel
  brew install --cask claude-code
  
  # Latest release channel
  brew install --cask claude-code@latest
  ```
- **Windows (WinGet):**
  ```powershell
  winget install Anthropic.ClaudeCode
  ```
- **Linux Packages:** Available via `apt`, `dnf`, or `apk` for Debian, Ubuntu, Fedora, RHEL, and Alpine distributions.

Verify the installation:
```bash
claude --version
```

### 2. Authentication Options

Claude Code supports three primary authentication modes:

1. **Claude Subscription Login (Default & Recommended):**
   Run `claude` in any project folder. You will be prompted with an OAuth browser login associated with your Claude Pro, Max, Team, or Enterprise account.
2. **Anthropic Console API Key:**
   Set the environment variable in your shell profile (`~/.bashrc` or `~/.zshrc`):
   ```bash
   export ANTHROPIC_API_KEY="sk-ant-api..."
   ```
3. **Enterprise Cloud Providers:**
   For organizations with specific cloud compliance requirements, Claude Code can authenticate through:
   - **Amazon Bedrock**
   - **Google Cloud Agent Platform (formerly Vertex AI)**
   - **Microsoft Foundry**

---

## Module 3: Core Everyday Workflows

### 1. Exploring & Understanding a Codebase
When onboarding to a new project or debugging an unfamiliar repository:

```bash
cd my-project
claude
```
```text
> Explain how authentication and JWT token refresh are handled in this service.
```
*Claude will locate the auth controllers, middleware, and token rotation logic, presenting a clear summary with code references.*

### 2. Building Features & Implementing Requirements
Describe your desired functionality in plain language:

```text
> Add an endpoint POST /api/v1/export/csv that exports user activity logs. 
  Make sure it checks permissions with the requireAuth middleware and adds unit tests.
```

### 3. Debugging & Self-Healing Bug Fixes
Paste raw error traces, test failures, or issue descriptions:

```text
> The test suite is failing in tests/payments.test.ts: "Error: Stripe invalid payload signature".
  Investigate the webhook verification middleware, fix the root cause, and run the tests to verify.
```

### 4. Codebase-Wide Refactoring
```text
> Migrate all database queries in src/services/ from raw SQL to our Kysely query builder. 
  Run TypeScript type checks and linting after making changes.
```

### 5. Automated Git Operations
Claude Code natively understands Git status, branches, and diffs:

```text
> Stage the changes we just made, create a branch named feat/user-export, 
  and commit with a concise conventional-commit message.
```

---

## Module 4: Terminal UI & Interactive Navigation

### Interactive Keybindings & Controls

| Shortcut / Action | Function |
| :--- | :--- |
| `Enter` | Submit prompt / send command. |
| `Shift + Enter` | Insert newline without submitting. |
| `Esc` | Immediately stop Claude's current tool execution / step. |
| `Esc` twice | **Undo file modifications** (rewinds to previous file checkpoint). |
| `Shift + Tab` | Cycle between permission modes (`Auto` → `Manual` → `Accept Edits` → `Plan`). |
| Type while busy + `Enter` | **Queue messages**: Claude receives your instruction immediately after completing its current tool call. |

### Built-in Slash Commands Quick Reference

- `/init` — Analyzes your codebase and generates a customized starter `CLAUDE.md`.
- `/doctor` — Runs a diagnostic health check on your environment, PATH, network, and configs.
- `/diff` — Opens an interactive diff panel showing current uncommitted modifications.
- `/context` — Displays a breakdown of what files, rules, and tools currently occupy the context window.
- `/compact [focus]` — Summarizes prior conversation history while preserving key context.
- `/clear` — Clears conversation history to start a clean turn.
- `/rewind` — Rolls back to a previous conversation state or checkpoint.
- `/model` — Interactively switches between Claude models (Sonnet, Opus, Haiku).
- `/cost` — Displays the token consumption and estimated financial cost of the current session.
- `/desktop` — Hands off the current terminal session to the Claude Code Desktop GUI.

---

## Module 5: Memory System & Context Management

```mermaid
flowchart TD
    subgraph Storage ["Persistent Configuration & Memory"]
        Global["~/.claude/CLAUDE.md<br>(User-wide rules)"]
        Project["./CLAUDE.md<br>(Project-specific standards)"]
        Nested[".claude/rules/*.md<br>(Scoped path rules)"]
        AutoMem["Auto Memory<br>(~/.claude/projects/.../MEMORY.md)"]
    end

    subgraph RuntimeContext ["Claude Context Window"]
        System["System Harness + Tools"]
        PromptCache["Prompt Caching Layer"]
        SessionMsgs["Session History & File Reads"]
    end

    Global --> PromptCache
    Project --> PromptCache
    Nested --> PromptCache
    AutoMem --> PromptCache
    PromptCache --> RuntimeContext
```

### 1. `CLAUDE.md` Project Guidelines
`CLAUDE.md` is loaded at the beginning of every session in that project. Keep it concise (recommended under 200 lines) and focus on non-obvious rules:

```markdown
# Project Conventions: Billing Service

## Build & Test Commands
- Install: `pnpm install`
- Dev Server: `pnpm dev`
- Run Tests: `pnpm test`
- Single Test: `pnpm test -t "<test-name>"`
- Linter: `pnpm lint:fix`

## Architecture & Code Rules
- Always use `camelCase` for variables and `PascalCase` for React components.
- Never write raw SQL; use our repository layer in `src/repositories/`.
- All monetary amounts must be stored as integer cents, never floats.
```

> [!TIP]
> Use `@path/to/file` in `CLAUDE.md` to import modular reference files on demand rather than bloating the main file.

### 2. Auto Memory
Claude automatically learns preferences and patterns as you work together (such as package manager preferences or test command syntax) and records them in persistent auto-memory so you don't have to repeat yourself across sessions.

---

## Module 6: Extensibility Layer

Claude Code provides five distinct extension mechanisms:

```mermaid
classDiagram
    class CLAUDE_MD {
        Always-on Project Rules
        Loaded every session
    }
    class Skills {
        On-demand Workflows & Docs
        Triggered via /command or model
    }
    class Subagents {
        Isolated Worker Contexts
        Parallel tasks & specialized roles
    }
    class MCP {
        Model Context Protocol
        External tools & DB connections
    }
    class Hooks {
        Deterministic Event Triggers
        Lint on edit, block dangerous ops
    }
    class Plugins {
        Packaging & Distribution Layer
        Bundles Skills, Hooks, MCP & Agents
    }

    Plugins --> Skills
    Plugins --> Subagents
    Plugins --> MCP
    Plugins --> Hooks
```

### 1. Custom Skills (`.claude/skills/<name>.md`)
Skills define reusable workflows or reference documentation. They can be triggered by slash command (`/<name>`) or discovered automatically:

```markdown
---
name: deploy-staging
description: Run pre-flight checks, build Docker image, and deploy to staging Kubernetes cluster
disable-model-invocation: true
---

# Staging Deployment Workflow

When this skill is invoked:
1. Run `pnpm test` to ensure all tests pass.
2. Run `pnpm build` to verify production assets compile.
3. Execute `./scripts/deploy.sh staging`.
4. Monitor the deployment rollout: `kubectl rollout status deployment/web -n staging`.
5. Post deployment status summary to the user.
```

### 2. Custom Subagents (`.claude/agents/<name>.md`)
Subagents run in their own **isolated context windows**, shielding your primary conversation from token bloat when performing deep research or large audits:

```markdown
---
name: security-auditor
description: Scans modified files for security vulnerabilities, injection flaws, and exposed secrets.
tools:
  - ReadFile
  - Grep
  - Glob
---
You are a senior Application Security Engineer. Carefully inspect the codebase for OWASP Top 10 vulnerabilities, insecure deserialization, SQL injections, and hardcoded credentials. Provide actionable remediation snippets.
```

### 3. Model Context Protocol (MCP)
MCP connects Claude Code to external systems (databases, GitHub, Slack, Linear, Google Drive). Configure servers in `.claude/settings.json` or `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "database-readonly": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@localhost:5432/mydb"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    }
  }
}
```

### 4. Lifecycle Hooks (`.claude/hooks/`)
Hooks run deterministic scripts on lifecycle events without requiring model reasoning:

- **`PreToolUse`**: Enforce guardrails (e.g., prevent editing `.env` or running `rm -rf`).
- **`PostToolUse`**: Format code automatically after every file edit (`prettier` / `ruff`).
- **`SessionStart`**: Set up virtual environments or display custom welcome banners.
- **`Stop`**: Notify team channels or play alert sound on completion.

Example `settings.json` hook:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "EditFile|WriteFile",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\""
      }
    ]
  }
}
```

### 5. Plugins & Marketplaces (Featured: `frontend-design`)
Plugins package skills, subagents, MCP connections, and hooks into shareable, installable units via Anthropic marketplaces.

A premier example is the official **Frontend Design** plugin (`frontend-design@claude-plugins-official`):
- **Install**: `claude plugin install frontend-design@claude-plugins-official`
- **What it does**: Enforces production-grade, distinctive UI design, avoiding generic AI aesthetics (banning default system fonts and repetitive purple gradients in favor of intentional typography, neo-minimalism, and fluid animations).
- **In-depth guide**: See [FRONTEND_DESIGN_PLUGIN.md](./FRONTEND_DESIGN_PLUGIN.md) for full documentation and prompt templates.

---

## Module 7: Permissions, Sandboxing & Safety

### Permission Modes

```mermaid
stateDiagram-v2
    [*] --> AutoMode
    AutoMode --> ManualMode: Shift+Tab
    ManualMode --> AcceptEditsMode: Shift+Tab
    AcceptEditsMode --> PlanMode: Shift+Tab
    PlanMode --> AutoMode: Shift+Tab

    AutoMode: Auto Mode (AI Classifier reviews safety in background)
    ManualMode: Manual Mode (Prompts before each file edit or command)
    AcceptEditsMode: Accept Edits (Allows file edits, prompts on shell commands)
    PlanMode: Plan Mode (Read-only exploration & plan drafting)
```

### Reversible Checkpoints
Every time Claude modifies a file, it creates an internal file snapshot.
- Press **`Esc` twice** or type `/rewind` to revert code back to any previous state without affecting unrelated git history.

### Granular Declarative Rules (`.claude/settings.json`)
```json
{
  "permissions": {
    "allow": [
      "Bash:npm test*",
      "Bash:git status",
      "Bash:git diff*",
      "ReadFile:*"
    ],
    "deny": [
      "Bash:rm -rf /",
      "WriteFile:.env",
      "WriteFile:*.pem"
    ]
  }
}
```

---

## Module 8: Multi-Agent Coordination & Parallel Work

### 1. Git Worktree Isolation (`--worktree`)
When running multiple tasks simultaneously, avoid branch conflicts by running Claude in dedicated git worktrees:

```bash
# Launch a session in a separate worktree branch
claude --worktree feat/refactor-auth
```

### 2. Dynamic Workflows (`/workflows`)
For large-scale tasks (such as migrating 100+ endpoints or performing full-repo audits), dynamic workflows generate automated execution scripts that coordinate fleets of parallel subagents and combine findings into verified reports.

```text
> Run a dynamic workflow to audit all API controllers for missing input validation schemas.
```

---

## Module 9: Programmatic Automation with Claude Agent SDK

The **Claude Agent SDK** (available for TypeScript and Python) enables embedding Claude Code directly into your backend services, CI/CD pipelines, and autonomous automation scripts.

### TypeScript Example
```typescript
import { ClaudeAgent } from "@anthropic-ai/claude-agent-sdk";

async function runAutonomousReview() {
  const agent = new ClaudeAgent({
    projectDir: process.cwd(),
    permissionMode: "acceptEdits",
  });

  const result = await agent.run({
    prompt: "Scan recent git commits on this branch, write tests for untested functions, and report results.",
  });

  console.log("Agent finished with status:", result.status);
  console.log("Summary of changes:", result.summary);
}

runAutonomousReview();
```

### Unix CLI Piping & Headless Mode
```bash
# Analyze runtime log errors and propose fixes
tail -n 100 /var/log/app.error.log | claude -p "Diagnose why these errors occurred and suggest a fix."

# Batch translation or documentation generation
git diff main --name-only | claude -p "Review these files for potential security vulnerabilities."
```

---

## Module 10: Best Practices, Cost Control & Troubleshooting

### 1. The "Delegate, Don't Dictate" Mindset
- **Avoid micromanaging:** Don't dictate every single file path or shell syntax.
- **Provide context & goals:** Describe the symptom, relevant subsystems, and criteria for success. Let Claude explore and determine the execution steps.

### 2. Token & Cost Optimization
- Run `/compact` periodically during lengthy multi-hour sessions.
- Keep `CLAUDE.md` under 200 lines to minimize prompt overhead on every turn.
- Use subagents for noisy research tasks so large file contents don't pollute the main conversation context.
- Use `disable-model-invocation: true` on reference skills so they only load when explicitly invoked.

### 3. Troubleshooting & Diagnostics
- **Configuration issues:** Run `/doctor` to diagnose broken PATHs, missing dependencies, or malformed configs.
- **Context Inspection:** Run `/context` to see what files and tools are taking up space.
- **MCP Connection Failures:** Run `/mcp` to inspect status and reconnection state.
