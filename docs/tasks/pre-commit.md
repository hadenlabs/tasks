# Pre-commit Module

Gestión de hooks de pre-commit.

## Tasks

### `pre-commit:update-hooks`
Actualiza todos los hooks de pre-commit a sus últimas versiones.

```bash
pre-commit autoupdate
```

### `pre-commit:clear-cache`
Limpia el cache de pre-commit.

```bash
pre-commit clean
```

### `pre-commit:run-hooks`
Ejecuta todos los hooks localmente.

```bash
pre-commit run --all-files --show-diff-on-failure
```

## Dependencias

- `pre-commit`