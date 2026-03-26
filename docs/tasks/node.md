# Node Module

Gestión de Node.js con [fnm](https://github.com/Schniz/fnm).

## Tasks

### `node:check`
Verifica Node y fnm.

### `node:environment`
Instala y usa versión de Node configurada.

```bash
fnm install {{ .NODE_VERSION }} && fnm use {{ .NODE_VERSION }}
```

## Variables

| Variable | Default | Descripción |
|----------|---------|-------------|
| `NODE_VERSION` | v24.11.1 | Versión de Node.js |

## Dependencias

- `node`
- `fnm`