# Yarn Module

Gestión de proyectos Node.js con [Yarn](https://yarnpkg.com).

## Tasks

### `yarn:check`
Verifica Yarn y fnm.

### `yarn:setup`
Instala dependencias.

```bash
yarn install
```

### `yarn:update`
Actualiza paquetes.

```bash
yarn ncu:patch
```

### `yarn:environment`
Configura versión de Node con fnm.

```bash
fnm use {{.NODE_VERSION}}
```

## Dependencias

- `yarn`
- `fnm`