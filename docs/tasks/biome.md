# BiomeJS Module

Linting y formateo con [Biome](https://biomejs.dev).

## Tasks

### `biome:check`
Verifica que bunx esté instalado.

### `biome:setup`
Inicializa BiomeJS en el proyecto.

```bash
bunx @biomejs/biome init
```

### `biome:fmt`
Formatea archivos.

```bash
bunx @biomejs/biome format . --write
```

### `biome:lint`
Ejecuta linting.

```bash
bunx @biomejs/biome lint .
```

### `biome:check:all`
Verifica y corrige archivos (fmt + lint).

```bash
bunx @biomejs/biome check --write .
```

### `biome:fix`
Corrige violaciones de lint automáticamente.

```bash
bunx @biomejs/biome lint . --write --max-diagnostics=none --unsafe
```

### `biome:migrate`
Migra configuraciones de Prettier/ESLint.

```bash
bunx @biomejs/biome migrate prettier
bunx @biomejs/biome migrate eslint
```

## Dependencias

- `bunx` (Bun)