# OpenSSL Module

Generación de claves SSH y certificados.

## Tasks

### `openssl:check`
Verifica OpenSSL.

### Claves por Ambiente

| Task | Stage |
|------|-------|
| `openssl:make:pem:dev` | dev |
| `openssl:make:pem:testing` | testing |

### `openssl:make`
Genera clave SSH y exporta a directorio.

```bash
ssh-keygen -q -m PEM -t rsa -b 4096 \
  -C "admin@{{.PROJECT_NAME}}.com" \
  -f {{.PROJECT_NAME}} -P ""
openssl rsa -in {{.PROJECT_NAME}} -outform pem > {{.PROJECT_NAME}}.pem
chmod 0600 {{.PROJECT_NAME}}.pem
# Exporta a KEYBASE_PROJECT_PATH
mv {{.PROJECT_NAME}} {{.KEYBASE_PROJECT_PATH}}/{{.STAGE}}/keys/
mv {{.PROJECT_NAME}}.pem {{.KEYBASE_PROJECT_PATH}}/{{.STAGE}}/keys/
mv {{.PROJECT_NAME}}.pub {{.KEYBASE_PROJECT_PATH}}/{{.STAGE}}/keys/
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `PROJECT_NAME` | Nombre del proyecto |
| `STAGE` | Ambiente |
| `KEYBASE_PROJECT_PATH` | Ruta base del proyecto |

## Dependencias

- `openssl`
- `ssh-keygen`