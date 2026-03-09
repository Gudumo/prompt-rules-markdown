# Prompt Rules for LLM-Assisted Development

These rules apply to every wave/step prompt given to any LLM (Claude, Codex, Qwen, etc.) working on this codebase.

## How to Use These Files

| File | When to read | Content |
|------|-------------|---------|
| `PROMPT_RULES.md` | **Every prompt** | Core process, naming, protocols, general rules |
| `PROJECT_SETUP.md` | **Once** at project init | Tech stack pick-lists, env setup, README template |
| `FRONTEND_RULES.md` | **Frontend steps only** | Conventions, design reference, visual QA |

**First time?** Copy these files into your project, then say: **"Read PROMPT_RULES.md and set up this project."** The LLM will walk you through configuring everything interactively.

**After setup:** Before any prompt, say: **"Read PROMPT_RULES.md first and follow it."**
For frontend steps, add: **"Also read FRONTEND_RULES.md."**

## Quick Commands

All commands assume the LLM has already read this file.

### Project init

| Command | What it does |
|---------|-------------|
| `Set up this project.` | Reads `PROJECT_SETUP.md`, asks you questions, configures all files |

### Planning & status

| Command | What it does |
|---------|-------------|
| `Give me a dev status update.` | Health check + progress report → updates `DEV_STATUS.md` |
| `Plan wave X: [scope]` | Creates wave folder + feature files → updates `PLAN_OVERVIEW.md` |
| `Give me an overview of wave X.` | Returns the wave overview with linked features and model assignments |
| `Run step 18-F1-S2.` | Reads the feature file, implements the step |
| `Verify feature 18-F1.` | Build + tests + review all changed files for the feature |

### Testing

| Command | What it does |
|---------|-------------|
| `Run tests.` | `cd .devteam/engine && npm test` — report pass/fail count |
| `Run integration tests.` | `cd .devteam/engine && npm run test:integration` — requires env vars |
| `Run build.` | `cd .devteam/engine && npm run build` — report pass/fail |
| `Run tests and build.` | Both in sequence, report combined result |

### Commit & push

| Command | What it does |
|---------|-------------|
| `Commit feature 18-F1.` | Stage changed files, commit with `feat(wave-18): <description>` |
| `Commit and push feature 18-F1.` | Same + push to current branch |
| `Commit all completed features.` | One commit per completed+verified feature, then push |

Commit protocol:
1. Run `git status` and `git diff --stat` to review what will be committed.
2. Only stage files relevant to the feature (not unrelated changes).
3. Commit message format: `feat(wave-18): <feature short name>` + co-author line.
4. After commit, run tests once more to confirm nothing broke.
5. **Always ask the user for confirmation before pushing.**
6. Never force-push. Never push to main without asking.

### Dev server / localhost

| Command | What it does |
|---------|-------------|
| `Start dev server.` | Ask user which port to use, then start |
| `Start serve on port X.` | Start webhook server on specified port |

Port protocol:
1. Before starting any localhost server, **ask the user which port to use**.
2. Suggest a default (3737 for `devteam serve`, 1234 for LM Studio) but always confirm.
3. Check if the port is already in use: run `lsof -i :<port>` (Unix) or `netstat -ano | findstr :<port>` (Windows).
4. If the port is busy, tell the user and ask for an alternative.
5. Update `config.json` `server.port` if the user picks a non-default port.

---

## .dev-plan Folder Structure

All planning files live in `.dev-plan/` at the repo root. The planner creates this on first use.

```
.dev-plan/
  PLAN_OVERVIEW.md          — master index of all waves (links to everything)
  DEV_STATUS.md             — current health, progress, blockers (updated every session)
  design-reference/         — inspiration images + LLM screenshots (see FRONTEND_RULES.md)
  wave-18/
    OVERVIEW.md             — wave overview: feature list, model assignments, parallelism
    18-F1-fix-command.md    — feature file: description + all step prompts
    18-F2-pre-plan-hooks.md
    18-F3-config-profiles.md
    ...
  wave-19/
    OVERVIEW.md
    19-F1-some-feature.md
    ...
```

