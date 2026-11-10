## 1. Create Nix module

- [x] 1.1 Create `src/nix/Taskfile.yml` starting with `---` on line 1 and `version: "3"` on line 2 (R1).
- [x] 1.2 Add `default` task listing public tasks, exempt from `run: once`.
- [x] 1.3 Add `check` aggregator with `deps: [check:nix]`.
- [x] 1.4 Add `check:nix` with `sh: command -v nix` and actionable msg including install hint.
- [x] 1.5 Add `fmt` task running `nixfmt . {{.CLI_ARGS}}` with `deps: [check:nix]`.
- [x] 1.6 Add `lint` task running `statix check . {{.CLI_ARGS}}` with `deps: [check:nix]`.
- [x] 1.7 Add `fix` task running `statix fix . {{.CLI_ARGS}}` with `deps: [check:nix]`.
- [x] 1.8 Add `setup` task with `deps: [check:nix, _setup]`.
- [x] 1.9 Add private `_setup` helper with `_` prefix and `run: once`.

## 2. Root integration

- [x] 2.1 Add `nix: ./src/nix/Taskfile.yml` to the `includes:` block of `Taskfile.yml`.

## 3. Documentation

- [x] 3.1 Create `docs/tasks/nix.md` following the `docs/tasks/uv.md` module-docs pattern.

## 4. Validation (guide Section 4)

- [x] 4.1 YAML valid: `python3 -c "import yaml; yaml.safe_load(open('src/nix/Taskfile.yml'))"`.
- [x] 4.2 Public tasks listed: `task -t src/nix/Taskfile.yml --list` shows all public tasks with `desc:`.
- [x] 4.3 Check pattern present: aggregator `check` with `deps` to `check:nix`.
- [x] 4.4 No `run: always`: `grep -c 'run: always' src/nix/Taskfile.yml` returns `0`.
- [x] 4.5 No bare `test {{.`: `grep -c 'test {{\.' src/nix/Taskfile.yml` returns `0`.
- [x] 4.6 Every public task has `desc`: `task -t src/nix/Taskfile.yml --list | grep -v desc` produces no output.
- [x] 4.7 No invented subcommands: `grep -c 'uv run command' src/nix/Taskfile.yml` returns `0`.
- [x] 4.8 No hardcoded versions: grep for `GOLANG_VERSION:`/`PYTHON_VERSION:`/`NODE_VERSION:` returns `0`.
- [x] 4.9 File starts with `---`: `head -1 src/nix/Taskfile.yml` returns `---`.
- [x] 4.10 `task -t src/nix/Taskfile.yml --list` shows all expected public tasks.

## 5. Flake suite

- [x] 5.1 Add `check:flake` task with precondition `sh: test -f flake.nix` and actionable msg.
- [x] 5.2 Add `test` task running `nix flake check` with deps `[check:nix, check:flake]`.
- [x] 5.3 Add `show` task running `nix flake show` with deps `[check:nix, check:flake]`.
- [x] 5.4 Add `lock` task running `nix flake lock` with deps `[check:nix, check:flake]`.
- [x] 5.5 Add `update` task running `nix flake update` with deps `[check:nix, check:flake]`.
- [x] 5.6 Add `develop` task running `nix develop` with deps `[check:nix, check:flake]`.
- [x] 5.7 Add `run` task running `nix develop --command {{.CMD}}` with deps `[check:nix, check:flake]` and precondition `sh: test -n "{{.CMD}}"`.

## 6. Install / reinstall

- [x] 6.1 Add `install` task with idempotent `|` block (DeterminateSystems installer), no deps.
- [x] 6.2 Add `reinstall` task with `|` block (profile remove + install), deps `[check:nix]`.

## 7. direnv

- [x] 7.1 Add `check:direnv` task with `sh: command -v direnv` and actionable msg.
- [x] 7.2 Add `direnv` task running `direnv allow` with deps `[check:direnv, check:nix, check:flake]`.

## 8. Garbage collection

- [x] 8.1 Add `gc` task running `nix-collect-garbage` with deps `[check:nix]`.

## 9. Dual-mode fmt

- [x] 9.1 Change `fmt` to `|` block branching on `flake.nix`, `nix fmt` vs `nixfmt .`, CLI_ARGS at end of each branch.

## 10. Documentation

- [x] 10.1 Update `docs/tasks/nix.md` documenting all public tasks (old + new) with desc and examples.

## 11. Validation (extended)

- [x] 11.1 Re-run guide Section 4 checklist on final file.
- [x] 11.2 `task -t src/nix/Taskfile.yml --list` shows all public tasks with desc; `_setup` hidden.
- [x] 11.3 `openspec validate --changes --strict` passes.
- [x] 11.4 `openspec status --change "add-nix-taskfile"` reports complete.
