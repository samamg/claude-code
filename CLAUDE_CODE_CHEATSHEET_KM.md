# តារាងសង្ខេបពាក្យបញ្ជា និងគន្លឹះរហ័ស Claude Code (ភាសាខ្មែរ)

ឯកសារយោងរហ័សសម្រាប់ស្វែងរកពាក្យបញ្ជា គ្រាប់ចុចកាត់ ការកំណត់រចនាសម្ព័ន្ធ និងគំរូ Template ក្នុង Claude Code។

---

## ១. ពាក្យបញ្ជា CLI និង Startup Flags

| ពាក្យបញ្ជា / Flag | ការពិពណ៌នា | ឧទាហរណ៍ |
| :--- | :--- | :--- |
| `claude` | ចាប់ផ្តើម Session ថ្មីក្នុង Folder បច្ចុប្បន្ន | `claude` |
| `claude "<prompt>"` | ចាប់ផ្តើម Session ជាមួយការបញ្ជាភ្លាមៗ | `claude "ជួសជុល test ក្នុង auth.test.ts"` |
| `claude -p "<prompt>"` | **Headless Print Mode**៖ ដំណើរការដោយស្វ័យប្រវត្តិតាម Terminal | `claude -p "សង្ខេប commit ថ្មីៗក្នុង git"` |
| `claude --continue` | បន្តការសន្ទនាពី Session ចុងក្រោយបង្អស់ | `claude --continue` |
| `claude --resume [id]` | បើកបញ្ជីជ្រើសរើស Session ឬបន្ត Session តាម ID | `claude --resume` |
| `claude --fork-session` | ចម្លងប្រវត្តិសន្ទនាពី Session មុនទៅកាន់ Session ID ថ្មី | `claude --resume my-session --fork-session` |
| `claude --model <name>` | ជ្រើសរើសម៉ូដែល AI (`sonnet`, `opus`, `haiku`) | `claude --model opus` |
| `claude --worktree <branch>` | បំបែកការងារទៅកាន់ Git Worktree ដាច់ដោយឡែក | `claude --worktree feat/refactor-db` |
| `claude --teleport` | ទាញយក Session ពី Web / Cloud មកកាន់ Terminal ក្នុងម៉ាស៊ីនផ្ទាល់ | `claude --teleport` |

---

## ២. គ្រាប់ចុចកាត់ (Keyboard Shortcuts)

| គ្រាប់ចុចកាត់ | សកម្មភាព | ការពិពណ៌នា |
| :--- | :--- | :--- |
| `Enter` | **ផ្ញើសារ** | បញ្ជូនការបញ្ជាទៅកាន់ Claude។ |
| `Shift + Enter` | **ចុះបន្ទាត់** | បញ្ចូលបន្ទាត់ថ្មីដោយមិនទាន់ផ្ញើ (Multiline)។ |
| `Esc` | **ផ្អាកភ្លាមៗ** | បញ្ឈប់សកម្មភាពបច្ចុប្បន្នរបស់ Claude។ |
| `Esc` ២ដង | **Undo Checkpoint** | ត្រឡប់ការកែប្រែ File ទាំងអស់ទៅកាន់ស្ថានភាពមុន។ |
| `Shift + Tab` | **ប្តូរ Mode** | ប្តូររវាង `Auto` ➔ `Manual` ➔ `Accept Edits` ➔ `Plan`។ |
| វាយអក្សរពេល Claude កំពុងដំណើរការ + `Enter` | **តម្រង់ជួរ (Queue)** | តម្រង់ជួរសារ ដើម្បីឱ្យ Claude អានភ្លាមៗក្រោយចប់ជំហានបច្ចុប្បន្ន។ |

---

## ៣. ពាក្យបញ្ជា Slash Commands សំខាន់ៗ

