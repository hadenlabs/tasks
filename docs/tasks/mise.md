# Mise Module

Gestión de herramientas y runtimes con [mise](https://mise.jdx.dev/) (polyglot version manager).

## Tasks

### `mise:default`

Lista las tareas disponibles del módulo.

```bash
task mise:default
```

### `mise:check`

Verifica que `mise` esté instalado.

```bash
task mise:check
```

### `mise:check:mise`

Verifica que `mise` esté instalado (`command -v mise`).

```bash
task mise:check:mise
```

### `mise:setup`

Confía en la configuración del proyecto e instala las herramientas definidas.

```bash
task mise:setup
```

### `mise:ls`

Lista las versiones de herramientas instaladas.

```bash
task mise:ls
```

### `mise:doctor`

Ejecuta el diagnóstico de mise.

```bash
task mise:doctor
```

### `mise:upgrade`

Actualiza las herramientas dentro de los rangos configurados.

```bash
task mise:upgrade
```

## Dependencias

- `mise` (version manager de herramientas)

```bash
curl -fsSL https://mise.run | sh
```