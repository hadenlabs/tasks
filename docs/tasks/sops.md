# SOPS Module

Gestión de secretos encriptados con [SOPS](https://github.com/mozilla/sops) y Age.

## Tasks

### `sops:check`
Verifica SOPS, Age y variables.

### `sops:environment`
Crea directorio para Age keys.

```bash
mkdir -p {{.KEYBASE_PROJECT_PATH}}/age
```

### `sops:make`
Genera clave Age.

```bash
age-keygen -o {{.SOPS_AGE_KEY_FILE}}
```

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `SOPS_AGE_KEY_FILE` | Ruta al archivo de clave Age |
| `KEYBASE_PROJECT_PATH` | Ruta base del proyecto |

## Dependencias

- `sops`
- `age`