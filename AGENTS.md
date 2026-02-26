---
last_validated: 2026-02-26T23:20:00Z+00:00
---

# Agent Instructions: nix-deadnix-bin

This file provides guidance to Claude Code when working with code in this repository.

## Repository Overview

Prebuilt [deadnix](https://github.com/astro/deadnix) binaries published as GitHub Releases. Cross-compiles for macOS (arm64, x86_64) and Linux (x86_64, arm64), installable via mise's `github:` backend. Builds from crates.io.

## Repository Structure

```
nix-deadnix-bin
├── .github
│   └── workflows
│       ├── build.yml
│       ├── check-upstream.yml
│       └── lint.yml
├── AGENTS.md
├── CHANGELOG.md
├── CLAUDE.md
├── lefthook
│   ├── commit-msg.yml
│   ├── files.yml
│   └── lint.yml
├── lefthook.yml
├── package.json
├── README.md
├── Taskfile.yml
├── taskfiles
│   ├── build.yml
│   ├── check-upstream.yml
│   ├── lint.yml
│   └── setup.yml
└── tool.json
```

## Key Files

### tool.json

```json
{
  "name": "deadnix",
  "crate": "deadnix",
  "upstream": "https://github.com/astro/deadnix",
  "description": "Scan nix files for dead code"
}
```

### GitHub Workflows

All workflows use `jdx/mise-action@v3` for tool installation and delegate logic to Taskfile tasks.

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `build.yml` | `workflow_dispatch` with version input | Orchestrates `task build:*` — cross-compile, package, release |
| `check-upstream.yml` | Weekly cron + `workflow_dispatch` | Runs `task check-upstream` — compare upstream vs ours, create issue |
| `lint.yml` | PR + push to main | Runs `task lint` — actionlint, yamllint, tool.json validation |

### Build Targets

| Target | Runner | Architecture |
|--------|--------|-------------|
| `aarch64-apple-darwin` | `macos-14` | macOS arm64 |
| `x86_64-unknown-linux-gnu` | `ubuntu-latest` | Linux x86_64 |
| `aarch64-unknown-linux-gnu` | `ubuntu-latest` (cross) | Linux arm64 |

## Dev Tooling

### mise (Tool Version Management)

| File | Purpose | Tools |
|------|---------|-------|
| `.mise.toml` | Base (always loaded) | task, actionlint, yamllint, jq |
| `.mise.development.toml` | Dev-only (local) | lefthook, node |
| `.mise.ci.toml` | CI profile (empty) | Inherits base only |
| `.miserc.toml` | Sets default env | `env = ["development"]` |

### Taskfile

```bash
task setup              # Install tools + hooks + npm deps
task lint               # Run actionlint + yamllint + tool.json validation
task build:compile      # Compile tool (reads source from tool.json)
task build:package      # Package binary as tarball + sha256 (needs TOOL_NAME, TARGET)
task build:release      # Create GitHub Release (needs GH_TOKEN, TOOL_NAME, VERSION)
task check-upstream     # Check for new upstream releases (needs GH_TOKEN, GITHUB_REPOSITORY)
```

### lefthook (Git Hooks)

| Hook | Checks |
|------|--------|
| `pre-commit` | Trailing whitespace, end-of-file newline, large files, merge conflicts, actionlint, yamllint, tool.json |
| `commit-msg` | Conventional commits via commitlint |

## Conventions

- Conventional commits enforced by commitlint
- YAML files linted with yamllint (200 char line limit, no document-start required)
- GitHub Actions workflows linted with actionlint
- Workflow logic in Taskfile tasks, not inline bash — workflows orchestrate only
