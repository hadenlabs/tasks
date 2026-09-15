# Taskfile.yml Implementation Guide — Agent Specification

> **Audience**: AI agents implementing Taskfile.yml modules.
> **Determinism contract**: follow the template, apply rules R1–R15, then run the validation checklist. Output must pass all checks.

---

## 0. How to Use This Guide

An agent MUST follow these steps in order:

1. Copy the canonical template (Section 1) verbatim.
2. Apply hard rules R1–R15 (Section 2) — each rule is mandatory unless marked SHOULD.
3. Resolve decisions via the decision tables (Section 3).
4. Run the validation checklist (Section 4) — all items MUST pass.
5. Never violate blocker rules (Section 5).

---

## 1. Canonical Template

This is the ONLY source of truth. Agents copy it and substitute placeholders (`{{TOOL}}`, `{{DESC}}`, `{{INSTALL_CMD}}`, etc.). Every line is annotated with the rule ID it satisfies.

<!-- prettier-ignore-start -->
```yaml
# R1: --- + version "3"
---
version: "3"

# R12: Shared variables (versions, paths) declared here — modules reference via {{.VAR}}
env:
  GO_FILES:
    sh: find ./ -type f -name '*.go' | grep -v '/vendor/' | sort -u | xargs -n 100 true

vars:
  PROJECT_NAME: "{{PROJECT_NAME}}"
  GOLANG_VERSION: "{{GOLANG_VERSION}}"

tasks:
  # ── Default ─────────────────────────────────────────────────────────
  default:
    desc: List available tasks
    run: once
    cmds:
      - cmd: task --list
        silent: true

  # ── Check pattern: two-level (R4) ──────────────────────────────────
  check:
    desc: Check all dependencies
    run: once
    deps:
      - task: check:{{TOOL}}

  # R5: check:<tool> uses sh: command -v <tool> + msg:
  check:{{TOOL}}:
    desc: Check if {{TOOL}} is installed
    run: once
    preconditions:
      - sh: command -v {{TOOL}}
        msg: "{{TOOL}} is not installed. {{INSTALL_CMD}}"

  # R6: Env var checks use sh: test -v VAR (never bare test {{.VAR}})
  check:vars:
    desc: Check required environment variables
    run: once
    preconditions:
      - sh: test -v REQUIRED_VAR
        msg: "Set environment variable REQUIRED_VAR"

  # ── Setup (R13: deps on checks) ────────────────────────────────────
  setup:
    desc: Setup dependencies
    run: once
    cmds:
      - {{SETUP_CMD}}
    deps:
      - task: check:{{TOOL}}

  # R10: Private helper — _ prefix
  _setup:
    desc: Internal helper to install {{TOOL}}
    run: once
    cmds:
      - cmd: {{INSTALL_CMD}}
        silent: true
    deps:
      - task: check:{{PREREQUISITE}}

  # R7+R8: fmt/fix/lint declare checks in deps, CLI_ARGS at end of command
  fmt:
    desc: Format source files
    run: once
    cmds:
      - cmd: >-
          {{FORMAT_CMD}} {{.CLI_ARGS}}
    deps:
      - task: check:{{FORMATTER}}

  fix:
    desc: Fix lint issues automatically
    run: once
    cmds:
      - cmd: >-
          {{FIX_CMD}} {{.CLI_ARGS}}
    deps:
      - task: check:{{LINTER}}

  lint:
    desc: Run linter
    run: once
    cmds:
      - cmd: >-
          {{LINT_CMD}} {{.CLI_ARGS}}
    deps:
      - task: check:{{LINTER}}

  # ── Test / Build ────────────────────────────────────────────────────
  test:
    desc: Run all tests
    run: once
    cmds:
      - {{TEST_CMD}}
    deps:
      - task: check:{{TEST_TOOL}}

  build:
    desc: Build binaries
    run: once
    cmds:
      - >-
        {{BUILD_CMD}}
    deps:
      - task: check:{{BUILD_TOOL}}

  # R9: | for bash scripts with logic/conditionals
  feedback:
    desc: Show build feedback
    run: once
    cmds:
      - |
        RUN_ID=$(curl -s {{API_URL}} | jq -r '.runs[0].id')
        test -n "$RUN_ID" || { echo "No runs found"; exit 1; }
        echo "Latest run: $RUN_ID"
    deps:
      - task: check:vars
```
<!-- prettier-ignore-end -->

---

## 2. Hard Rules

### R1: File starts with `---` then `version: "3"`

- **Normative**: MUST
- **Applies to**: top of every Taskfile.yml
- **Rationale**: YAML document marker + go-task schema version. Non-negotiable.
- **Example**:
  ```yaml
  ---
  version: "3"
  ```
- **Counter-example**:
  ```yaml
  version: "3"
  # missing --- on line 1
  ```