### File naming rules

| Type | Pattern | Example |
|------|---------|---------|
| Wave folder | `wave-<N>/` | `wave-18/` |
| Wave overview | `wave-<N>/OVERVIEW.md` | `wave-18/OVERVIEW.md` |
| Feature file | `wave-<N>/<ID>-<slug>.md` | `wave-18/18-F1-fix-command.md` |
| Plan overview | `PLAN_OVERVIEW.md` | (root of `.dev-plan/`) |
| Dev status | `DEV_STATUS.md` | (root of `.dev-plan/`) |

### Source code naming (unchanged)

| Type | Pattern | Example |
|------|---------|---------|
| Source file | `src/<module>/<name>.ts` | `src/cli/errorParser.ts` |
| Test file | `src/<module>/__tests__/<name>.test.ts` | `src/cli/__tests__/errorParser.test.ts` |
| Integration test | `src/<module>/__tests__/integration.test.ts` | `src/llm/__tests__/integration.test.ts` |
| E2E test | `src/__tests__/e2e.<variant>.test.ts` | `src/__tests__/e2e.lmstudio.test.ts` |

---

## Naming Hierarchy

```
Wave  →  Feature  →  Step
 18       18-F1       18-F1-S1
```

- **Wave**: a numbered release milestone (Wave 18, Wave 19, ...).
- **Feature**: a self-contained capability within a wave. ID: `<wave>-F<n>` (e.g. `18-F1`).
- **Step**: an atomic unit of work within a feature. ID: `<wave>-F<n>-S<m>` (e.g. `18-F1-S2`).

### In prompts and commits

- Always use full step IDs: `18-F1-S2` (not "step 1.2")
- Commit messages: `feat(wave-18): <short description>` — one commit per feature
- Branch names: `wave-18/fix-command`, `wave-18/config-profiles`

---

## Wave Overview File Format

Each `wave-<N>/OVERVIEW.md` lists all features in a readable format. Feature names link to their feature files.

```markdown
# Wave 18 Overview

## Features

### 01 — [fix command](18-F1-fix-command.md)
- Severity: High
- Steps: 4 (S1 ‖ S2, then S3 → S4)
- Qwen: S1, S4
- Claude: S2, S3
- Progress:
  - [x] 18-F1-S1 — Config schema + defaults
  - [x] 18-F1-S2 — Error parser utility + tests
  - [ ] 18-F1-S3 — CLI wiring
  - [ ] 18-F1-S4 — Integration test

### 02 — [pre-plan hooks](18-F2-pre-plan-hooks.md)
- Severity: Medium
- Steps: 3 (all sequential)
- Qwen: S1, S3
- Claude: S2
- Progress:
  - [x] 18-F2-S1 — Hook schema
  - [x] 18-F2-S2 — Hook runner
  - [x] 18-F2-S3 — CLI integration

### 03 — [config profiles](18-F3-config-profiles.md)
- Severity: Medium
- Steps: 4 (S3 ‖ S4)
- Qwen: S1, S4
- Claude: S2, S3
- Progress:
  - [ ] 18-F3-S1 — Profile schema
  - [ ] 18-F3-S2 — Profile loader
  - [ ] 18-F3-S3 — CLI switcher
  - [ ] 18-F3-S4 — Tests

## Cross-feature parallelism

- Features 1, 2, 6 can run in parallel (different files).
- Feature 3 must complete before Feature 4.
- Features 5 and 7 are independent.
```

### Step progress tracking

Each feature in the wave overview includes a **Progress** checklist using GitHub-flavored markdown checkboxes (`- [x]` / `- [ ]`).

Update rules:
1. When a step is completed (COMPLETED block reported), the LLM MUST mark the step `[x]` in the wave overview file.
2. When a step is blocked, leave it as `[ ]` — do not add a third state. The BLOCKED status lives in the step output, not here.
3. A feature is "Done" when all its checkboxes are checked.
4. The planner creates the checklist when generating the wave. All steps start as `[ ]`.

---

## Feature File Format

Each feature file (`18-F1-fix-command.md`) contains:

