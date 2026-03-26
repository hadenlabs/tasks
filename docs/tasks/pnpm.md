# pnpm Module

Gestión de proyectos Node.js con [pnpm](https://pnpm.io).

## Tasks

### `pnpm:check`
Verifica pnpm y fnm.

### `pnpm:setup`
Instala dependencias.

```bash
pnpm install
```

### `pnpm:update`
Actualiza paquetes.

```bash
pnpm run ncu:patch
```

### `pnpm:environment`
Configura versión de Node con fnm.

```bash
fnm use {{.NODE_VERSION}}
```

## Dependencias

- `pnpm`
- `fnm`