### R2: Every public task has `desc:`

- **Normative**: MUST
- **Applies to**: every task not prefixed with `_` and not using `internal: true`
- **Rationale**: Without `desc:`, the task is invisible in `task --list`.
- **Example**:
  ```yaml
  fmt:
    desc: Format source files
  ```
- **Counter-example**:
  ```yaml
  fmt:
    cmds:
      - gofmt -s -l -w .
  ```

### R3: Every task has `run: once` except `default`

- **Normative**: MUST
- **Applies to**: all tasks except `default`
- **Rationale**: Prevents re-execution in the same run. `default` is exempt because it orchestrates sub-tasks.
- **Example**:
  ```yaml
  fmt:
    desc: Format source files
    run: once
  ```
- **Counter-example**:
  ```yaml
  fmt:
    run: always
  ```

### R4: Two-level check pattern

- **Normative**: MUST
- **Applies to**: `check:` task and `check:<tool>` tasks
- **Rationale**: Aggregator `check` depends on individual `check:<tool>` tasks via `deps`. Never mix levels.
- **Example**:

  ```yaml
  check:
    desc: Check all dependencies
    run: once
    deps:
      - task: check:go
      - task: check:gofmt

  check:go:
    desc: Check if Go is installed
    run: once
    preconditions:
      - sh: command -v go
        msg: "Go is not installed"
  ```

- **Counter-example**:
  ```yaml
  check:
    desc: Check all
    run: once
    preconditions:
      - sh: command -v go
  ```

### R5: Each `check:<tool>` uses `sh: command -v <tool>` + `msg:`

- **Normative**: MUST
- **Applies to**: every `check:<tool>` task
- **Rationale**: `command -v` is POSIX-portable. The `msg:` MUST be actionable (include install command).
- **Example**:
  ```yaml
  check:go:
    desc: Check if Go is installed
    run: once
    preconditions:
      - sh: command -v go
        msg: "Go is not installed. Install from https://go.dev/dl/"
  ```
- **Counter-example**:
  ```yaml
  check:go:
    preconditions:
      - sh: which go
        msg: "Install go"
  ```

### R6: Env var checks use `sh: test -v VAR`

- **Normative**: MUST
- **Applies to**: `check:vars` and any task checking environment variables
- **Rationale**: Bare `test {{.VAR}} == ...` fails when the variable is unset. `test -v` checks existence safely.
- **Example**:
  ```yaml
  preconditions:
    - sh: test -v CI_REGISTRY_USER
      msg: "Set CI_REGISTRY_USER"
  ```
- **Counter-example**:
  ```yaml
  preconditions:
    - sh: test "{{.CI_REGISTRY_USER}}" == ""
  ```

### R7: Work tasks (fmt/fix/lint) declare their checks in `deps`

- **Normative**: MUST
- **Applies to**: `fmt`, `fix`, `lint` tasks
- **Rationale**: Ensures tools exist before execution. Use `deps` with `task: check:<tool>`.
- **Example**:
  ```yaml
  fmt:
    desc: Format source files
    run: once
    cmds:
      - cmd: >-
          gofmt -s -l -w . {{.CLI_ARGS}}
    deps:
      - task: check:gofmt
  ```
- **Counter-example**:
  ```yaml
  fmt:
    cmds:
      - gofmt -s -l -w .
  ```

### R8: `{{.CLI_ARGS}}` only in fmt/fix/lint, at the END of the command

- **Normative**: MUST
- **Applies to**: `fmt`, `fix`, `lint` commands
- **Rationale**: Gives users a way to pass extra flags. Never in test/build. Always last argument.
- **Example**:
  ```yaml
  cmds:
    - cmd: >-
        ruff format ./ {{.CLI_ARGS}}
  ```
- **Counter-example**:
  ```yaml
  cmds:
    - cmd: >-
        ruff format {{.CLI_ARGS}} ./
  ```

### R9: `>-` for long single-logical-line commands; `|` for multi-line bash with logic

- **Normative**: MUST
- **Applies to**: all `cmds:` entries
- **Rationale**: `>-` folds lines into one command (replaces newlines with spaces). `|` preserves newlines for scripts with variables, conditionals, pipes, or subshells.
- **Example** (`>-`):
  ```yaml
  cmds:
    - >-
      docker build
      --no-cache
      --platform=linux/amd64
      --file Dockerfile
  ```
- **Example** (`|`):
  ```yaml
  cmds:
    - |
      RUN_ID=$(curl -s "$URL" | jq -r '.id')
      test -n "$RUN_ID" || exit 1
  ```
- **Counter-example**:
  ```yaml
  cmds:
    - docker build \
      --no-cache \
      --platform=linux/amd64
  ```

### R10: Private helper tasks use `_` prefix or `internal: true`

