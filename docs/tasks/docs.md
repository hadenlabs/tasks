# Docs Module

Generación y visualización de documentación con MkDocs y Grip.

## Tasks

### `docs:check`

Verifica Python, package manager, mkdocs y grip.

### `docs:show`

Abre README localmente en el navegador usando Grip.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run grip
```

### `docs:build`

Construye documentación con MkDocs.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run mkdocs build
```

### `docs:serve`

Serve documentation locally.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run mkdocs serve --dev-addr '127.0.0.1:8001'
```

## Dependencies

- `python`
- `mkdocs`
- `grip`