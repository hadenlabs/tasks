# OpenCode Module

Gestión de configuración de OpenCode.

## Tasks

### `opencode:check`
Verifica OpenCode y curl.

### `opencode:setup`
Instala OpenCode si no está presente.

```bash
[ -n "$(command -v opencode)" ] || curl -fsSL https://opencode.ai/install | bash
```

### `opencode:sync`
Sincroniza commands, skills y context de OpenCode.

### `opencode:validate`
Valida configuración de OpenCode.

### `opencode:check:all`
Ejecuta sync y validate.

## Dependencias

- `opencode`
- `curl`