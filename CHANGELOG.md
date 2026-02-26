# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Conventional Commits](https://www.conventionalcommits.org/).

## [Unreleased]

### Added

- mise tool management with 3-tier profiles: base, development, CI
- Taskfile with `setup`, `lint`, `build:*`, and `check-upstream` tasks
- lefthook git hooks: file hygiene, YAML linting, commitlint
- CLAUDE.md and AGENTS.md project documentation
### Changed

- Workflows now use `jdx/mise-action@v3` + `task` as thin orchestration wrappers
- Extracted all inline bash from workflows into Taskfile tasks
- Lint workflow now uses actionlint + yamllint via mise instead of Python YAML parser
