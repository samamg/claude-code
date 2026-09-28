# សៀវភៅណែនាំ និងមេរៀនពេញលេញអំពី Claude Code (ភាសាខ្មែរ)

> **Claude Code** គឺជាឧបករណ៍ជំនួយសរសេរកូដឆ្លាតវៃកម្រិតភ្នាក់ងារ (Agentic Coding Tool) ដែលបង្កើតឡើងដោយ Anthropic។ វាមានសមត្ថភាពស្វែងយល់ពីគម្រោងកូដទាំងមូល (Codebase) របស់អ្នក រៀបចំផែនការ សរសេរ និងកែសម្រួលឯកសារកូដ ដំណើរការ Shell Commands ធ្វើតេស្តស្វ័យប្រវត្តិ និងភ្ជាប់ទំនាក់ទំនងយ៉ាងរលូនជាមួយឧបករណ៍អភិវឌ្ឍន៍លើ Terminal, IDE (VS Code, JetBrains), Desktop App និង Web Browser។

---

## មាតិកា (Table of Contents)
1. [មេរៀនទី ១៖ សេចក្តីផ្តើម និងស្ថាបត្យកម្ម (Introduction & Architecture)](#មេរៀនទី-១-សេចក្តីផ្តើម-និងស្ថាបត្យកម្ម)
2. [មេរៀនទី ២៖ ការដំឡើង និងការកំណត់ផ្ទៀងផ្ទាត់ (Installation & Setup)](#មេរៀនទី-២-ការដំឡើង-និងការកំណត់ផ្ទៀងផ្ទាត់)
3. [មេរៀនទី ៣៖ ដំណើរការការងារប្រចាំថ្ងៃ (Core Everyday Workflows)](#មេរៀនទី-៣-ដំណើរការការងារប្រចាំថ្ងៃ)
4. [មេរៀនទី ៤៖ ផ្ទាំងបញ្ជា Terminal UI និងការបញ្ជាផ្លូវកាត់ (Terminal UI & Navigation)](#មេរៀនទី-៤-ផ្ទាំងបញ្ជា-terminal-ui-និងការបញ្ជាផ្លូវកាត់)
5. [មេរៀនទី ៥៖ ប្រព័ន្ធចងចាំ និងការគ្រប់គ្រងបរិបទ (Memory & Context Management)](#មេរៀនទី-៥-ប្រព័ន្ធចងចាំ-និងការគ្រប់គ្រងបរិបទ)
6. [មេរៀនទី ៦៖ ស្រទាប់បន្ថែមសមត្ថភាព (Skills, Subagents, MCP, Hooks, Plugins)](#មេរៀនទី-៦-ស្រទាប់បន្ថែមសមត្ថភាព)
7. [មេរៀនទី ៧៖ សិទ្ធិ សុវត្ថិភាព និង Sandboxing (Permissions & Safety)](#មេរៀនទី-៧-សិទ្ធិ-សុវត្ថិភាព-និង-sandboxing)
8. [មេរៀនទី ៨៖ ការសម្របសម្រួលភ្នាក់ងារច្រើន និងការងារស្របគ្នា (Multi-Agent & Parallel Work)](#មេរៀនទី-៨-ការសម្របសម្រួលភ្នាក់ងារច្រើន-និងការងារស្របគ្នា)
9. [មេរៀនទី ៩៖ ការប្រើប្រាស់តាមរយៈកូដកម្មវិធី (Claude Agent SDK)](#មេរៀនទី-៩-ការប្រើប្រាស់តាមរយៈកូដកម្មវិធី-claude-agent-sdk)
10. [មេរៀនទី ១០៖ គន្លឹះអនុវត្តល្អៗ និងការដោះស្រាយបញ្ហា (Best Practices & Troubleshooting)](#មេរៀនទី-១០-គន្លឹះអនុវត្តល្អៗ-និងការដោះស្រាយបញ្ហា)

---

## មេរៀនទី ១៖ សេចក្តីផ្តើម និងស្ថាបត្យកម្ម

### តើ Claude Code ជាអ្វី?
ខុសប្លែកពីឧបករណ៍បង្កើតកូដជំនាន់មុន (Inline Autocomplete) ដែលគ្រាន់តែជួយបំពេញកូដពីរបីបន្ទាត់នៅក្នុង File កំពុងបើក **Claude Code** ដំណើរការក្នុងទម្រង់ជា **ដៃគូសរសេរកូដស្វ័យប្រវត្ត (Agentic Coding Partner)**។ វាមានសមត្ថភាពមើលឃើញគម្រោងកូដទាំងមូល និងមានសិទ្ធិចូលបញ្ជា Terminal ក្នុងម៉ាស៊ីនរបស់អ្នកផ្ទាល់។

```mermaid
flowchart LR
    subgraph Inline["Inline AI Completion (ជំនាន់មុន)"]
        Buffer["File កំពុងបើក"] --> LLM1["សំណើ AI បំពេញកូដ"]
    end

    subgraph Agentic["Claude Code (Agentic)"]
        FS["គម្រោងកូដទាំងមូល"] & Terminal["Terminal / Shell"] & Git["Git Repository"] & MCP["MCP / Tools ក្រៅ"] --> Harness["Claude Agent Harness"]
        Harness <--> Reason["ម៉ូដែលគិត (Sonnet / Opus)"]
        Harness --> Actions["កែកូដច្រើន Files, រត់ Command, ធ្វើ Test"]
    end
```

### វដ្តភ្នាក់ងារ (The Agentic Loop)
នៅពេលអ្នកដាក់កិច្ចការមួយឱ្យ Claude Code វានឹងដំណើរការជា ៣ ដំណាក់កាលបន្តបន្ទាប់គ្នា រហូតដល់កិច្ចការត្រូវបានបញ្ចប់ទាំងស្រុង៖

```mermaid
flowchart TD
    Start(["អ្នកប្រើប្រាស់បញ្ចូល Prompt"]) --> LoopStart
    
    subgraph AgenticLoop ["វដ្តភ្នាក់ងារ (The Agentic Loop)"]
        LoopStart["១. ប្រមូលបរិបទ (Gather Context)<br>(ស្វែងរក Files, អានកូដ, មើល error, ពិនិត្យ git)"]
        --> Action["២. ចាត់វិធានការ (Take Action)<br>(រៀបចំដំណោះស្រាយ, កែសម្រួល Files, រត់ commands)"]
        --> Verify["៣. ផ្ទៀងផ្ទាត់លទ្ធផល (Verify Results)<br>(ដំណើរការ Test, ពិនិត្យ Linter, មើល log)"]
        --> Check{"តើកិច្ចការរួចរាល់?"}
        Check -- នៅមានកំហុស/មិនទាន់ចប់ --> LoopStart
    end
    
    Check -- រួចរាល់ --> Done(["ចប់សព្វគ្រប់ / ជូនដំណឹងដល់អ្នកប្រើ"])
    
    User["អ្នកប្រើអាចកែតម្រូវបានគ្រប់ពេល (Esc / Enter)"] -.->|ផ្តល់ព័ត៌មានបន្ថែម| AgenticLoop
```

1. **ប្រមូលបរិបទ (Gather Context)**៖ Claude ស្វែងរក Files ក្នុងគម្រោង (`grep`, `glob`, អានកូដ) ពិនិត្យ Git History និងអានសារ Error ដើម្បីយល់ពីទម្រង់កូដ។
2. **ចាត់វិធានការ (Take Action)**៖ Claude សរសេរកូដ បង្កើត File ថ្មី កែសម្រួលច្រើន Files ព្រមគ្នា និងបញ្ជា Shell Commands។
3. **ផ្ទៀងផ្ទាត់លទ្ធផល (Verify Results)**៖ Claude ដំណើរការ Test Suites, ពិនិត្យ Linter និង Compiler Output។ ប្រសិនបើមាន Error វានឹងវិភាគរកមូលហេតុដើម រួចកែសម្រួលដោយស្វ័យប្រវត្តិ។

### ផ្ទៃដំណើរការដែលអាចប្រើបាន (Surfaces)
Claude Code អាចប្រើបាននៅលើបរិស្ថានជាច្រើន ដោយប្រើម៉ាស៊ីនស្នូល (Engine) តែមួយ៖

| ផ្ទៃដំណើរការ (Surface) | ស័ក្តិសមបំផុតសម្រាប់ | លក្ខណៈពិសេស |
| :--- | :--- | :--- |
| **Terminal CLI** | អ្នកចូលចិត្ត command line | គាំទ្រ Unix Pipeline ពេញលេញ (`tail -n 50 app.log \| claude -p ...`), ល្បឿនលឿន។ |
| **VS Code / Cursor** | អ្នកសរសេរកូដលើ Editor | បង្ហាញ diff ផ្ទាល់ក្នុងកូដ, `@-mentions`, ផ្ទាំងពិនិត្យ Plan, ផ្ទាំងចំហៀង។ |
| **JetBrains IDEs** | IntelliJ, PyCharm, WebStorm | ឧបករណ៍មើល diff អន្តរកម្ម, ចែករំលែកកូដដែលបានជ្រើសរើស (selection context)។ |
| **Desktop App** | កម្មវិធីដាច់ដោយឡែក | គ្រប់គ្រងច្រើន Session ស្របគ្នា, បង្ហាញ diff ងាយស្រួល, គាំទ្រ iOS Simulator។ |
| **Web (`claude.ai/code`)** | ដំណើរការលើ Cloud | មិនបាច់ Setup លើម៉ាស៊ីនផ្ទាល់ខ្លួន, ដំណើរការលើ Server Cloud, គាំទ្រលើទូរស័ព្ទដៃ។ |
| **Remote Control** | ភាពបត់បែនខ្ពស់ | បញ្ជា Terminal លើកុំព្យូទ័ររបស់អ្នក ពីចម្ងាយតាមរយៈទូរស័ព្ទដៃ ឬ Browser។ |

---

## មេរៀនទី ២៖ ការដំឡើង និងការកំណត់ផ្ទៀងផ្ទាត់

### ១. របៀបដំឡើង (Installation)

#### ដំឡើងតាម Native Script (ណែនាំបំផុត)
ការដំឡើងតាម Native នឹងមានប្រព័ន្ធ Auto-update ដោយស្វ័យប្រវត្តិដើម្បីទទួលបានមុខងារ និងសុវត្ថិភាពចុងក្រោយជានិច្ច។

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

#### ដំឡើងតាមរយៈ Package Managers
- **macOS / Linux (Homebrew):**
  ```bash
  # កំណែ Stable
  brew install --cask claude-code
  
  # កំណែ Latest
  brew install --cask claude-code@latest
  ```
- **Windows (WinGet):**
  ```powershell
  winget install Anthropic.ClaudeCode
  ```
- **Linux Packages:** អាចដំឡើងតាម `apt`, `dnf`, ឬ `apk` សម្រាប់ Debian, Ubuntu, Fedora, RHEL, និង Alpine។

ពិនិត្យមើលថាតើការដំឡើងជោគជ័យដែរឬទេ៖
```bash
claude --version
```

### ២. ជម្រើសនៃការ Login និងការកំណត់សិទ្ធិ (Authentication)

Claude Code គាំទ្រការ Login ជា ៣ របៀប៖

1. **គណនី Claude Subscription (លំនាំដើម និងពេញនិយមបំផុត):**
   ដំណើរការ `claude` នៅក្នុង Folder គម្រោងណាមួយ វានឹងបើក Browser ឱ្យអ្នក Login តាមរយៈគណនី Claude Pro, Max, Team ឬ Enterprise។
2. **Anthropic Console API Key:**
   កំណត់ Environment Variable ក្នុង Shell (`~/.bashrc` ឬ `~/.zshrc`)៖
   ```bash
   export ANTHROPIC_API_KEY="sk-ant-api..."
   ```
3. **Enterprise Cloud Providers:**
   សម្រាប់ស្ថាប័ន ឬក្រុមហ៊ុនធំៗ អ្នកអាចភ្ជាប់ Claude Code តាមរយៈ៖
   - **Amazon Bedrock**
   - **Google Cloud Agent Platform (Vertex AI ពីមុន)**
   - **Microsoft Foundry**

---

## មេរៀនទី ៣៖ ដំណើរការការងារប្រចាំថ្ងៃ

### ១. ការស្វែងយល់ និងរុករកគម្រោងកូដថ្មី (Exploring a Codebase)
នៅពេលអ្នកទើបតែចូលរួមគម្រោង ឬចង់ស្វែងយល់ពី Component ណាមួយ៖

```bash
cd my-project
claude
```
```text
> ពន្យល់ពីរបៀបដែលគម្រោងនេះគ្រប់គ្រង Authentication និងការ Refresh JWT Token។
```
*Claude នឹងស្វែងរក Controller, Middleware និងឯកសារពាក់ព័ន្ធ រួចសង្ខេបជាមួយតំណភ្ជាប់កូដយ៉ាងច្បាស់លាស់។*

### ២. ការបង្កើតមុខងារថ្មី (Building Features)
រៀបរាប់ពីអ្វីដែលអ្នកចង់បានជាភាសាសាមញ្ញ៖

```text
> បង្កើត endpoint ថ្មី POST /api/v1/export/csv សម្រាប់ Export log សកម្មភាពអ្នកប្រើប្រាស់។ 
  ត្រូវប្រាកដថាបានដាក់ Middleware requireAuth និងសរសេរ Unit Test ផងដែរ។
```

### 3. ការដោះស្រាយបញ្ហា និងជួសជុល Bug (Debugging & Self-Healing)
អ្នកគ្រាន់តែ Copy Error Message ឬ Log បិទចូលក្នុង Claude៖

```text
> កូដតេស្តកំពុងបរាជ័យក្នុង tests/payments.test.ts: "Error: Stripe invalid payload signature"។ 
  សូមពិនិត្យមើល Webhook Middleware រកមូលហេតុដើម កែសម្រួលវា និងរត់ Test ផ្ទៀងផ្ទាត់ឡើងវិញ។
```

### ៤. ការកែទម្រង់កូដទូទាំងគម្រោង (Codebase-Wide Refactoring)
```text
> ប្តូរការសរសេរ Database Query ទាំងអស់ក្នុង src/services/ ពី Raw SQL ទៅប្រើ Kysely Query Builder វិញ។ 
  បន្ទាប់មកសូមរត់ Type check និង Linter ផ្ទៀងផ្ទាត់។
```

### ៥. ការគ្រប់គ្រង Git ដោយស្វ័យប្រវត្តិ (Automated Git Operations)
```text
> Stage កូដដែលទើបកែរួច បង្កើត Branch ឈ្មោះ feat/user-export 
  ហើយ Commit ជាមួយសារ Conventional-commit ដ៏ច្បាស់លាស់មួយ។
```

---

## មេរៀនទី ៤៖ ផ្ទាំងបញ្ជា Terminal UI និងការបញ្ជាផ្លូវកាត់

### គ្រាប់ចុចកាត់ និងការបញ្ជាសំខាន់ៗ

| គ្រាប់ចុចកាត់ (Shortcut) | សកម្មភាព | ការពិពណ៌នា |
| :--- | :--- | :--- |
| `Enter` | **ផ្ញើសារ** | ដាក់បញ្ជាទៅកាន់ Claude។ |
| `Shift + Enter` | **ចុះបន្ទាត់ថ្មី** | ចុះបន្ទាត់ដោយមិនទាន់ផ្ញើ (Multiline input)។ |
| `Esc` | **បញ្ឈប់ភ្លាមៗ** | បញ្ឈប់សកម្មភាពដែល Claude កំពុងធ្វើ។ |
| `Esc` ២ដង | **Undo កូដត្រឡប់ក្រោយ** | ត្រឡប់កូដដែលបានកែទៅកាន់ស្ថានភាពមុន (Checkpoint Restore)។ |
| `Shift + Tab` | **ប្តូររបៀបសិទ្ធិ** | ប្តូររវាង `Auto` ➔ `Manual` ➔ `Accept Edits` ➔ `Plan`។ |
| វាយអក្សរពេល Claude កំពុងរត់ + `Enter` | **តម្រង់ជួរសារ (Queue)** | សាររបស់អ្នកនឹងត្រូវបានរក្សាទុក ហើយ Claude នឹងអានវាភ្លាមៗពេលចប់ជំហានបច្ចុប្បន្ន។ |

### បញ្ជី Slash Commands សំខាន់ៗ

- `/init` — វិភាគគម្រោងកូដ និងបង្កើតឯកសារ `CLAUDE.md` ដំបូងដោយស្វ័យប្រវត្តិ។
- `/doctor` — ពិនិត្យសុខភាពបរិស្ថាន (PATH, កំណែ Node/Python, ការតភ្ជាប់ Network, Configs)។
- `/diff` — បើកផ្ទាំងមើលការកែប្រែកូដ (Visual diff) ជាក់ស្តែង។
- `/context` — បង្ហាញទំហំ Token ដែលកំពុងប្រើប្រាស់ ព្រមទាំង Files និង Tools ដែលបានផ្ទុកក្នុង Context។
- `/compact [ប្រធានបទ]` — បង្រួមប្រវត្តិសន្ទនា ដើម្បីសន្សំទំហំ Context Window ដោយរក្សាទុកចំណុចសំខាន់ៗ។
- `/clear` — សម្អាតប្រវត្តិសន្ទនាដើម្បីចាប់ផ្តើមជុំថ្មីស្អាត។
- `/rewind` — ជ្រើសរើសត្រឡប់ទៅចំណុចសន្ទនា ឬស្ថានភាពកូដកាលពីមុន។
- `/model` — ប្តូរម៉ូដែល AI ភ្លាមៗ (Sonnet, Opus, Haiku)។
- `/cost` — បង្ហាញចំនួន Token និងតម្លៃចំណាយប៉ាន់ស្មាននៃ Session នេះ។
- `/desktop` — ផ្ទេរការសន្ទនាបច្ចុប្បន្នទៅកាន់ Claude Desktop App។

---

## មេរៀនទី ៥៖ ប្រព័ន្ធចងចាំ និងការគ្រប់គ្រងបរិបទ

```mermaid
flowchart TD
    subgraph Storage ["ការផ្ទុកការចងចាំ និងច្បាប់គម្រោង"]
        Global["~/.claude/CLAUDE.md<br>(ច្បាប់ទូទៅសម្រាប់ User ទាំងមូល)"]
        Project["./CLAUDE.md<br>(ច្បាប់ស្តង់ដារប្រចាំគម្រោង)"]
        Nested[".claude/rules/*.md<br>(ច្បាប់សម្រាប់ Folder ជាក់លាក់)"]
        AutoMem["Auto Memory<br>(~/.claude/projects/.../MEMORY.md)"]
    end

    subgraph RuntimeContext ["Context Window របស់ Claude"]
        System["System Harness + Tools"]
        PromptCache["ស្រទាប់ Prompt Caching"]
        SessionMsgs["ប្រវត្តិសន្ទនា និង Files ដែលបានអាន"]
    end

    Global --> PromptCache
    Project --> PromptCache
    Nested --> PromptCache
    AutoMem --> PromptCache
    PromptCache --> RuntimeContext
```

### ១. ឯកសារណែនាំ `CLAUDE.md`
`CLAUDE.md` គឺជាឯកសារ Markdown ដែល Claude អានរាល់ពេលចាប់ផ្តើម Session ថ្មីក្នុងគម្រោង។ គួររក្សាទុកឱ្យខ្លីក្រោម ២០០ បន្ទាត់ ដោយផ្តោតលើចំណុចសំខាន់ៗ៖

```markdown
# ស្តង់ដារគម្រោង៖ សេវាកម្ម Billing Service

## ពាក្យបញ្ជា Build & Test
- ដំឡើង Dependencies: `pnpm install`
- ដំណើរការ Dev: `pnpm dev`
- រត់តេស្ត: `pnpm test`
- រត់តេស្តតែមួយ: `pnpm test -t "<ឈ្មោះតេស្ត>"`
- ពិនិត្យ Linter: `pnpm lint:fix`

## ច្បាប់សរសេរកូដ និងស្ថាបត្យកម្ម
- ប្រើ `camelCase` សម្រាប់អថេរ និង `PascalCase` សម្រាប់ React Components។
- មិនត្រូវសរសេរ Raw SQL ដោយផ្ទាល់ទេ ត្រូវប្រើ Repository Pattern ក្នុង `src/repositories/`។
- តម្លៃលុយទាំងអស់ត្រូវរក្សាទុកជាលេខគត់ Cents (Integer) មិនត្រូវប្រើចំនួនទសភាគ Float ឡើយ។
```

> [!TIP]
> អ្នកអាចប្រើ `@path/to/file` នៅក្នុង `CLAUDE.md` ដើម្បីទាញយកឯកសារយោងមកប្រើប្រាស់តាមតម្រូវការ ដោយមិនបាច់សរសេរទាំងអស់ក្នុងឯកសារតែមួយឡើយ។

### ២. ប្រព័ន្ធ Auto Memory
Claude Code នឹងរៀនពីចំណង់ចំណូលចិត្តរបស់អ្នកដោយស្វ័យប្រវត្តិកំឡុងពេលធ្វើការជាមួយគ្នា (ដូចជា Package Manager ដែលអ្នកចូលចិត្ត របៀបដាក់ឈ្មោះ branch) ហើយកត់ត្រាទុកក្នុង Auto-Memory ដើម្បីកុំឱ្យអ្នកត្រូវប្រាប់ដដែលៗនៅពេលក្រោយ។

---

## មេរៀនទី ៦៖ ស្រទាប់បន្ថែមសមត្ថភាព

```mermaid
classDiagram
    class CLAUDE_MD {
        ច្បាប់គម្រោងជាប់ជានិច្ច
        ផ្ទុកឡើងរាល់ Session
    }
    class Skills {
        លំហូរការងារ & ឯកសារយោង
        ដំណើរការតាម /command ឬតាមតម្រូវការ
    }
    class Subagents {
        ភ្នាក់ងាររងក្នុង Context ដាច់ដោយឡែក
        ការងារស្របគ្នា & ជំនាញឯកទេស
    }
    class MCP {
        Model Context Protocol
        ភ្ជាប់ Database, Tools & APIs ក្រៅ
    }
    class Hooks {
        សកម្មភាពស្វ័យប្រវត្តិតាម Event
        Lint ពេលកែកូដ, ការពារកូដគ្រោះថ្នាក់
    }
    class Plugins {
        កញ្ចប់ចែកចាយមុខងារ
        វេចខ្ចប់ Skills, Hooks, MCP & Agents
    }

    Plugins --> Skills
    Plugins --> Subagents
    Plugins --> MCP
    Plugins --> Hooks
```

### ១. ការបង្កើត Skills ផ្ទាល់ខ្លួន (`.claude/skills/<name>.md`)
Skills គឺជាឯកសារណែនាំពីជំហានការងារដែលអ្នកអាចហៅប្រើបានគ្រប់ពេលតាមរយៈ `/<ឈ្មោះ-skill>`៖

```markdown
---
name: deploy-staging
description: ពិនិត្យកូដមុនចេញ, Build Docker Image, និង Deploy ទៅកាន់ Staging Kubernetes
disable-model-invocation: true
---

# ជំហាន Deploy ទៅកាន់ Staging

នៅពេល Skill នេះត្រូវបានហៅ៖
1. រត់ `pnpm test` ដើម្បីប្រាកដថាតេស្តទាំងអស់ជាប់។
2. រត់ `pnpm build` ដើម្បីផ្ទៀងផ្ទាត់ថាកូដ Production អាច Compile បាន។
3. ដំណើរការ Script: `./scripts/deploy.sh staging`។
4. ពិនិត្យការ Rollout: `kubectl rollout status deployment/web -n staging`។
5. ផ្ញើសេចក្តីសង្ខេបពីលទ្ធផល Deploy ជូនអ្នកប្រើប្រាស់។
```

### ២. ការបង្កើត Subagents (`.claude/agents/<name>.md`)
Subagents ដំណើរការនៅក្នុង **Context Window ដាច់ដោយឡែក** ដែលជួយកុំឱ្យ Context ចម្បងរបស់អ្នកត្រូវកកស្ទះនៅពេលស្រាវជ្រាវឯកសាររាប់សិប Files៖

```markdown
---
name: security-auditor
description: ពិនិត្យកូដដែលបានកែប្រែ ដើម្បីស្វែងរកចន្លោះប្រហោងសុវត្ថិភាព និងការបែកធ្លាយ Password/Token
tools:
  - ReadFile
  - Grep
  - Glob
---
អ្នកគឺជាវិស្វករសន្តិសុខកម្មវិធីជាន់ខ្ពស់ (Application Security Engineer)។ សូមពិនិត្យកូដឱ្យបានល្អិតល្អន់រកមើលបញ្ហា OWASP Top 10, SQL Injections, XSS និង Hardcoded Secrets រួចផ្តល់ដំណោះស្រាយជាក់ស្តែង។
```

### ៣. Model Context Protocol (MCP)
MCP អនុញ្ញាតឱ្យ Claude ភ្ជាប់ទៅកាន់ប្រព័ន្ធខាងក្រៅដូចជា Database, GitHub, Slack, Linear, Google Drive។ កំណត់ក្នុង `.claude/settings.json`៖

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

### ៤. Lifecycle Hooks (`.claude/hooks/`)
Hooks ដំណើរការ Script ដោយស្វ័យប្រវត្តិតាមព្រឹត្តិការណ៍ជាក់លាក់ (Deterministic Automation)៖
- **`PreToolUse`**៖ រារាំងសកម្មភាពគ្រោះថ្នាក់ (ឧទាហរណ៍៖ ហាមកែ `.env` ឬហាមរត់ `rm -rf`)។
- **`PostToolUse`**៖ Format កូដដោយស្វ័យប្រវត្តិរាល់ពេលកែឯកសាររួច (`prettier` / `ruff`)។
- **`SessionStart`**៖ រៀបចំបរិស្ថាន ឬបង្ហាញសារស្វាគមន៍។
- **`Stop`**៖ បន្លឺសំឡេង ឬផ្ញើសារចូល Slack ពេលកិច្ចការចប់។

ឧទាហរណ៍ Hook ក្នុង `settings.json`៖
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

### ៥. Plugins & Marketplaces (ឧទាហរណ៍លេចធ្លោ៖ `frontend-design`)
Plugins គឺជាកញ្ចប់វេចខ្ចប់ Skills, Subagents, MCP Servers និង Hooks ចូលគ្នា ដើម្បីងាយស្រួលដំឡើង និងចែករំលែកតាមរយៈ Anthropic Marketplace។

ឧទាហរណ៍ជាក់ស្តែងដ៏ពេញនិយមបំផុតគឺ Plugin ផ្លូវការ **Frontend Design** (`frontend-design@claude-plugins-official` ដែលមានអ្នកដំឡើងលើសពី ១.១ លាននាក់)៖
- **ពាក្យបញ្ជាដំឡើង**៖ `claude plugin install frontend-design@claude-plugins-official`
- **សមត្ថភាព**៖ កំណត់ឱ្យ Claude បង្កើតផ្ទាំង UI/UX លំដាប់ Production ដ៏ស្រស់ស្អាត ប្លែកគេ និងលុបបំបាត់ចោលនូវរចនាបថ AI ដដែលៗ។
- **សៀវភៅណែនាំលម្អិត**៖ សូមអាន [FRONTEND_DESIGN_PLUGIN_KM.md](./FRONTEND_DESIGN_PLUGIN_KM.md) សម្រាប់របៀបប្រើ និងគំរូ Prompt ជាក់ស្តែង។

---

## មេរៀនទី ៧៖ សិទ្ធិ សុវត្ថិភាព និង Sandboxing

### របៀបសិទ្ធិទាំង ៤ (Permission Modes)

```mermaid
stateDiagram-v2
    [*] --> AutoMode
    AutoMode --> ManualMode: Shift+Tab
    ManualMode --> AcceptEditsMode: Shift+Tab
    AcceptEditsMode --> PlanMode: Shift+Tab
    PlanMode --> AutoMode: Shift+Tab

    AutoMode: Auto Mode (AI Classifier វាយតម្លៃសុវត្ថិភាពពីក្រោយដោយស្វ័យប្រវត្តិ)
    ManualMode: Manual Mode (សួររាល់ពេលកែកូដ ឬរត់ Command នីមួយៗ)
    AcceptEditsMode: Accept Edits (អនុញ្ញាតឱ្យកែកូដបាន តែសួររាល់ពេលរត់ Command ធំៗ)
    PlanMode: Plan Mode (សម្រាប់តែការរុករក និងរៀបចំ Plan ដោយមិនកែ File កូដឡើយ)
```

### ការត្រឡប់កូដដោយសុវត្ថិភាព (Reversible Checkpoints)
រាល់ពេល Claude កែប្រែឯកសារណាមួយ វានឹងថតចម្លង Snapshot ទុកជាមុន។
- ចុច **`Esc` ២ដង** ឬវាយ `/rewind` ដើម្បីត្រឡប់កូដទៅកាន់ស្ថានភាពដើមភ្លាមៗ ដោយមិនប៉ះពាល់ដល់ Git History ឡើយ។

### ការកំណត់ច្បាប់ជាក់លាក់ក្នុង `.claude/settings.json`
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

## មេរៀនទី ៨៖ ការសម្របសម្រួលភ្នាក់ងារច្រើន និងការងារស្របគ្នា

### ១. ការបំបែកការងារជាមួយ Git Worktrees (`--worktree`)
ដើម្បីដំណើរការកិច្ចការច្រើនស្របគ្នាដោយមិនបាច់បារម្ភពីរឿងជាន់ Branch គ្នា៖

```bash
# ចាប់ផ្តើម Session ក្នុង Worktree ដាច់ដោយឡែកមួយ
claude --worktree feat/refactor-auth
```

### ២. Dynamic Workflows (`/workflows`)
សម្រាប់កិច្ចការធំៗ (ដូចជាការកែសម្រួល Endpoints លើសពី ១០០ ឬការត្រួតពិនិត្យគម្រោងទាំងមូល) Dynamic Workflows នឹងបង្កើត Script ស្វ័យប្រវត្តិ ដើម្បីបញ្ជាភ្នាក់ងាររងរាប់សិបនាក់ឱ្យធ្វើការស្របគ្នា រួចចងក្រងលទ្ធផលផ្ទៀងផ្ទាត់តែមួយជូនអ្នក។

```text
> ដំណើរការ dynamic workflow ដើម្បី audit controllers ទាំងអស់រកមើលកន្លែងដែលខ្វះ Input Validation។
```

---

## មេរៀនទី ៩៖ ការប្រើប្រាស់តាមរយៈកូដកម្មវិធី (Claude Agent SDK)

**Claude Agent SDK** (សម្រាប់ TypeScript និង Python) អនុញ្ញាតឱ្យអ្នកបញ្ចូលសមត្ថភាពរបស់ Claude Code ទៅក្នុងកម្មវិធី Backend, ប្រព័ន្ធ CI/CD ឬ Script ស្វ័យប្រវត្តិកម្ម។

### ឧទាហរណ៍ TypeScript SDK
```typescript
import { ClaudeAgent } from "@anthropic-ai/claude-agent-sdk";

async function runAutonomousReview() {
  const agent = new ClaudeAgent({
    projectDir: process.cwd(),
    permissionMode: "acceptEdits",
  });

  const result = await agent.run({
    prompt: "ពិនិត្យមើល commit ថ្មីៗលើ branch នេះ សរសេរ test បន្ថែមសម្រាប់អនុគមន៍ដែលមិនទាន់មាន test និងផ្ញើរបាយការណ៍សង្ខេប។",
  });

  console.log("ស្ថានភាព:", result.status);
  console.log("សេចក្តីសង្ខេប:", result.summary);
}

runAutonomousReview();
```

### ការប្រើប្រាស់តាម Unix CLI Pipeline & Headless
```bash
# បញ្ជូន Log Error ទៅឱ្យ Claude វិភាគ និងរកវិធីដោះស្រាយ
tail -n 100 /var/log/app.error.log | claude -p "វិភាគរកមូលហេតុនៃ Error ទាំងនេះ និងស្នើដំណោះស្រាយ។"

# ពិនិត្យសុវត្ថិភាពលើ Files ដែលបានកែប្រែក្នុង Git
git diff main --name-only | claude -p "ពិនិត្យមើល Files ទាំងនេះថាតើមានចន្លោះប្រហោងសុវត្ថិភាពដែរឬទេ?"
```

---

## មេរៀនទី ១០៖ គន្លឹះអនុវត្តល្អៗ និងការដោះស្រាយបញ្ហា

### ១. ផ្នត់គំនិត "Delegate, Don't Dictate" (ចាត់ចែង មិនមែនបញ្ជាលម្អិតពេក)
- កុំបញ្ជាគ្រប់បន្ទាត់កូដ ឬគ្រប់ Syntax តូចតាចពេក។
- ផ្តល់បរិបទ រោគសញ្ញានៃបញ្ហា និងលទ្ធផលចុងក្រោយដែលចង់បាន។ ទុកឱ្យ Claude ស្វែងយល់ រៀបចំផែនការ និងអនុវត្តដោយខ្លួនឯង។

### ២. ការគ្រប់គ្រងទំហំ Context និងការសន្សំសំចៃ Token
- ដំណើរការ `/compact` ម្តងម្កាល ប្រសិនបើសន្ទនាយូរម៉ោង។
- រក្សា `CLAUDE.md` ឱ្យខ្លីក្រោម ២០០ បន្ទាត់។
- ប្រើ Subagents សម្រាប់ការងារស្រាវជ្រាវវែងៗ ដើម្បីកុំឱ្យខូច Context ចម្បង។
- ដាក់ `disable-model-invocation: true` លើ Skills ណាដែលអ្នកចង់ហៅប្រើដោយដៃផ្ទាល់។

### ៣. ឧបករណ៍វិនិច្ឆ័យ និងដោះស្រាយបញ្ហា
- **បញ្ហាដំឡើង ឬ Config មិនដំណើរការ:** វាយ `/doctor` ដើម្បីឱ្យប្រព័ន្ធស្កេន និងជួសជុលដោយស្វ័យប្រវត្តិ។
- **ពិនិត្យទំហំ Context:** វាយ `/context` ដើម្បីដឹងថាឯកសារណាខ្លះកំពុងស៊ីទំហំច្រើន។
- **បញ្ហា MCP Server មិនភ្ជាប់:** វាយ `/mcp` ដើម្បីពិនិត្យស្ថានភាព Connection និងភ្ជាប់ឡើងវិញ។
