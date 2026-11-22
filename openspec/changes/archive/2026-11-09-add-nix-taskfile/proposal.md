# Proposal

## Why

The repository centralizes reusable Taskfile.yml templates per tool under `src/`. There is no module for Nix, so consumers cannot manage Nix-related checks, formatting, linting, and setup through the same standardized contract as other modules. Adding a Nix module makes the template set complete for Nix-based projects and fulfills issue PE-559.

This change was extended with an approved scope: a flake suite (`test`/`show`/`lock`/`update`/`develop`/`run`), Nix install/reinstall tasks, direnv integration, and `nix-collect-garbage`, plus a dual-mode `fmt` that uses `nix fmt` for flake projects and `nixfmt` otherwise. The goal is to support many consumer projects (including flake-based ones such as codip-ai) through the same standardized task contract.

## What Changes

- Add new file `src/nix/Taskfile.yml` following the canonical template and hard rules R1–R15 from `docs/guides/taskfile.md`.
- Register the module in the root `Taskfile.yml` `includes:` block as `nix: ./src/nix/Taskfile.yml`.
- Provide Nix tooling tasks: `default`, `check` aggregator, `check:nix`, `fmt` (dual-mode: `nix fmt` when `flake.nix` exists, otherwise `nixfmt`), `lint` (statix), `fix` (statix fix), `setup`, and private helper `_setup`.
- Provide flake suite tasks: `check:flake` (precondition on `flake.nix`), `test` (`nix flake check`), `show` (`nix flake show`), `lock` (`nix flake lock`), `update` (`nix flake update`), `develop` (`nix develop`), `run` (`nix develop --command {{.CMD}}`).
- Provide install/reinstall tasks: `install` (idempotently install Nix via the DeterminateSystems installer when missing), `reinstall` (remove + reinstall `nixfmt`/`statix` in the nix profile).
- Provide direnv integration: `check:direnv` and `direnv` (`direnv allow`).
- Provide `gc` (`nix-collect-garbage`).
- Add module documentation at `docs/tasks/nix.md` (implementation step).

## Capabilities

### New Capabilities

- `nix-taskfile`: Nix tooling Taskfile module providing availability checks, formatting, linting, fixing, setup, flake operations, install/reinstall, direnv, and garbage collection tasks following the repository's deterministic Taskfile contract.

### Modified Capabilities

## Impact

- **Files added**: `src/nix/Taskfile.yml`, `docs/tasks/nix.md`.
- **Files modified**: `Taskfile.yml` (includes registration), OpenSpec artifacts under `openspec/changes/add-nix-taskfile/`.
- **Dependencies**: Nix ecosystem tools (`nix`, `nixfmt`, `statix`, `direnv`) checked at runtime, not version-pinned in the module.
- **Systems affected**: Reusable template consumers using Nix, including flake-based projects.