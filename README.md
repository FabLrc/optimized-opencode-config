# OpenCode Config — Inspired by Claude Code

## Why this config exists

Out-of-the-box OpenCode results are disappointing when you're coming from Claude Code. A bare model with no roles, no conventions, and no guardrails produces generic code, ignores project patterns, and requires constant hand-holding.

This configuration bridges that gap. It brings Claude Code's best practices into OpenCode: specialized agents, project-level commands, behavioral skills, and sensible permissions. The goal is for an agent to carry a task from start to finish — explore, plan, implement, review, commit — without manual intervention at every step.

---

## Structure

```
~/.config/opencode/
├── opencode.jsonc          # Global config: models, permissions, built-in agents
│
├── agents/
│   ├── feature-dev/        # Trio of agents for feature development
│   │   ├── code-explorer.md      — Codebase exploration and understanding
│   │   ├── code-architect.md     — Technical planning and design
│   │   └── code-reviewer.md      — Feature-focused code review
│   │
│   └── pr-review/          # Pull request review suite
│       ├── code-reviewer.md      — Guideline compliance and bug detection
│       ├── code-simplifier.md    — Code simplification and consistency
│       ├── comment-analyzer.md   — PR comment analysis
│       ├── pr-test-analyzer.md   — Test coverage assessment
│       ├── silent-failure-hunter.md — Silent error and swallowed exception detection
│       └── type-design-analyzer.md  — Type design quality review
│
├── commands/
│   ├── commit.md           # Conventional commit with generated message
│   ├── commit-push-pr.md   # Commit + push + GitHub PR creation
│   ├── clean-gone.md       # Delete local branches whose upstream is gone
│   ├── feature-dev.md      # Orchestrates the feature-dev trio in parallel
│   ├── code-review.md      # Local code review (4 agents in parallel)
│   └── review-pr.md        # Full PR review (6 agents in parallel)
│
└── skills/
    ├── security-guidance/      # 9 security rules to apply systematically
    ├── explanatory-output/     # Pedagogical mode: ★ Insight format
    ├── learning-output/        # Interactive learning mode with TDD
    ├── claude-opus-migration/  # Migration guide to Claude Opus 4.5
    └── frontend-design/        # Frontend design conventions and patterns
```

---

## Models

All agents run on **DeepSeek via OpenCode Go**, with reasoning at maximum budget (`thinking.budgetTokens: 32000`).

| Role | Model | Temperature |
|------|-------|-------------|
| Default | `deepseek-v4-flash` | — |
| Plan (planning agent) | `deepseek-v4-pro` | 0.1 |
| Build (implementation agent) | `deepseek-v4-flash` | 0.2 |
| Read-only agents | `deepseek-v4-flash` | — |
| Write-capable agents | `deepseek-v4-pro` | — |

Pro is used for planning and write-capable agents where precision matters; Flash handles exploration and read-only agents where speed is the priority.

---

## Permissions

The config minimizes interruptions for safe operations and requires confirmation for anything that mutates the environment.

- **File edits**: always allowed — that's the agent's job
- **Read-only bash** (`git log/diff/status`, `grep`, `find`, `ls`, `cat`): auto-approved
- **Bash with side effects**: requires confirmation
- **Webfetch**: requires confirmation

---

## Setup

### Vision (image support for DeepSeek)

DeepSeek models don't natively support images. The `opencode-vision` plugin bridges this by routing pasted images through a vision model ([nvidia/nemotron-nano-12b-v2-vl:free](https://openrouter.ai/nvidia/nemotron-nano-12b-v2-vl:free) via OpenRouter) before passing a text description to DeepSeek.

**1. Build and install the plugin**

`opencode-vision` is not on npm — it must be built from source and placed in `~/.config/opencode/plugin/`.

<details>
<summary><strong>Windows (PowerShell)</strong></summary>

```powershell
cd $env:TEMP
git clone https://github.com/DavidEasden/opencode-vision.git
cd opencode-vision
npm install
npm run build
New-Item -ItemType Directory -Force "$env:USERPROFILE\.config\opencode\plugin"
Copy-Item "dist\index.js" "$env:USERPROFILE\.config\opencode\plugin\opencode-vision.js"
```
</details>

<details>
<summary><strong>macOS / Linux</strong></summary>

```sh
cd /tmp
git clone https://github.com/DavidEasden/opencode-vision.git
cd opencode-v