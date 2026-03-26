# Bun Module

Gestión de Bun runtime.

## Tasks

### `bun:check`
Verifica Bun y curl.

### `bun:setup`
Instala Bun si no está presente.

```bash
[ -n "$(command -v bun)" ] || curl -fsSL https://bun.com/install | bash
```

## Dependencias

- `bun`
- `curl`