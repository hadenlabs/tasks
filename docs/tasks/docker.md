# Docker Module

Build y publicación de imágenes Docker.

## Tasks

### `docker:check`
Verifica que `docker` esté instalado y variables de entorno configuradas.

### `docker:login`
Inicia sesión en el registry de contenedores.

```bash
echo -n {{.CI_REGISTRY_PASSWORD}} | docker login \
  -u {{.CI_REGISTRY_USER}} --password-stdin {{.CI_REGISTRY}}
```

### `docker:build`
Construye una imagen Docker.

```bash
docker build --no-cache \
  --platform={{.DOCKER_PLATFORM}} \
  --file Dockerfile \
  --tag {{.CI_REGISTRY}}/{{.GROUP_NAME}}/{{.PROJECT_NAME}}:{{.APP_TAG}} \
  --build-arg VERSION={{.APP_TAG}} .
```

### `docker:publish`
Construye y publica imagen en el registry.

```bash
docker buildx build --no-cache \
  --platform={{.DOCKER_PLATFORM}} \
  --file Dockerfile \
  --tag {{.CI_REGISTRY}}/{{.GROUP_NAME}}/{{.PROJECT_NAME}}:{{.APP_TAG}} \
  --build-arg VERSION={{.APP_TAG}} \
  --push
```

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `CI_REGISTRY_USER` | Usuario del registry |
| `CI_REGISTRY_PASSWORD` | Contraseña del registry |
| `CI_REGISTRY` | URL del registry |
| `DOCKER_PLATFORM` | Plataforma (default: linux/amd64) |
| `PROJECT_NAME` | Nombre del proyecto |
| `GROUP_NAME` | Nombre del grupo/organización |
| `APP_TAG` | Tag de la imagen (version) |

## Dependencias

- `docker`