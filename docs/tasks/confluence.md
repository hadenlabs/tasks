# Confluence Module

Sincronización de documentación con Atlassian Confluence usando [Mark](https://github.com/kovetskiy/mark).

## Tasks

### `confluence:check`
Verifica Docker y variables de Confluence.

### `confluence:sync`
Sincroniza archivos Markdown a Confluence.

```bash
docker run \
  --platform linux/amd64 \
  --rm \
  --workdir /usr/src/ \
  -v {{.PWD}}/:/usr/src/ \
  -v {{.PWD}}/config.log:/config/config.toml \
  kovetskiy/mark:latest mark --config /config/config.toml \
  --minor-edit -f $file
```

### `confluence:sync:all`
Sincroniza todos los archivos `.md` con comentarios de Space.

```bash
# Busca archivos con <!-- Space --> y los sincroniza
```

### `confluence:sync:develop`
Sincroniza archivos modificados vs rama develop.

```bash
# Archivos diferentes entre develop y HEAD
```

### `confluence:sync:main`
Sincroniza archivos modificados vs rama main.

### `confluence:sync:master`
Sincroniza archivos modificados vs rama master.

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `CONFLUENCE_BASE_URL` | URL base de Confluence |
| `CONFLUENCE_ACCESS_TOKEN` | Token de acceso |
| `CONFLUENCE_USER` | Usuario de Confluence |
| `FILES_CONFLUENCE` | Archivos a sincronizar |

## Dependencias

- `docker`
- Imagen: `kovetskiy/mark`