## Context

The `src/ai/Taskfile.yml` currently manages AI tooling (opencode, codegraph, openspec) with a consistent pattern: check → environment → setup (init/sync). Graphify needs to follow this same pattern for seamless integration.

Graphify is installed via `uv tool install graphifyy` and the CLI command is `graphify`. It supports OpenCode platform via `graphify install --platform opencode`.

## Goals / Non-Goals

**Goals:**
- Add graphify check, environment, setup, and operational tasks to `src/ai/Taskfile.yml`
- Follow the existing task pattern (check → environment → setup)
- Support project-scoped installs for OpenCode
- Include graph build, query, and hook installation tasks

**Non-Goals:**
- Modifying graphify itself
- Changing existing tool patterns
- Adding graphify to other Taskfiles outside `src/ai/`

## Decisions

### Decision 1: Follow existing pattern (check → environment → setup)
**Rationale**: Consistency with codegraph/openspec reduces cognitive load and maintenance burden.
**Alternatives considered**: Standalone tasks without the pattern (rejected for consistency).

### Decision 2: Use `uv tool install graphifyy` for environment
**Rationale**: Matches how openspec is installed (via bun), keeps tools isolated.
**Alternatives considered**: pip/pipx (rejected due to PATH issues on macOS).

### Decision 3: Support both global and project-scoped installs
**Rationale**: `graphify install --project` writes to `.claude/skills/graphify/` or `.agents/skills/graphify/` for team sharing.
**Alternatives considered**: Global only (rejected for team collaboration needs).

### Decision 4: Include hook installation for auto-rebuild
**Rationale**: `graphify hook install` auto-rebuilds graph on git commit (AST only, no API cost).
**Alternatives considered**: Manual rebuild only (rejected for developer experience).

## Risks / Trade-offs

- **Risk**: Graphify Python version conflicts with existing uv-managed Python → **Mitigation**: Use `uv tool install` which isolates the package
- **Risk**: Graph build time on large repos → **Mitigation**: Support `--update` flag for incremental builds
- **Trade-off**: Adds another dependency to maintain → **Benefit**: Significant improvement in code navigation and understanding
