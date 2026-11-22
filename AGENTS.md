# Agent Notes (tasks repo)

This repository is a collection of reusable `Taskfile.yml` templates plus project automation. Agents should prefer using `task` (go-task) and `uv` for day-to-day workflows.

## Quick Start

- Install toolchain: `task setup`
- Run all checks like CI: `task validate`
- List tasks: `task --list` (or `task --list-all`)

### Tool Versions

- Python: `3.11.5` (see `.python-version`)
- Node: `v24.11.1` (see `.nvmrc` / `.pre-commit-config.yaml`)

### Common Environment

- `TASK_X_REMOTE_TASKFILES=1`: enables go-task remote includes (used by CI workflows)
- `SONAR_URL` / `SONAR_TOKEN`: required for `task sonar:scan`

## Build / Lint / Test Commands

### Task Runner (preferred)

- Setup dependencies: `task setup` (creates `.env` from `.env.example` if missing; also runs `task git:setup` which updates `.gitignore` and sets `git config github.reviews`)
- Python deps only (no git config changes): `task uv:setup`
- Format everything (Prettier + Python formatting): `task prettier`
- Run CI-style validations (pre-commit on all files): `task validate`
- Repo dependency checks: `task check`

### Pre-commit (what CI runs)

- Run all hooks: `uv run pre-commit run --all-files`
- Run one hook: `uv run pre-commit run <hook-id> --all-files`
- Run one hook on specific files: `uv run pre-commit run <hook-id> --files path/to/file`
- Run hooks on changed files only: `uv run pre-commit run`
- Install hooks locally: `task uv:precommit`

See `.pre-commit-config.yaml` for the authoritative list of hooks (codespell, commitlint, shellcheck, gitleaks, hadolint, markdown-link-check, ...).

### Python (uv)

Python is managed with `uv` (see `pyproject.toml`, `.python-version`, `Taskfile.yml`).

- Install Python deps: `task uv:setup` (runs `uv sync --all-groups`)
- Format Python: `task uv:fmt` (or `uv run ruff format ./`)
- Lint + types: `task uv:lint` (ruff check + mypy)
- Typecheck only: `task uv:mypy`

Run a single test (or subset) directly with pytest:

- One file: `uv run pytest tests/test_file.py -q`
- One test: `uv run pytest tests/test_file.py::test_name -q`
- By keyword: `uv run pytest -k "keyword" -q`
- Stop on first failure: `uv run pytest -x`
- Run a single parameterized case: `uv run pytest tests/test_file.py -k "case_name" -q`

Note: the repo currently contains little/no first-party Python code; these commands mainly support tooling and template validation.

### Go (only if/when applicable)

Some templates define Go tasks (see `src/go/Taskfile.yml`). This repo's default branch does not ship a `go.mod`, so these apply when you add/include a Go module.

- Format: `task go:fmt`
- Lint: `task go:lint`
- Test: `task go:test`
- Build: `task go:build`

Run a single Go test without Taskfile:

- One package: `go test ./path/to/pkg -run TestName -count=1`
- One file's package tests (from that package dir): `go test -run TestName -count=1`
- Subtest only: `go test ./path/to/pkg -run TestName/Subtest -count=1`

### Docs

- Build docs: `task docs:build`
- Serve docs: `task docs:serve`

### Changelog / Release

- Generate full changelog: `task changelog:generate`
- Generate next-tag changelog: `task changelog:next APP_TAG=0.1.0`
- Generate tag-only notes (used by release workflow): `task changelog:tag APP_TAG=0.1.0`

## Code Style Guidelines

### General

- Respect `.editorconfig` (LF, final newline, trim whitespace; 2-space indents by default, 4 for Python, tabs for Makefiles).
- Prefer small, composable tasks over monolithic scripts.
- Avoid adding new build systems; integrate via `Taskfile.yml` + pre-commit when possible.

### Taskfile.yml (go-task)

- Use `version: "3"` and start files with `---` when other Taskfiles do.
- Naming: `namespace:verb` (examples in root `Taskfile.yml`: `uv:setup`, `docs:serve`).
- Add `desc:` for anything user-facing; keep descriptions action-oriented.
- Use `preconditions:` for required tools/vars (pattern used across `src/*/Taskfile.yml`).
- Prefer `{{.CLI_ARGS}}` passthrough for tasks meant to be extended from the CLI.
- Keep YAML simple: 2-space indents, quote strings that contain `:` or special chars.

### Python

- Formatting: `ruff format` (configured in `pyproject.toml`, line length 90).
- Linting: `ruff check` with isort rules enabled (import sorting is enforced by Ruff).
- Types: `mypy` is strict (`disallow_untyped_defs = true`, `strict = true`). Add type annotations for new Python code; keep public APIs typed.
- Imports: group stdlib / third-party / local; avoid unused imports (`pyright` and Ruff report them).
- Naming: follow PEP 8 naming (ruff includes `N` / pep8-naming); use `snake_case` for functions/vars, `CapWords` for classes, `UPPER_SNAKE_CASE` for constants.
- Error handling: raise specific exceptions; avoid `except Exception: pass`; use `contextlib`/`pathlib` helpers over manual try/finally where it improves clarity.

### JavaScript / TypeScript (when present)

- ESLint config lives at `.ci/linters/.eslintrc.js` (no semicolons, double quotes).
- Avoid `any` (warn); unused args must be prefixed with `_`.
- Promises: no floating promises; use `await`/`return` intentionally.
- Equality/blocks: `eqeqeq` and `curly` are enforced.

### Shell / CLI scripts

- ShellCheck runs in pre-commit (`.pre-commit-config.yaml`).
- Prefer POSIX-ish shell for portability; quote variables; use `set -euo pipefail` in new scripts unless there is a strong reason not to.

### Markdown / Docs

- Prettier formats `*.md` (see `.ci/linters/prettier.config.cjs`).
- Don’t add brittle external links in docs unless necessary; `markdown-link-check` runs in pre-commit.

### Git / Commits

- Commit messages must follow Conventional Commits; commitlint runs via pre-commit (config: `.ci/linters/.commitlintrc.json`).
- Optional helper config: `.goji.json` defines allowed commit types/scopes.
- Prefer non-interactive, reproducible commands in tasks; CI expects `task ...` to work without prompting.

## Repo-Specific Rules / Gotchas

- `README.md` is generated; follow the banner at the top of `README.md` (edit `provision/generators/README.yaml`, then run `task readme`).
- `.env` is loaded by Taskfile (`dotenv:` in `Taskfile.yml`). Treat `.env` as secret; do not commit it.
- CI linting is primarily `pre-commit`; if you change formatting/lint configs, run `task validate` before opening a PR.
- Secrets scanning: `gitleaks` runs in pre-commit (config: `.ci/linters/.gitleaks.toml`).
- Commit messages are enforced via commitlint (config: `.ci/linters/.commitlintrc.json`); Conventional Commits are expected (see `docs/contributing.md`).

## Agent Skills

- Local agent skill prompts live in `.claude/skills/` (e.g. `.claude/skills/release/`).

## Further Reading (docs/)

- Commands catalog: `docs/commands.md`
- Testing notes: `docs/testing.md`
- Contributing + commit conventions: `docs/contributing.md`
- Environment variables: `docs/env-vars.md`
- Release process: `docs/releasing.md`
- Usage overview: `docs/usage.md`
- Troubleshooting: `docs/troubleshooting.md`

## Cursor / Copilot Rules

- No `.cursor/rules/`, `.cursorrules`, or `.github/copilot-instructions.md` were found in this repository at the time this file was created.