1. A short description of the feature
2. A step list with metadata
3. The full prompt for each step

```markdown
# 18-F1: fix command

`devteam fix "<error>"` — plan from an error message or stack trace.

## Steps

  Step       | Complexity | Model  | Depends on         | Parallel with
  18-F1-S1   | 1          | Qwen   | --                 | 18-F1-S2
  18-F1-S2   | 3          | Claude | --                 | 18-F1-S1
  18-F1-S3   | 3          | Claude | 18-F1-S1, S2      | --
  18-F1-S4   | 2          | Qwen   | 18-F1-S3          | --

## 18-F1-S1 — Config schema + defaults

  STEP: 18-F1-S1
  MODEL: Qwen
  PARALLEL WITH: 18-F1-S2
  DEPENDS ON: (none)

  [prompt text here]

## 18-F1-S2 — Error parser utility + tests

  STEP: 18-F1-S2
  MODEL: Claude
  PARALLEL WITH: 18-F1-S1
  DEPENDS ON: (none)

  [prompt text here]
```

The step list at the top uses plain indented text, not markdown tables, for readability.

---

## Prompt Header Block

Every step prompt starts with this metadata:

```
STEP: 18-F1-S2
MODEL: Claude
PARALLEL WITH: 18-F1-S1
DEPENDS ON: (none)
```

The LLM reads this to know its assignment. If the step says `MODEL: Qwen`, do not run it with Claude (and vice versa) unless falling back.

---

## Completion Protocol

When done implementing a step, the LLM MUST end with:

```
=== STEP 18-F1-S2 COMPLETED ===
Files changed: [list]
Tests: [count passing] / [count total]
Build: [pass/fail]
Notes: [any gotchas or follow-up needed]
```

If blocked:

```
=== STEP 18-F1-S2 BLOCKED ===
Reason: [what went wrong]
Action needed: [what must happen before retrying]
```

---

## Complexity Scale

```
       Human estimate    LLM estimate
1 = trivial      ~10 min       ~2 min     (schema tweak, add a flag)
2 = simple       ~20 min       ~5 min     (mechanical tests, small utility)
3 = moderate     ~45 min       ~10 min    (new function with logic, CLI wiring)
4 = substantial  ~1.5h         ~20 min    (flow modification, multi-file changes)
5 = complex      ~3h+          ~45 min+   (new subsystem, prompt engineering)
```

LLM estimates assume a standard-tier model (Claude Sonnet, Qwen 32B, Codex) with full context. Actual time varies with model speed, context size, and retry loops.

Model recommendations:
- **Qwen**: complexity 1–2
- **Claude**: complexity 3–5
- **Codex**: large refactors, boilerplate across many files

---

## General Rules

1. **Read before writing** — always read a file before modifying it.
2. **Run tests** after every change — `cd .devteam/engine && npm test`.
3. **Run build** after every change — `cd .devteam/engine && npm run build`.
4. **Add new test files** to `package.json` `scripts.test` (not auto-discovered).
5. **Follow existing patterns** — match code style and structure of adjacent files.
6. **No over-engineering** — implement exactly what the step asks for.
7. **ESM imports** — always use `.js` extension in import paths.
8. **Test runner** — Node.js built-in `node:test` + `tsx`. No Jest, no Vitest.
9. **Never auto-commit** — leave changes for user review.
10. **Never commit secrets** — `.env`, API keys, tokens, credentials, and private config must never be staged or committed. If spotted in a diff, warn the user immediately.
11. **Gitignore awareness** — before creating files that may contain secrets, verify `.gitignore` covers them. If not, add the pattern to `.gitignore` first.

---

## Testing Expectations

### What the LLM must do

1. **Every step that adds or modifies logic must include corresponding unit tests.** No code-only steps without tests unless the step is purely config/schema with no behavior.
2. Run `cd .devteam/engine && npm test` and `npm run build` after every change.
3. Report pass/fail counts in the COMPLETED block.

### Manual tests for the user

Some tests cannot be run by the LLM (e.g., they need real API keys, a running server, or browser interaction). When a step involves these, the LLM MUST include a **Manual Test** section at the end of its output:

