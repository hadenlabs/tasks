# Packer Module

Gestión de imágenes con [Packer](https://www.packer.io).

## Tasks

### `packer:check`

Verifica Packer.

### `packer:init`

Inicializa Packer.

```bash
packer init {{.PACKER_PATH}}/{{.REGION}}/arch/{{.ARCH}}/{{.IMAGE}}
```

### `packer:validate`

Valida templates.

```bash
packer validate {{.PACKER_PATH}}/{{.REGION}}/arch/{{.ARCH}}/{{.IMAGE}}
```

### `packer:fmt`

Formatea templates.

```bash
packer fmt {{.PACKER_PATH}}/{{.REGION}}/arch/{{.ARCH}}/{{.IMAGE}}
```

### `packer:build`

Build imagen.

```bash
packer build {{.PACKER_PATH}}/{{.REGION}}/arch/{{.ARCH}}/{{.IMAGE}}
```

## Variables

| Variable      | Descripción       |
| ------------- | ----------------- |
| `PACKER_PATH` | Ruta de templates |
| `REGION`      | Región            |
| `ARCH`        | Arquitectura      |
| `IMAGE`       | Imagen a build    |

## Dependencias

- `packer`