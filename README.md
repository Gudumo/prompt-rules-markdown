# prompt-rules-markdown

A drop-in rule system for managing LLM-assisted development. Copy these files into any project to give LLMs (Claude, Codex, Qwen, etc.) a consistent process for planning, implementing, testing, and committing code.

## Files

| File | Purpose |
|------|---------|
| `PROMPT_RULES.md` | Core process — wave/feature/step workflow, naming, protocols, general rules. Read every prompt. |
| `PROJECT_SETUP.md` | Tech stack pick-lists, environment setup, README template. Read once at project init. |
| `FRONTEND_RULES.md` | UI conventions, design reference system, visual QA protocol. Read for frontend steps. |

## How to use

1. Copy the three `.md` files into your project root.
2. Open `PROJECT_SETUP.md` and delete every stack option you don't use. Fill in your versions.
3. Before any LLM prompt, say: **"Read PROMPT_RULES.md first and follow it."**
4. For frontend work, add: **"Also read FRONTEND_RULES.md."**

## What it covers

- **Wave-based planning** — break work into Waves > Features > Steps with clear IDs (`18-F1-S2`)
- **Multi-LLM coordination** — assign steps to different models, handle parallel execution and handoffs
- **Step progress tracking** — checkbox-based tracking in wave overview files
- **Completion protocol** — structured output format so results are parseable across models
- **Testing expectations** — LLM writes tests + provides manual test commands for the user
- **Design reference system** — drop inspiration screenshots, LLM compares its work against them
- **Error recovery & rollback** — retry logic, safe revert procedures
- **Environment & secrets** — `.env.example` sync, `.gitignore` rules, bootstrap script

## License

MIT
