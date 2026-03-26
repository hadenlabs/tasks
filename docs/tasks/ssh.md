# SSH Module

Generación y gestión de claves SSH.

## Tasks

### `ssh:check`
Verifica ssh-keygen.

### SSH Keys por Ambiente

| Task | Stage |
|------|-------|
| `ssh:make:pem:dev` | dev |
| `ssh:make:pem:staging` | staging |
| `ssh:make:pem:testing` | testing |
| `ssh:make:pem:prod` | prod |
| `ssh:make:pem:custom` | custom |

### `ssh:make`
Genera clave SSH y convierte a formato PEM.

```bash
ssh-keygen -q -m PEM -t rsa -b 4096 \
  -C "admin@{{.PROJECT_NAME}}-{{.STAGE}}.com" \
  -f {{.PROJECT_NAME}}-{{.STAGE}} -P ""
openssl rsa -in {{.PROJECT_NAME}}-{{.STAGE}} -outform pem > {{.PROJECT_NAME}}-{{.STAGE}}.pem
chmod 0600 {{.PROJECT_NAME}}-{{.STAGE}}.pem
```

### `ssh:export`
Exporta claves a directorio del proyecto.

```bash
mkdir -p {{.KEYBASE_PROJECT_PATH}}/{{.STAGE}}/keys/
mv {{.PROJECT_NAME}}-{{.STAGE}} {{.KEYBASE_PROJECT_PATH}}/{{.STAGE}}/keys/
mv {{.PROJECT_NAME}}-{{.STAGE}}.pub {{.KEYBASE_PROJECT_PATH}}/{{.STAGE}}/keys/
mv {{.PROJECT_NAME}}-{{.STAGE}}.pem {{.KEYBASE_PROJECT_PATH}}/{{.STAGE}}/keys/
```

### `ssh:make:user`
Genera clave SSH para usuario específico.

```bash
ssh-keygen -q -m PEM -t rsa -b 4096 \
  -C "{{.USERNAME}}" -f {{.USERNAME}} -P ""
openssl rsa -in {{.USERNAME}} -outform pem > {{.USERNAME}}.pem
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `PROJECT_NAME` | Nombre del proyecto |
| `STAGE` | Ambiente (dev, staging, prod, etc.) |
| `USERNAME` | Nombre de usuario |
| `KEYBASE_PROJECT_PATH` | Ruta base del proyecto |

## Dependencias

- `ssh-keygen`
- `openssl`