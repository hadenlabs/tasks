# Keybase Module

Gestión de claves GPG para SOPS.

## Tasks

### `keybase:check`
Verifica SOPS, GPG y variables.

### `keybase:make`
Genera clave GPG.

```bash
gpg --batch --gen-key {{.GPG_FILE_CONF}}
```

### `keybase:import`
Importa clave GPG.

```bash
gpg --import {{.KEYBASE_PROJECT_PATH}}/gpg/key.asc
```

### `keybase:export`
Exporta clave GPG secreta.

```bash
gpg --armor --export-secret-keys {{.GPG_KEY}} > {{.KEYBASE_PROJECT_PATH}}/gpg/key.asc
```

### `keybase:environment`
Crea directorios para GPG y K8s.

```bash
mkdir -p {{.KEYBASE_PROJECT_PATH}}/gpg
mkdir -p {{.KEYBASE_TEAM_PATH}}/k8s
```

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `GPG_KEY` | ID de clave GPG |
| `KEYBASE_PROJECT_PATH` | Ruta base del proyecto |
| `KEYBASE_TEAM_PATH` | Ruta del team |
| `GPG_FILE_CONF` | Archivo de configuración GPG |

## Dependencias

- `sops`
- `gpg`