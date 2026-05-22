# BMAD Starter

A pre-bootstrapped [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD) install, checked into git so it can be used in environments where the official installer can't run (e.g. corporate proxies blocking `npx` / install scripts, air-gapped networks).

Instead of running the installer, you **clone this repo** and you're ready to go.

## Modules & versions

Snapshot last updated **2026-05-22** with BMAD installer **v6.7.1** (initial install: 2026-05-14 / v6.6.0).

| Module | Version | Source | What it does |
|--------|---------|--------|--------------|
| `core` | 6.7.1 | built-in | Cross-cutting skills: brainstorming, party mode, editorial reviews, adversarial review, doc indexing/sharding, customize, distillator, help |
| `bmm`  | 6.7.1 | built-in | BMad Method — the planning/build pipeline and agents (Analyst, PM, UX, Architect, Dev, Tech Writer) |
| `tea`  | v1.19.0 | [bmad-method-test-architecture-enterprise](https://github.com/bmad-code-org/bmad-method-test-architecture-enterprise) (npm: `bmad-method-test-architecture-enterprise`, sha `8734d51`) | Test Architect (Murat) — risk-based test strategy, ATDD, framework scaffolding, NFR/trace/test-design workflows |
| `automator` | `main` (channel `next`) | [bmad-automator](https://github.com/bmad-code-org/bmad-automator) (npm: `bmad-story-automator`, sha `3b01cfd`) | Story Automator — drives the create → dev → QA → review → retro loop autonomously. New official module name; the legacy `baut/` directory (v1.14.2) is still on disk from the previous install but the v6.7.x installer now ships this module under `automator/` |

Configured IDEs: `claude-code`, `codex`, `github-copilot`, `cursor`. GitHub Copilot agents are now generated into [`.github/agents/`](.github/agents/) (analyst, architect, dev, PM, tech-writer, UX designer, TEA).

### What's new since the previous snapshot (v6.6.0 → v6.7.1)

- **`bmad-prd`** — new unified PRD skill (create / update / validate intents). Supersedes `bmad-create-prd`, `bmad-edit-prd`, and `bmad-validate-prd`, which are now marked **deprecated** and slated for removal in v7 (aliases still resolve for now).
- **`bmad-investigate`** — new forensic investigation skill: evidence-graded case files for bug hunts, incident triage, or building a mental model of unfamiliar code before working on it.
- **`automator` module** — Story Automator pulled from the `next` channel with a refreshed runtime (new `runtime_layout` / `stop_hooks` modules, updated orchestration policy and tmux runtime).
- **TEA → v1.19.0** — adds the `confidence-gate` knowledge resource and refreshes the NFR / test-design / trace workflows.
- Skill catalog is up to **56 BMAD skills** (from 54).

Full manifest (install timestamps, SHAs, channels) lives in [`_bmad/_config/manifest.yaml`](_bmad/_config/manifest.yaml).

## What's in here

```
.
├── _bmad/                  # BMAD configuration + modules (core, bmm, tea, automator, baut*)
│   ├── config.toml         # project config (installer-managed)
│   ├── config.user.toml    # per-user config (installer-managed)
│   └── custom/             # your durable overrides (never touched by installer)
│       ├── config.toml         # team — committed
│       └── config.user.toml    # personal — gitignored
├── _bmad-output/           # BMAD output: planning / implementation / test artifacts
├── .claude/skills/         # 56 BMAD skills for Claude Code
├── .agents/skills/         # same skills for other agent clients (Codex, Cursor)
├── .github/agents/         # GitHub Copilot agent definitions
└── docs/                   # your project knowledge: PRDs, architecture, etc.
```

\* `baut/` is the legacy directory for the Story Automator module — see the modules table above.

## How to use

1. **Fork / push this repo** to your own GitHub (org or personal).
2. **Clone** it as the starting point for your project:
   ```bash
   git clone <your-repo-url> my-project
   cd my-project
   ```
3. **Customize the config** (see below).
4. (Optional) Wipe history for a fresh start:
   ```bash
   rm -rf .git && git init
   ```
5. **Start working** — in Claude Code, type `/bmad-help` to see available workflows.

## What to change

### `_bmad/config.toml` — project-level

| Field | What to set |
|-------|-------------|
| `project_name` | your project name (currently `"bmad-starter"`) |
| `document_output_language` | language for generated artifacts (PRD, architecture, stories) — currently `"English"` |

### `_bmad/config.user.toml` — per-user

| Field | What to set |
|-------|-------------|
| `user_name` | your name (currently `"Dev"`) |
| `communication_language` | language the agent should talk to you in (currently `"Polish"`) |
| `user_skill_level` | `beginner` / `intermediate` / `advanced` |

> ⚠️ Both files carry an *"installer-managed, regenerated on every install"* header. In this no-installer workflow, editing them directly is fine. If you ever re-run the installer, it will overwrite them — use `_bmad/custom/` (below) for changes that must survive a reinstall.

### `_bmad/custom/` — durable overrides

Use these when you want changes to survive a possible future reinstall:

- **`_bmad/custom/config.toml`** — team-wide overrides, committed to the repo. Example: tweak an agent's description:
  ```toml
  [agents.bmad-agent-pm]
  description = "Short, bulleted PRDs over prose."
  ```
- **`_bmad/custom/config.user.toml`** — your personal overrides, **gitignored**.

Tables deep-merge in order: base config → team custom → user custom (last wins).

## Communication language vs document language

Two separate settings, easy to confuse:

- `communication_language` (config.user.toml) — what the agent **talks to you** in.
- `document_output_language` (config.toml) — what the agent **writes artifacts** in (PRDs, architecture, stories).

A common setup: chat in your native language, write artifacts in English so the rest of the team can read them.

## .gitignore

This repo does **not** ship with a `.gitignore` yet. At minimum, before your first commit, add:

```gitignore
_bmad/custom/config.user.toml
```

The rest depends on the tech stack you drop into the project.

## Credit

BMAD Method itself: <https://github.com/bmad-code-org/BMAD-METHOD>. This repo is just a frozen snapshot of a fresh install — all credit for the method, skills, and agents goes there.
