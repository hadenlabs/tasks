# nix-taskfile Specification

## Purpose

Provide a reusable Nix Taskfile module so consumers can check, format, lint, fix, set up, run flake operations, install/reinstall Nix tooling, integrate direnv, and garbage-collect through the repository's standardized task contract.

## Requirements

### Requirement: Nix availability check

The system SHALL verify that the `nix` command is available before running Nix tasks, displaying an actionable install hint when it is missing.

#### Scenario: Nix is installed

- **WHEN** user runs `task nix:check:nix`
- **THEN** system verifies `nix` exists and exits successfully

#### Scenario: Nix is not installed

- **WHEN** user runs `task nix:check:nix` and nix is not installed
- **THEN** system displays an actionable message including an install hint (https://nixos.org/download or DeterminateSystems installer) and exits with error

### Requirement: Check aggregator

The system SHALL provide a `check` task that aggregates the Nix availability check only.

#### Scenario: All checks pass

- **WHEN** user runs `task nix:check`
- **THEN** system runs `check:nix` and exits successfully

#### Scenario: Non-flake project keeps passing

- **WHEN** user runs `task nix:check` in a project without `flake.nix`
- **THEN** system still passes because `check:flake` is not part of the aggregator

### Requirement: Format Nix files

The system SHALL format Nix files using `nix fmt` when `flake.nix` exists, and `nixfmt` otherwise, accepting extra flags via `{{.CLI_ARGS}}` at the end of each command branch.

#### Scenario: Flake project format

- **WHEN** user runs `task nix:fmt` in a directory containing `flake.nix`
- **THEN** system runs `nix fmt {{.CLI_ARGS}}`

#### Scenario: Non-flake project format

- **WHEN** user runs `task nix:fmt` in a directory without `flake.nix`
- **THEN** system runs `nixfmt . {{.CLI_ARGS}}`

#### Scenario: Format with extra flags

- **WHEN** user runs `task nix:fmt -- <flags>`
- **THEN** system appends the flags to the end of the chosen command

### Requirement: Lint Nix files

The system SHALL lint Nix files using `statix check` and accept extra flags via `{{.CLI_ARGS}}` at the end of the command.

#### Scenario: Lint with default arguments

- **WHEN** user runs `task nix:lint`
- **THEN** system runs `statix check` against the project files

#### Scenario: Lint with extra flags

- **WHEN** user runs `task nix:lint -- <flags>`
- **THEN** system appends the flags to the end of the statix check command

### Requirement: Fix Nix lint issues

The system SHALL fix Nix lint issues using `statix fix` and accept extra flags via `{{.CLI_ARGS}}` at the end of the command.

#### Scenario: Fix with default arguments

- **WHEN** user runs `task nix:fix`
- **THEN** system runs `statix fix` against the project files

#### Scenario: Fix with extra flags

- **WHEN** user runs `task nix:fix -- <flags>`
- **THEN** system appends the flags to the end of the statix fix command

### Requirement: Work tasks declare their checks

The system SHALL declare `check:nix` as a dependency of `fmt`, `fix`, and `lint` tasks.

#### Scenario: Tool missing prevents execution

- **WHEN** user runs `task nix:fmt` and nix is not installed
- **THEN** system fails the `check:nix` dependency before executing fmt

### Requirement: Setup Nix tooling

The system SHALL provide a `setup` task that depends on the Nix availability check and the private setup helper.

#### Scenario: Setup runs checks first

- **WHEN** user runs `task nix:setup`
- **THEN** system runs `check:nix` and `_setup` before the setup commands

### Requirement: Private setup helper

The system SHALL provide a private `_setup` helper task with the `_` prefix that is not listed in `task --list`.

#### Scenario: Helper is hidden from list

- **WHEN** user runs `task -t src/nix/Taskfile.yml --list`
- **THEN** `_setup` does not appear in the public task list

### Requirement: Default lists tasks

The system SHALL provide a `default` task that lists all public tasks and is exempt from `run: once`.

#### Scenario: Default lists tasks

- **WHEN** user runs `task -t src/nix/Taskfile.yml`
- **THEN** system lists all public tasks with descriptions

### Requirement: Module registration in root Taskfile

The system SHALL register the Nix module in the root `Taskfile.yml` includes block as `nix: ./src/nix/Taskfile.yml`.

#### Scenario: Nix namespace available

- **WHEN** user runs `task nix:check`
- **THEN** system resolves to the Nix module tasks

### Requirement: No invented subcommands

The system SHALL use `command -v nix` directly in preconditions and SHALL NOT use invented subcommands such as `uv run command -v`.

#### Scenario: Preconditions use command -v

- **WHEN** inspecting `src/nix/Taskfile.yml`
- **THEN** preconditions use `sh: command -v nix` directly

### Requirement: Flake availability check

The system SHALL provide `check:flake` that verifies `flake.nix` exists in the project root, with an actionable message when it is missing.

#### Scenario: Flake present

- **WHEN** user runs `task nix:check:flake` in a directory containing `flake.nix`
- **THEN** system exits successfully

#### Scenario: Flake missing

- **WHEN** user runs `task nix:check:flake` in a directory without `flake.nix`
- **THEN** system displays an actionable message (e.g. "No flake.nix found in the project root. Initialize with: nix flake init") and exits with error

### Requirement: Flake check task

The system SHALL provide `test` that runs `nix flake check`, depending on `check:nix` and `check:flake`.

#### Scenario: Run flake check

- **WHEN** user runs `task nix:test`
- **THEN** system runs `nix flake check`

### Requirement: Flake show task

The system SHALL provide `show` that runs `nix flake show`, depending on `check:nix` and `check:flake`.

#### Scenario: Show flake

- **WHEN** user runs `task nix:show`
- **THEN** system runs `nix flake show`

### Requirement: Flake lock task

The system SHALL provide `lock` that runs `nix flake lock`, depending on `check:nix` and `check:flake`.

#### Scenario: Lock flake

- **WHEN** user runs `task nix:lock`
- **THEN** system runs `nix flake lock`

### Requirement: Flake update task

The system SHALL provide `update` that runs `nix flake update`, depending on `check:nix` and `check:flake`.

#### Scenario: Update flake

- **WHEN** user runs `task nix:update`
- **THEN** system runs `nix flake update`

### Requirement: Flake develop task

The system SHALL provide `develop` that runs `nix develop`, depending on `check:nix` and `check:flake`.

#### Scenario: Enter dev shell

- **WHEN** user runs `task nix:develop`
- **THEN** system runs `nix develop`

### Requirement: Flake run task

The system SHALL provide `run` that runs `nix develop --command {{.CMD}}`, depending on `check:nix` and `check:flake`, and requires the `CMD` template variable to be non-empty (verified via `sh: test -n "{{.CMD}}"`).

#### Scenario: Run command in dev shell

- **WHEN** user runs `task nix:run CMD="task test"`
- **THEN** system runs `nix develop --command task test`

#### Scenario: Missing CMD

- **WHEN** user runs `task nix:run` without CMD
- **THEN** system displays an actionable message and exits with error

#### Scenario: CMD missing → actionable msg; CMD provided → command executes in dev shell

- **WHEN** user runs `task nix:run` without CMD
- **THEN** system fails the precondition with the actionable "Pass the command to run" message
- **WHEN** user runs `task nix:run CMD="echo hola"`
- **THEN** the precondition passes and the command executes in the dev shell

### Requirement: Direnv availability check

The system SHALL provide `check:direnv` that verifies `direnv` is installed, with an actionable install hint.

#### Scenario: direnv installed

- **WHEN** user runs `task nix:check:direnv` and direnv is installed
- **THEN** system exits successfully

#### Scenario: direnv missing

- **WHEN** user runs `task nix:check:direnv` and direnv is missing
- **THEN** system displays an actionable install hint (https://direnv.net/) and exits with error

### Requirement: Direnv allow task

The system SHALL provide `direnv` that runs `direnv allow`, depending on `check:direnv`, `check:nix`, and `check:flake`.

#### Scenario: Allow direnv

- **WHEN** user runs `task nix:direnv`
- **THEN** system runs `direnv allow`

### Requirement: Nix install task

The system SHALL provide `install` that ensures Nix is present: if `nix` is already installed it prints a message and does nothing; otherwise it requires `curl` and runs the DeterminateSystems installer. It SHALL NOT depend on `check:nix` (must run when Nix is missing).

#### Scenario: Nix already installed

- **WHEN** user runs `task nix:install` and nix is present
- **THEN** system prints "nix is already installed" and exits successfully

#### Scenario: Nix missing with curl available

- **WHEN** user runs `task nix:install` and nix is missing and curl is available
- **THEN** system runs the DeterminateSystems installer

#### Scenario: Nix missing without curl

- **WHEN** user runs `task nix:install` and nix is missing and curl is missing
- **THEN** system prints an actionable error and exits with error

### Requirement: Nix reinstall task

The system SHALL provide `reinstall` that removes and reinstalls `nixfmt` and `statix` in the nix profile, depending on `check:nix`.

#### Scenario: Reinstall tooling

- **WHEN** user runs `task nix:reinstall`
- **THEN** system runs `nix profile remove nixpkgs#nixfmt nixpkgs#statix` (ignoring errors) then `nix profile install nixpkgs#nixfmt nixpkgs#statix`

### Requirement: Garbage collection task

The system SHALL provide `gc` that runs `nix-collect-garbage`, depending on `check:nix`.

#### Scenario: Collect garbage

- **WHEN** user runs `task nix:gc`
- **THEN** system runs `nix-collect-garbage`

### Requirement: No CLI_ARGS outside fmt/fix/lint

The system SHALL use `{{.CLI_ARGS}}` only in `fmt`, `fix`, and `lint`, and only at the end of the command.

#### Scenario: Inspection

- **WHEN** inspecting `src/nix/Taskfile.yml`
- **THEN** `{{.CLI_ARGS}}` appears only in fmt/fix/lint at command end

### Requirement: No hardcoded versions

The system SHALL not declare tool versions in the module.

#### Scenario: Inspection

- **WHEN** inspecting `src/nix/Taskfile.yml`
- **THEN** no `GOLANG_VERSION:`/`PYTHON_VERSION:`/`NODE_VERSION:` declarations are present