| ពាក្យបញ្ជា | របៀបប្រើ | ការពិពណ៌នា |
| :--- | :--- | :--- |
| `/init` | `/init` | ស្កេនគម្រោង និងបង្កើតឯកសារ `CLAUDE.md` ដោយស្វ័យប្រវត្តិ។ |
| `/doctor` | `/doctor` | ពិនិត្យសុខភាពបរិស្ថាន (PATH, Network, Configs) និងជួសជុល។ |
| `/diff` | `/diff` | បើកផ្ទាំងមើលការកែកូដជាក់ស្តែង (Visual Diff)។ |
| `/context` | `/context` ឬ `/context all` | ពិនិត្យទំហំ Token និងឯកសារដែលកំពុងផ្ទុកក្នុង Context Window។ |
| `/compact` | `/compact [ប្រធានបទ]` | បង្រួមប្រវត្តិសន្ទនា ដើម្បីសន្សំទំហំ Context។ |
| `/clear` | `/clear` | សម្អាតប្រវត្តិសន្ទនាដើម្បីចាប់ផ្តើមជុំថ្មីស្អាត។ |
| `/rewind` | `/rewind` | ជ្រើសរើសត្រឡប់ទៅចំណុចសន្ទនា ឬស្ថានភាពកូដមុន។ |
| `/model` | `/model` | ជ្រើសរើសប្តូរម៉ូដែល AI ភ្លាមៗ។ |
| `/cost` | `/cost` | មើលចំនួន Token និងតម្លៃចំណាយនៃ Session បច្ចុប្បន្ន។ |
| `/mcp` | `/mcp` | ពិនិត្យស្ថានភាពតភ្ជាប់នៃ MCP Servers ទាំងអស់។ |
| `/config` | `/config [key] [value]` | ពិនិត្យ ឬកែសម្រួល Settings ផ្ទាល់ក្នុង Terminal។ |
| `/desktop` | `/desktop` | ផ្ទេរការសន្ទនាបច្ចុប្បន្នទៅកាន់ Claude Desktop App។ |

---

## ៤. រចនាសម្ព័ន្ធ Folder និងឯកសារ

```text
my-project/
├── CLAUDE.md                 # ច្បាប់ណែនាំប្រតិបត្តិការគម្រោង (ផ្ទុកឡើងរាល់ Session)
├── DESIGN.md                 # កិច្ចសន្យាប្រព័ន្ធរចនា UI & Design Tokens
└── .claude/                  # ការកំណត់រចនាសម្ព័ន្ធកម្រិតគម្រោង
    ├── settings.json         # កំណត់សិទ្ធិ, Hooks, និង Settings គម្រោង
    ├── rules/                # ច្បាប់បំបែកតាម Folder (*.md)
    │   └── backend-rules.md
    ├── skills/               # Skills & ពាក្យបញ្ជា Slash Commands ប្រចាំគម្រោង
    │   └── deploy.md
    └── agents/               # និយមន័យ Subagents ផ្ទាល់ខ្លួន
        └── reviewer.md

~/.claude/                    # ការកំណត់រចនាសម្ព័ន្ធកម្រិត User ទាំងមូល
├── CLAUDE.md                 # ច្បាប់សកលទូទាំងកុំព្យូទ័រ
├── settings.json             # កំណត់សិទ្ធិ និង MCP Servers សកល
├── skills/                   # Skills សកល
└── projects/                 # ប្រវត្តិសន្ទនា និង Auto-Memory
```

---

## ៥. គំរូ Template ងាយៗសម្រាប់ចម្លងយកទៅប្រើ

### ក. គំរូឯកសារ `CLAUDE.md`
```markdown
# ឈ្មោះគម្រោង និងស្ថាបត្យកម្ម

## ពាក្យបញ្ជា Build & Test
- Dev: `npm run dev`
- Test: `npm test`
- Lint: `npm run lint`

## ស្តង់ដារសរសេរកូដ
- ប្រើប្រាស់ TypeScript Strict Mode។
- ប្រើ Functional Components សម្រាប់ React UI។
- អនុវត្តតាម `@DESIGN.md` សម្រាប់រាល់ការរចនា UI/UX។
```

### ខ. គំរូ `.claude/settings.json` (សិទ្ធិ + Hooks + MCP)
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

