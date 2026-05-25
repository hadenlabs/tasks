# 🚀 How to use this project

## Quick Start

Include the remote Taskfiles in your project's `Taskfile.yaml`:

```yaml
version: "3"

includes:
  changelog:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/changelog/Taskfile.yml"
  git:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/git/Taskfile.yml"
  docker:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/docker/Taskfile.yml"
  terraform:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/terraform/Taskfile.yml"
  uv:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/uv/Taskfile.yml"
  # ... see /docs/tasks/ for all available modules
```

## Available Modules

See [docs/tasks/](/docs/tasks/) for detailed documentation on each module:

| Module | Description |
|--------|-------------|
| [changelog](/docs/tasks/changelog.md) | Changelog generation with git-chglog |
| [git](/docs/tasks/git.md) | Git setup, ignore, reviews |
| [pre-commit](/docs/tasks/pre-commit.md) | Pre-commit hooks management |
| [prettier](/docs/tasks/prettier.md) | Code formatting |
| [docker](/docs/tasks/docker.md) | Docker build & publish |
| [terraform](/docs/tasks/terraform.md) | Terraform & tfenv |
| [github](/docs/tasks/github.md) | GitHub CLI automation |
| [uv](/docs/tasks/uv.md) | Python with uv |
| [python](/docs/tasks/python.md) | Python package manager |
| [node](/docs/tasks/node.md) | Node.js with fnm |
| [bun](/docs/tasks/bun.md) | Bun runtime |
| [release](/docs/tasks/release.md) | Version bumps |
| [confluence](/docs/tasks/confluence.md) | Confluence sync |
| [docs](/docs/tasks/docs.md) | MkDocs documentation |
| And more... | |

## Common Tasks

```bash
# Check dependencies
task check

# Setup project
task setup

# Format code
task format

# Run tests
task test

# Generate README
task readme
```

## Documentation

For detailed task documentation, see:

- [docs/tasks/](/docs/tasks/) - Module-specific documentation
- [docs/index.md](/docs/index.md) - Project documentation