- **Normative**: MUST
- **Applies to**: any task not intended for direct user invocation
- **Rationale**: Keeps `task --list` clean. Use `_` prefix convention (e.g., `_setup`).
- **Example**:
  ```yaml
  _setup:
    desc: Internal helper
    run: once
    cmds:
      - curl -LsSf https://example.com/install.sh | sh
  ```
- **Counter-example**:
  ```yaml
  setup_helper:
    desc: Internal helper
    cmds:
      - curl -LsSf https://example.com/install.sh | sh
  ```

### R11: `silent: true` only on pure-output tasks or `cmd:` wrappers (SHOULD)

- **Normative**: SHOULD
- **Applies to**: `default`, `summary`, `help`, or individual `cmd:` lines
- **Rationale**: `silent: true` at task level silences ALL commands. Use `cmd:` + `silent: true` on specific noisy lines.
- **Example**:
  ```yaml
  default:
    cmds:
      - cmd: echo "{{.PROJECT_NAME}}"
        silent: true
  ```
- **Counter-example**:
  ```yaml
  fmt:
    silent: true
    cmds:
      - gofmt -s -l -w .
  ```

### R12: Shared variables (versions, paths) declared in root Taskfile vars, not in modules

- **Normative**: MUST
- **Applies to**: module Taskfile.yml files
- **Rationale**: Single source of truth for versions. Modules reference `{{.VARIABLE_NAME}}`.
- **Example**: Module uses `{{.GOLANG_VERSION}}` — root defines it.
- **Counter-example**:
  ```yaml
  # In src/go/Taskfile.yml
  vars:
    GOLANG_VERSION: "1.26"
  ```

### R13: `setup` task declares `deps` on its checks

- **Normative**: MUST
- **Applies to**: `setup` task
- **Rationale**: Ensures prerequisites exist before installation commands run.
- **Example**:
  ```yaml
  setup:
    desc: Setup dependencies
    run: once
    cmds:
      - uv sync --all-groups
    deps:
      - task: check:python
      - task: _setup
  ```
- **Counter-example**:
  ```yaml
  setup:
    cmds:
      - uv sync --all-groups
  ```

### R14: Never invent tool subcommands

- **Normative**: MUST NOT
- **Applies to**: all `cmds:` entries
- **Rationale**: `uv run command -v tool` is not a valid uv subcommand. Use `command -v tool` or `uvx`.
- **Example**:
  ```yaml
  preconditions:
    - sh: command -v ruff
  ```
- **Counter-example**:
  ```yaml
  preconditions:
    - sh: uv run command -v ruff
  ```

### R15: Never duplicate checks across modules

- **Normative**: MUST NOT
- **Applies to**: module Taskfile.yml files
- **Rationale**: Each module declares only the tools IT needs. The root aggregates; modules don't re-check everything.
- **Example**: `src/go/Taskfile.yml` declares `check:go` only.
- **Counter-example**: `src/go/Taskfile.yml` re-declares `check:docker` from `src/docker/`.

---

## 3. Decision Tables

### Task Attribute Decisions

| Decision         | Condition                                                    | Action                               |
| ---------------- | ------------------------------------------------------------ | ------------------------------------ |
| `run: once`      | Task is idempotent or is check/setup/fmt/fix/lint/test/build | Add `run: once`                      |
| `silent: true`   | Task output IS the result (help, summary, default)           | Add `silent: true`                   |
| `deps`           | Task requires tools/vars present                             | List `task: check:<tool>`            |
| `internal: true` | Helper not for direct use                                    | Add `internal: true` (or `_` prefix) |
| `{{.CLI_ARGS}}`  | Task is fmt/fix/lint                                         | Append at end of command             |
| `>-`             | Single logical command, long                                 | Use folding block                    |
| `\|`             | Bash script with logic/conditionals                          | Use literal block                    |
| `preconditions`  | Task is `check:<tool>`                                       | Use `sh: command -v <tool>` + `msg:` |

### Task Type → Required Attributes

| Task Type            | Required Attributes                                                               | Optional Attributes    |
| -------------------- | --------------------------------------------------------------------------------- | ---------------------- |
| `check` (aggregator) | `desc`, `run: once`, `deps` (list of `check:<tool>`)                              | —                      |
| `check:<tool>`       | `desc`, `run: once`, `preconditions` with `sh: command -v` + `msg:`               | —                      |
| `setup`              | `desc`, `run: once`, `cmds`, `deps` (checks)                                      | —                      |
| `fmt`                | `desc`, `run: once`, `cmds` with `{{.CLI_ARGS}}` at end, `deps` (formatter check) | —                      |
| `fix`                | `desc`, `run: once`, `cmds` with `{{.CLI_ARGS}}` at end, `deps` (linter check)    | —                      |
| `lint`               | `desc`, `run: once`, `cmds` with `{{.CLI_ARGS}}` at end, `deps` (linter check)    | —                      |
| `test`               | `desc`, `run: once`, `cmds`, `deps` (test tool check)                             | `{{.CLI_ARGS}}`        |
| `build`              | `desc`, `run: once`, `cmds`, `deps` (build tool check)                            | —                      |
| `default`            | `cmds`, `deps`                                                                    | `desc`, `silent: true` |
| Private (`_*`)       | `desc`, `run: once`, `cmds`                                                       | `deps`                 |