### គ. គំរូ Skill ផ្ទាល់ខ្លួន (`.claude/skills/review-pr.md`)
```markdown
---
name: review-pr
description: ពិនិត្យកូដ (Code Review) លើ Branch បច្ចុប្បន្នយ៉ាងល្អិតល្អន់
disable-model-invocation: true
---

# ជំហានត្រួតពិនិត្យ Pull Request

1. ពិនិត្យ Git Diff: `git diff origin/main...HEAD`
2. ពិនិត្យមើលចំណុចសំខាន់ៗ៖
   - សុវត្ថិភាពកូដ និងការការពារ Input Validation។
   - តើមាន Unit Test គ្រប់គ្រាន់ដែរឬទេ?
   - ភាពងាយអាន និងស្តង់ដារ Clean Code។
3. បង្កើតរបាយការណ៍ជា Markdown ជាមួយ Badge៖
   - 🔴 កំហុសធ្ងន់ធ្ងរ (Critical)
   - 🟡 ការព្រមាន (Warning)
   - 🟢 យោបល់កែលម្អ (Suggestion)
```

### ឃ. គំរូកិច្ចសន្យារចនា `DESIGN.md` (Design System Contract)
```markdown
# កិច្ចសន្យាប្រព័ន្ធរចនា (DESIGN.md)

## រចនាបថ និង Tokens
- **ទម្រង់**: Modern Kinetic Minimalism
- **ក្ដារពណ៌**: Canvas `#FAF9F5` (Dark: `#141413`), Primary `#D97757`, Subtle `#E8E6DC`
- **ពុម្ពអក្សរ**: Headings: `Poppins`, Body: `Inter`, Mono: `JetBrains Mono`
- **ក្រឡាចត្រង្គ**: អនុវត្តតាមខ្នាត 4pt/8pt យ៉ាងតឹងរ៉ឹង (`4px`, `8px`, `16px`, `24px`, `32px`, `48px`)
- **បម្រាម**: ហាមដាច់ខាតមិនឱ្យប្រើ Pixel តាមចិត្ត; ត្រូវគាំទ្រ Light/Dark Modes និង WCAG AA Contrast។
```

---

## ៦. តារាងពាក្យបញ្ជារហ័សសម្រាប់ Plugins សំខាន់ៗ

### ក. Superpowers (`superpowers`)
- **ដំឡើង**: `claude plugin install superpowers@claude-plugins-official`
- **ពាក្យបញ្ជាស្នូល**:
  - `/superpowers:brainstorm` - សួរបំភ្លឺតម្រូវការ និងស្ថាបត្យកម្មប្រព័ន្ធបែប Socratic។
  - `/superpowers:write-plan` - បង្កើតផែនការកិច្ចការតូចៗកម្រិតអាតូមិក (២-៥ នាទី)។
  - `/superpowers:test-driven` - អនុវត្តវដ្ត TDD (Red-Green-Refactor) យ៉ាងតឹងរ៉ឹង។
  - `/superpowers:debug-systematic` - ដោះស្រាយ Bug តាមប្រព័ន្ធ ៤ ដំណាក់កាល។
  - `/superpowers:review` - ត្រួតពិនិត្យកូដ និងវាយតម្លៃកម្រិតធ្ងន់ធ្ងរនៃបញ្ហា។
  - `/superpowers:help` - បង្ហាញជំនាញ និងពាក្យបញ្ជាទាំងអស់។
- **ឯកសារពេញលេញ**: [SUPERPOWERS_PLUGIN_KM.md](./SUPERPOWERS_PLUGIN_KM.md)

### ខ. Frontend Design (`frontend-design`)
- **ដំឡើង**: `claude plugin install frontend-design@claude-plugins-official`
- **សមត្ថភាព**: បង្កើតកូដ UI/UX លំដាប់ Production ស្រស់ស្អាត ប្លែកភ្នែក មិនដដែលៗ ព្រមទាំងគាំទ្រ Tailwind CSS, React, Vue, Svelte។
- **ឯកសារពេញលេញ**: [FRONTEND_DESIGN_PLUGIN_KM.md](./FRONTEND_DESIGN_PLUGIN_KM.md)

