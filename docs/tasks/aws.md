# AWS Module

Gestión de recursos AWS.

## Tasks

### `aws:check-aws`
Verifica que AWS CLI esté instalado.

### `aws:check-jq`
Verifica que jq esté instalado.

### `aws:cleanup-kms`
Encuentra y programa eliminación de KMS keys sin tag Name.

```bash
# Para cada key sin Name tag:
# 1. Deshabilita la key
# 2. Programa eliminación en N días
```

**Variables:**

| Variable | Default | Descripción |
|----------|---------|-------------|
| `REGION` | us-east-2 | Región AWS |
| `DAYS_TO_DELETION` | 7 | Días antes de eliminación |

### `aws:cleanup-efs`
Elimina EFS file system y sus mount targets.

```bash
# 1. Obtiene mount targets
# 2. Elimina cada mount target
# 3. Espera a que se eliminen
# 4. Elimina el file system
```

**Variables:**

| Variable | Descripción |
|----------|-------------|
| `REGION` | Región AWS (default: us-east-2) |
| `FILE_SYSTEM_ID` | ID del EFS a eliminar |

## Dependencias

- `aws` CLI
- `jq`