# Renovate Module

Ejecución de Renovate Bot en Docker.

## Tasks

### `renovate:check-docker`
Verifica que Docker esté instalado y corriendo.

### `renovate:check-repository-format`
Valida formato org/repo.

### `renovate:renovate-docker-debug`
Ejecuta Renovate en Docker con debugging.

```bash
docker run --rm -it \
  -e LOG_LEVEL="{{.LOG_LEVEL | default "debug"}}" \
  -e GITHUB_TOKEN="${GITHUB_TOKEN}" \
  -e RENOVATE_REPOSITORY="{{.REPOSITORY}}" \
  -v "${PWD}/{{.CONFIG_FILE_PATH | default ".github/renovate.json5"}}:/usr/src/app/renovate.json5" \
  -v "${PWD}:/tmp/renovate/repository" \
  {{.RENOVATE_IMAGE | default "ghcr.io/renovatebot/renovate:latest"}} \
  {{.REPOSITORY}}
```

## Variables

| Variable | Default | Descripción |
|----------|---------|-------------|
| `REPOSITORY` | - | Repositorio (org/repo) |
| `GITHUB_TOKEN` | - | Token de GitHub |
| `LOG_LEVEL` | debug | Nivel de log |
| `PLATFORM` | github | Plataforma (github, gitlab) |
| `RENOVATE_IMAGE` | ghcr.io/renovatebot/renovate:latest | Imagen Docker |
| `CONFIG_FILE_PATH` | .github/renovate.json5 | Path de config |
| `VALIDATE_REPO` | true | Validar repo existe |

## Dependencias

- `docker`
- `gh` (opcional, para validación)