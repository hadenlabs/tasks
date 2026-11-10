# Design

## Context

See proposal.md — Why. The repository already has a deterministic Taskfile contract (`docs/guides/taskfile.md`) with a canonical template and rules R1–R15. Modules such as `src/uv/Taskfile.yml` and `src/go/Taskfile.yml` follow it. There is no Nix module yet. The root `Taskfile.yml` aggregates modules via an `includes:` block. This change adds `src/nix/Taskfile.yml` and registers it, without touching other modules.

## Goals / Non-Goals

**Goals:**
- Add `src/nix/Taskfile.yml` satisfying R1–R15 and the validation checklist.
- Register `nix: ./src/nix/Taskfile.yml` in the root includes block.
- Choose Nix-ecosystem tools for formatting/linting/fixing and document them.
- Support flake-based consumer projects (test/show/lock/update/develop/run).
- Support installing/reinstalling Nix tooling for many consumer projects.
- Integrate direnv (`direnv allow`) for flake devShells.
- Keep the module passing in non-flake projects (`check` aggregator depends only on `check:nix`).

**Non-Goals:**
- Adding environment variables or `check:vars` (none required).
- Modifying other modules or duplicating their checks.
- Modifying consumer repositories (e.g. codip-ai).

## Decisions

### Decision 1: Formatter = `nixfmt` (fallback) + `nix fmt` (flake)
**Rationale**: `nixfmt` is the canonical formatter in the Nix ecosystem. Flake-based projects (such as codip-ai) use `nix fmt` (backed by `nixpkgs-fmt` via the flake's formatter). The module's `fmt` task is dual-mode: when `flake.nix` exists it runs `nix fmt`; otherwise it runs `nixfmt`.  
**Alternatives considered**: `nixpkgs-fmt` (rejected as primary; used only via `nix fmt` in flake projects).

### Decision 2: Linter/Fixer = `statix`
**Rationale**: `statix` provides both `check` and `fix` subcommands, covering lint and auto-fix with one tool.  
**Alternatives considered**: `nix-linter` (rejected; less maintained); `deadnix` (rejected; narrower scope).

### Decision 3: Single availability check `check:nix` (+ `check:flake`, `check:direnv`)
**Rationale**: Per scope, `nix` availability is checked by `check:nix`; `nixfmt`/`statix` are invoked directly and their absence surfaces at runtime. Flake availability is a separate `check:flake` precondition (not part of the `check` aggregator) so non-flake projects keep passing `check`. direnv availability is checked by `check:direnv`. This avoids inventing extra `check:<tool>` tasks not required.  
**Alternatives considered**: Separate `check:nixfmt`/`check:statix` tasks (rejected to keep the module minimal); adding `check:flake` to the aggregator (rejected; breaks non-flake projects).

### Decision 4: Omit `check:vars`
**Rationale**: The Nix module requires no environment variables, so `check:vars` is omitted to avoid inventing requirements (R6).  
**Alternatives considered**: Adding a placeholder `check:vars` (rejected; violates "no invented requirements").

### Decision 5: Actionable install hint
**Rationale**: `check:nix` msg includes https://nixos.org/download and the DeterminateSystems installer command, satisfying R5. The same URL is reused by `install`.  
**Alternatives considered**: Generic "install nix" message (rejected; not actionable).

### Decision 6: Install/reinstall support (auto-installing Nix IS a product requirement)
**Rationale**: The module must support many consumer projects, including ones without Nix installed. `install` is idempotent: it prints "nix is already installed" when `nix` is present, otherwise requires `curl` and runs the DeterminateSystems installer. `reinstall` removes and reinstalls `nixfmt`/`statix` in the nix profile, depending on `check:nix`. The private `_setup` helper installs `nixfmt`/`statix` via `nix profile install`, depending on `check:nix`. `setup` depends on `check:nix` and `_setup` (R10, R13).  
**Alternatives considered**: Auto-installing Nix inside `setup` (rejected; setup keeps its check-based contract); skipping install entirely (rejected; user-approved requirement).

### Decision 7: `run` uses `{{.CMD}}` + `test -n "{{.CMD}}"` (R6/R8)
**Rationale**: R8 restricts `{{.CLI_ARGS}}` to fmt/fix/lint. The `run` task executes an arbitrary command inside `nix develop`, so it uses `{{.CMD}}` and validates presence with a rendered non-empty check `sh: test -n "{{.CMD}}"`. Note that `CMD` here is a go-task TEMPLATE variable rendered by the engine, not an environment variable: go-task does not export CLI vars to the precondition shell, so `sh: test -v CMD` empirically fails for CLI vars and `nix:run` could never run. Using `test -n "{{.CMD}}"` renders the template first; when CMD is unset go-task renders empty → `test -n ""` → false → actionable msg; when set → passes. This is NOT a bare `test {{.VAR}}` against an environment variable (blocker rule 2), so the checklist grep `test {{\.` stays 0 — the string is `test -n "{{.`.  
**Alternatives considered**: Using `{{.CLI_ARGS}}` (rejected; violates R8); `sh: test -v CMD` (rejected; empirically fails for CLI vars).

### Decision 8: `fmt` dual-mode uses a `|` block
**Rationale**: R9 requires `|` for logic/conditionals. The `fmt` task branches on `flake.nix` existence; CLI_ARGS stays at the end of each branch (R8). `fmt` depends on `check:nix`.  
**Alternatives considered**: Two separate tasks (rejected; single `fmt` entry point matches consumer expectation).

### Decision 9: direnv
**Rationale**: `direnv allow` is a common operation for flake devShells. `direnv` depends on `check:direnv`, `check:nix`, and `check:flake` (all these tasks execute `nix` binaries; `direnv allow` with `use flake` also requires nix); `check:direnv` uses `command -v direnv` with an actionable install hint.  
**Alternatives considered**: Combining into `develop` (rejected; separate task keeps concerns clear).

### Decision 11: Flake tasks depend on `check:nix` + `check:flake`
**Rationale**: All flake tasks (test, show, lock, update, develop, run) execute `nix` binaries, so they declare `check:nix` in addition to `check:flake`. The stricter deps stay in code (artifact reconciliation F2). `direnv` additionally depends on `check:direnv`.  
**Alternatives considered**: Depending only on `check:flake` (rejected; nix binary may be missing even when flake.nix exists).

### Decision 10: Root registration
**Rationale**: Add `nix: ./src/nix/Taskfile.yml` to the `includes:` block (already registered). Use namespace: path format (Section 6).  
**Alternatives considered**: None.

## Risks / Trade-offs

- **Risk**: `nixfmt`/`statix` may not be installed even when `nix` is → **Mitigation**: `_setup` installs them via `nix profile install`; runtime errors remain actionable.
- **Risk**: `nix profile install` requires network access → **Mitigation**: Documented; consumers can pre-install tools.
- **Risk**: `install` runs an external installer (curl | sh) → **Mitigation**: idempotent check first; only runs when `nix` is missing; curl presence verified.
- **Risk**: Root `includes:` edit could conflict with concurrent changes → **Mitigation**: Minimal single-line insertion (already present).

## Migration Plan

1. Update OpenSpec artifacts (proposal, spec, design, tasks).
2. Edit `src/nix/Taskfile.yml` to the final task set.
3. Update `docs/tasks/nix.md`.
4. Run validation checklist (guide Section 4) and `task -t src/nix/Taskfile.yml --list`.

## Open Questions

None.
