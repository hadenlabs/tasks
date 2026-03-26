# Version Module

Gestión de versiones con bump-my-version.

## Tasks

### `version:default`
Muestra la versión actual del release.

```bash
echo "Release version {{.APP_TAG}}"
```

### `version:major`
Incrementa versión MAJOR (X.0.0).

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump major --no-commit
```

### `version:minor`
Incrementa versión MINOR (x.Y.0).

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump minor --no-commit
```

### `version:patch`
Incrementa versión PATCH (x.y.Z).

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump patch --no-commit
```

### `version:pre_release`
Incrementa pre_release.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run bump-my-version bump pre_release --no-commit
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `APP_TAG` | Tag actual del repositorio |
| `PYTHON_PACKAGE_MANAGER` | Gestor de paquetes |

## Dependencias

- `bump-my-version`