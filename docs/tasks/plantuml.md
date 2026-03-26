# PlantUML Module

Generate UML diagrams with PlantUML.

## Tasks

### `plantuml:check`
Verify Docker is installed.

### `plantuml:make`
Generate PlantUML diagrams using Docker.

```bash
docker run \
  --platform linux/amd64 \
  --rm \
  --workdir /data \
  -v $(pwd):/data \
  hadenlabs/plantuml {{.CLI_ARGS}}
```

## Dependencies

- `docker`
- Image: `hadenlabs/plantuml`