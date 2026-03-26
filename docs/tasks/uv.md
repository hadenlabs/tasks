# UV Module

Gestión de Python con [uv](https://github.com/astral-sh/uv).

## Tasks

### `uv:check`
Verifica Python, uv, pre-commit y ruff.

### `uv:setup`
Instala dependencias de Python.

```bash
uv sync --all-groups
```

### `uv:fmt`
Formatea código con ruff.

```bash
uv run ruff format ./ {{.CLI_ARGS}}
```

### `uv:lint`
Linting con ruff y mypy.

```bash
uv run ruff check ./ {{.CLI_ARGS}}
uv run mypy src
```

### `uv:test`
Ejecuta tests con pytest y coverage.

### `uv:pytest`
Ejecuta tests con coverage.

```bash
uv run coverage run -m pytest -s -vv
uv run coverage report
uv run coverage html
```

### `uv:mypy`
Ejecuta verificación de tipos con mypy.

```bash
uv run mypy src
```

### `uv:environment`
Instala versión de Python.

```bash
uv python install
```

### `uv:precommit`
Instala hooks de pre-commit.

```bash
uv run pre-commit install
uv run pre-commit install -t pre-push
uv run pre-commit install -t commit-msg
uv run pre-commit install -t prepare-commit-msg
```

## Dependencias

- `python`
- `uv`
- `pre-commit`
- `ruff`
- `mypy`