```
=== MANUAL TESTS ===
Run these in your terminal to verify:

1. Start the dev server:
   cd .devteam/engine && npm run serve -- --port 3737

2. Test the fix command with a real error:
   node dist/index.js fix "TypeError: Cannot read property 'x' of undefined"

3. Verify webhook receives the event:
   curl -X POST http://localhost:3737/webhook -H "Content-Type: application/json" -d '{"event":"test"}'

Expected: [describe what success looks like]
```

The LLM must be specific — give exact commands, expected output, and what "pass" looks like. Do not leave the user guessing.

---

## Codebase Quick Reference

- Engine root: `.devteam/engine/`
- CLI entry point: `src/index.ts` (monolithic command router)
- Config schema: `src/config/schema.ts`
- Config defaults: `src/config/defaults.ts`
- LLM client: `src/llm/client.ts`
- Model aliases: `src/llm/modelRegistry.ts`
- Test pattern: `src/<module>/__tests__/<name>.test.ts`
- Config loader: `src/config/loader.ts`

---

## Wave Planning (Planner Role)

When asked to **plan a new wave**, the planner LLM must:

1. Read `PROMPT_RULES.md`, `.dev-plan/DEV_STATUS.md`, and existing wave folders.
2. Create `.dev-plan/wave-<N>/` folder.
3. Break scope into Features and Steps following the naming hierarchy.
4. Create `OVERVIEW.md` in the wave folder.
5. Create one `.md` file per feature with all step prompts.
6. Update `.dev-plan/PLAN_OVERVIEW.md` and `.dev-plan/DEV_STATUS.md`.

### PLAN_OVERVIEW.md

Single source of truth. Lives at `.dev-plan/PLAN_OVERVIEW.md`.

```markdown
# Plan Overview

## Completed Waves

  Wave 17 — VSCode extension
  Status: Done

## Active Wave

  Wave 18 — CLI power features
  Overview: [wave-18/OVERVIEW.md](wave-18/OVERVIEW.md)
  Features:
    01  [fix command](wave-18/18-F1-fix-command.md)           — 4 steps — In progress
    02  [pre-plan hooks](wave-18/18-F2-pre-plan-hooks.md)     — 3 steps — Not started
    03  [config profiles](wave-18/18-F3-config-profiles.md)   — 4 steps — Not started
    ...

## Upcoming Waves

  Wave 19 — [scope TBD] — not yet planned
```

---

## Daily Status Update

Say: **"Give me a dev status update."**

The LLM must:

1. Read `.dev-plan/DEV_STATUS.md`, `.dev-plan/PLAN_OVERVIEW.md`, and the active wave overview.
2. Run `cd .devteam/engine && npm test` and `npm run build`.
3. Check `git status` and `git log --oneline -5`.
4. Return:

```
=== DEV STATUS — 2026-03-07 ===

Health:
  Build: pass
  Tests: 1035 / 1035
  Uncommitted changes: yes — 3 files

Active Wave: 18
  Completed: 5 of 29 steps (18-F1-S1, 18-F1-S2, 18-F2-S1, 18-F2-S2, 18-F2-S3)
  Blocked: (none)
  Next up:
    - 18-F1-S3 with Claude (depends on S1+S2, both done)
    - 18-F3-S1 with Qwen (no dependencies, can start now)

Recommended actions:
  1. Run 18-F1-S3 with Claude
  2. Start 18-F3-S1 with Qwen in parallel
  3. Commit completed feature 18-F2
```

5. Update `.dev-plan/DEV_STATUS.md`.

### Source of truth

`PLAN_OVERVIEW.md` is the **canonical source** for feature status (not started / in progress / done). `DEV_STATUS.md` is a **derived snapshot** — it is regenerated from `PLAN_OVERVIEW.md` + live checks (tests, build, git) every time a status update runs.

If the two files conflict, `PLAN_OVERVIEW.md` wins. The LLM must fix `DEV_STATUS.md` to match, not the other way around.

### DEV_STATUS.md format

