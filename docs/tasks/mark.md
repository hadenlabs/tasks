# Mark Module

Sincronización de documentación a Confluence usando [Mark](https://github.com/kovetskiy/mark).

## Tasks

### `mark:check`
Verifica Docker y variables de Confluence.

### `mark:sync`
Sincroniza archivo Markdown a Confluence.

```bash
docker run \
  --platform linux/amd64 \
  --rm \
  --workdir /usr/src/ \
  -v {{.PWD}}/:/usr/src/ \
  kovetskiy/mark:latest mark \
  -u {{.CONFLUENCE_USER}} \
  -p {{.CONFLUENCE_ACCESS_TOKEN}} \
  -b {{.CONFLUENCE_BASE_URL}} \
  -f {{.CLI_ARGS}}
```

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `CONFLUENCE_BASE_URL` | URL base de Confluence |
| `CONFLUENCE_ACCESS_TOKEN` | Token de acceso |
| `CONFLUENCE_USER` | Usuario de Confluence |

## Dependencias

- `docker`
- Imagen: `kovetskiy/mark`