---

## 4. Validation Checklist

Run each check after implementing a module. ALL items MUST pass.

| #   | Check                           | Command                                                                       | Expected Result                 |
| --- | ------------------------------- | ----------------------------------------------------------------------------- | ------------------------------- | ----------------------------------------- | --------------------------- |
| 1   | YAML valid                      | `python3 -c "import yaml; yaml.safe_load(open('src/<module>/Taskfile.yml'))"` | No error                        |
| 2   | Public tasks listed             | `task -t src/<module>/Taskfile.yml --list`                                    | All public tasks show `desc:`   |
| 3   | Check passes                    | `task -t src/<module>/Taskfile.yml check`                                     | Passes (all tools installed)    |
| 4   | Markdown clean                  | `bunx prettier --check docs/guides/taskfile.md`                               | Clean                           |
| 5   | No `run: always`                | `grep -c 'run: always' src/<module>/Taskfile.yml`                             | `0`                             |
| 6   | No bare `test {{.`              | `grep -c 'test {{\.' src/<module>/Taskfile.yml`                               | `0`                             |
| 7   | Every public task has desc      | `task -t src/<module>/Taskfile.yml --list \| grep -v desc`                    | No output (all lines have desc) |
| 8   | No invented subcommands         | `grep -c 'uv run command' src/<module>/Taskfile.yml`                          | `0`                             |
| 9   | No hardcoded versions in module | `grep -cE 'GOLANG_VERSION:                                                    | PYTHON_VERSION:                 | NODE_VERSION:' src/<module>/Taskfile.yml` | `0` (versions in root only) |
| 10  | File starts with `---`          | `head -1 src/<module>/Taskfile.yml`                                           | `---`                           |

### Definition of Done

A module is complete when:

1. ALL MUST rules (R1–R15) are satisfied.
2. ALL validation checklist items (1–10) pass.
3. The module registers in root `includes:` (Section 6).
4. `task -t src/<module>/Taskfile.yml --list` shows all expected public tasks.

---

## 5. Blocker Rules (MUST NOT)

These are absolute prohibitions. Violating any one means the implementation is invalid.

1. **Inconsistent `---` marker** — every Taskfile.yml MUST start with `---` on line 1.
2. **Unguarded `test {{.VAR}}** — MUST use `test -v VAR` (R6). Never `test "{{.VAR}}" == ""`.
3. **Invented subcommands** — MUST NOT use `uv run command -v tool` or other non-existent subcommands (R14).
4. **Duplicated checks** — MUST NOT re-declare checks from other modules (R15).
5. **Missing `desc:`** — public tasks MUST have `desc:` (R2).
6. **Mixed silent levels** — MUST NOT use `silent: true` on task level for work tasks (R11).
7. **`run: always`** — MUST NOT use `run: always` (R3). Use `run: once`.
8. **Hardcoded versions in modules** — MUST NOT declare tool versions in module Taskfiles (R12).

---

## 6. Root Integration

Register the module in the root `Taskfile.yml` `includes:` block. Use namespace: path format.

```yaml
includes:
  changelog: ./src/changelog/Taskfile.yml
  docker: ./src/docker/Taskfile.yml
  go: ./src/go/Taskfile.yml
  terraform: ./src/terraform/Taskfile.yml
  uv: ./src/uv/Taskfile.yml
```

Tasks from included modules are invoked as `namespace:task` (e.g., `docker:build`, `uv:fmt`).

Root-level shared declarations:

```yaml
env:
  DOCKER:
    sh: docker --version 2> /dev/null || echo "not exist"
  GO:
    sh: go version 2> /dev/null || echo "not exist"

dotenv:
  - .env

vars:
  PROJECT_NAME: myproject
  GOLANG_VERSION: "1.26"
  PYTHON_VERSION: "3.11.5"
  NODE_VERSION: "v24.18.1"
```

---

## 7. References

- [Taskfile documentation](https://taskfile.dev/)
- [go-task source code](https://github.com/go-task/task)
- [hadenlabs/tasks repository](https://github.com/hadenlabs/tasks)
- [docs/tasks/](/docs/tasks/) — module-specific documentation
- [docs/usage.md](/docs/usage.md) — project usage overview
