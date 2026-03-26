# Go Module

Gestión de proyectos Go.

## Tasks

### `go:check`
Verifica Go, gofmt, golangci-lint y goreleaser.

### `go:setup`
Descarga y configura dependencias.

```bash
go mod download
go mod tidy
go mod vendor
go generate -v ./...
```

### `go:fmt`
Formatea código.

```bash
gofmt -s -l -w {{.GO_FILES}}
```

### `go:lint`
Ejecuta linter.

```bash
golangci-lint run --config .ci/linters/.golangci.yml
```

### `go:fix`
Corrige violaciones de lint.

```bash
golangci-lint run --fix --config .ci/linters/.golangci.yml
```

### `go:vet`
Ejecuta go vet.

```bash
go vet ./...
```

### `go:mockery`
Refresca mocks.

```bash
mockery --config .ci/config/mockery.yaml
```

### `go:generate`
Genera código con Wire.

```bash
wire ./...
```

### `go:test`
Ejecuta tests.

```bash
go test -race -v ./... -coverprofile cover.out -timeout 60m
```

### `go:build`
Build paquetes.

```bash
goreleaser build --snapshot --rm-dist
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `GO_FILES` | Archivos Go (find automático) |

## Dependencias

- `go`
- `gofmt`
- `golangci-lint`
- `goreleaser`
- `mockery` (opcional)
- `wire` (opcional)