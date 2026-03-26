# Python Module

Gestión de proyectos Python.

## Tasks

### `python:check`
Verifica Python, package manager y pre-commit.

### `python:setup`
Instala dependencias de desarrollo.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} install --with dev
```

### `python:fmt`
Formatea código con black.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run black ./ {{.CLI_ARGS}}
```

### `python:test`
Ejecuta tests con pytest.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run pytest -s
```

### `python:environment`
Instala versión de Python.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} python install
```

### `python:precommit`
Instala hooks de pre-commit.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run pre-commit install
{{ .PYTHON_PACKAGE_MANAGER }} run pre-commit install -t pre-push
{{ .PYTHON_PACKAGE_MANAGER }} run pre-commit install -t commit-msg
{{ .PYTHON_PACKAGE_MANAGER }} run pre-commit install -t prepare-commit-msg
```

## Variables

| Variable | Default | Descripción |
|----------|---------|-------------|
| `PYTHON_PACKAGE_MANAGER` | uv | Gestor de paquetes |

## Dependencias

- `python`
- Package manager configurable (uv, poetry, pip)
- `pre-commit`