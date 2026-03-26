# SonarQube Module

Análisis de código con SonarQube.

## Tasks

### `sonar:check`
Verifica Docker y variables de SonarQube.

### `sonar:show`
Muestra variables de SonarQube.

```bash
echo {{.SONAR_URL}}
echo {{.SONAR_TOKEN}}
```

### `sonar:scan`
Ejecuta scan de SonarQube.

```bash
docker run --rm \
  --platform linux/amd64 \
  --volume $(pwd)/:/usr/src \
  --env SONAR_URL={{.SONAR_URL}} \
  --env SONAR_TOKEN={{.SONAR_TOKEN}} \
  sonarsource/sonar-scanner-cli:latest \
  sonar-scanner
```

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `SONAR_URL` | URL del servidor SonarQube |
| `SONAR_TOKEN` | Token de autenticación |

## Dependencias

- `docker`
- Imagen: `sonarsource/sonar-scanner-cli`