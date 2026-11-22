## Why

Graphify builds knowledge graphs from code, docs, PDFs, images, and videos — queryable knowledge graphs you can traverse instead of grepping through files. We need to integrate it into the `src/ai/Taskfile.yml` alongside existing tools (opencode, codegraph, openspec) to enable project-wide code intelligence and graph-based querying for the team.

## What Changes

- Add `graphify` check, environment, setup, and operational tasks to `src/ai/Taskfile.yml`
- Follow the same pattern as codegraph/openspec: check → environment → setup (init/sync)
- Support project-scoped installs via `--project` flag for OpenCode platform
- Support graph initialization, sync, and query operations

## Capabilities

### New Capabilities

- `graphify-integration`: Graphify CLI installation, skill registration with OpenCode, and project-level graph operations (build, query, path, explain)

### Modified Capabilities

## Impact

- **Files modified**: `src/ai/Taskfile.yml`
- **Dependencies added**: `graphifyy` (PyPI package via uv/pipx)
- **Systems affected**: AI tooling pipeline (alongside codegraph, openspec)