```markdown
# Development Status

Last updated: 2026-03-07

## Current State

  Build: pass
  Tests: 1035
  Active wave: 18
  Uncommitted features: 18-F2

## Wave 18 Progress

  18-F1 fix command        — 2/4 steps — In progress
  18-F2 pre-plan hooks     — 3/3 steps — Done (uncommitted)
  18-F3 config profiles    — 0/4 steps — Not started
  18-F4 test-fail loop     — 0/4 steps — Not started
  18-F5 review command     — 0/4 steps — Not started
  18-F6 prompt debug       — 0/2 steps — Not started
  18-F7 task decomposition — 0/5 steps — Not started

## Recent Completions

  2026-03-07: 18-F2-S1, 18-F2-S2, 18-F2-S3
  2026-03-06: 18-F1-S1, 18-F1-S2

## Known Blockers

  (none)
```

---

## Error Recovery Protocol

When a step causes test failures or build errors:

1. The LLM MUST attempt to fix the issue (up to 3 attempts).
2. If fixed, report in the COMPLETED block under Notes.
3. If not fixable, report BLOCKED with full error output.
4. NEVER delete or skip existing tests to make the suite pass.
5. Total test count must not decrease.

### Rollback Protocol

If a step's changes must be fully reverted (e.g., step causes unfixable regressions):

1. Run `git diff --stat` to identify all files changed by the step.
2. Revert only the files belonging to the failed step: `git checkout HEAD -- <file1> <file2> ...`.
3. Run tests and build to confirm the codebase is back to a clean state.
4. Report BLOCKED with the list of reverted files and the reason.
5. **For parallel steps**: if step A succeeded and parallel step B failed, only revert B's files. Never touch A's changes.
6. **Never run `git reset --hard`** — this discards all uncommitted work, including other steps.

---

## Conflict Resolution for Parallel Steps

Parallel steps MUST NOT modify the same file.

1. The planner must verify file targets do not overlap.
2. If two steps need the same file, make them sequential.
3. Exception: `package.json` `scripts.test` — both can append, but the second must verify the first's entries are still present.

---

## Cross-Step State Passing

When a step depends on a previous step completed by a different LLM, the receiving LLM needs context about what changed.

### Handoff rules

1. The COMPLETED block (see Completion Protocol) is the primary handoff artifact. It MUST include:
   - All files changed (full paths).
   - Any new exports, interfaces, types, or config keys introduced.
   - Any changed function signatures or behavior.
2. Before starting a dependent step, the LLM MUST:
   - Read the COMPLETED block of every dependency listed in `DEPENDS ON`.
   - Read (not skim) every file listed in the dependency's "Files changed".
   - If a dependency's COMPLETED block is missing, report BLOCKED.
3. The planner SHOULD write dependency notes in the step prompt when the handoff is non-obvious (e.g., "S1 adds `parseError()` in `src/cli/errorParser.ts` — use it here").

### Extended COMPLETED block for handoff steps

When a step has downstream dependents, extend the COMPLETED block:

```
=== STEP 18-F1-S1 COMPLETED ===
Files changed: src/config/schema.ts, src/config/defaults.ts
Tests: 1036 / 1036
Build: pass
Exports added: FixCommandConfig (interface), fixDefaults (const)
Signatures: parseError(raw: string): ParsedError
Notes: ParsedError type is in src/cli/types.ts
```

---

## Verification Step

After ALL steps in a feature are completed, run:

> **"Verify feature 18-F1."**

The LLM reports:

```
=== FEATURE 18-F1 VERIFIED ===
Build: pass
Tests: 1042 / 1042
Files reviewed: src/cli/errorParser.ts, src/cli/__tests__/errorParser.test.ts, ...
Issues found: none
```

Only after verification should the feature be committed.

---

## Issues to Consider Adding

1. **Context budget** — specify which sections of large files to read, not "read fully".
2. **Prompt versioning** — version number at top; wave files reference which version they use.
3. **Model fallback** — explicit fallback chain when recommended model is unavailable.
4. **Max file changes per step** — cap scope creep (e.g., 5 files for complexity 1–2).
