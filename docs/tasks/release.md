# Release Module

Gestión de versiones y changelogs.

## Tasks

### `release:major`
Release major: incrementa versión MAJOR y genera next changelog.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump major --no-commit
task changelog:next
```

### `release:minor`
Release minor: incrementa versión MINOR y genera next changelog.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump minor --no-commit
task changelog:next
```

### `release:patch`
Release patch: incrementa versión PATCH y genera next changelog.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump patch --no-commit
task changelog:next
```

### `release:pre_release`
Release pre-release: incrementa pre_release y genera next changelog.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump pre_release --no-commit
task changelog:next
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `APP_TAG` | Tag actual del repositorio |
| `PYTHON_PACKAGE_MANAGER` | Gestor de paquetes (uv) |

## Dependencias

- `bump-my-version`
- `changelog` (generación de changelogs)