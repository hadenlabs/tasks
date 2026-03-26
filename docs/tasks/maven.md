# Maven Module

Gestión de proyectos Java con Maven.

## Tasks

### `maven:check`
Verifica Maven.

### `maven:setup`
Instala dependencias.

```bash
./mvnw clean install -DskipTests
```

### `maven:dependencies`
Muestra árbol de dependencias.

```bash
./mvnw dependency:tree
```

### `maven:clean`
Limpia build.

```bash
./mvnw clean
```

### `maven:build`
Build proyecto.

```bash
./mvnw clean package
```

### `maven:test`
Ejecuta tests.

```bash
./mvnw clean test
```

### `maven:test:allure`
Ejecuta tests con Allure.

```bash
./mvnw clean test
./mvnw allure:serve
```

### `maven:run`
Ejecuta Spring Boot.

```bash
./mvnw spring-boot:run
```

### `maven:publish`
Publica paquetes.

```bash
./mvnw deploy
```

## Variables

| Variable | Default | Descripción |
|----------|---------|-------------|
| `MAVEN_VERSION` | 3.9.9 | Versión de Maven |

## Dependencias

- `mvnw` (Maven Wrapper)