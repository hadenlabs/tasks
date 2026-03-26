# Gradle Module

Gestión de proyectos Java/Kotlin con Gradle.

## Tasks

### `gradle:check`
Verifica Gradle.

### `gradle:setup`
Instala dependencias.

```bash
./gradlew clean install
```

### `gradle:dependencies`
Muestra dependencias.

```bash
./gradlew dependencies
```

### `gradle:clean`
Limpia build.

```bash
./gradlew clean
```

### `gradle:build`
Build proyecto.

```bash
./gradlew clean build
```

### `gradle:test`
Ejecuta tests.

```bash
./gradlew clean test
```

### `gradle:test:allure`
Ejecuta tests con Allure.

```bash
./gradlew clean test allureServe
```

### `gradle:run`
Ejecuta aplicación.

```bash
./gradlew bootRun
```

### `gradle:publish`
Publica paquetes.

```bash
./gradlew publish
```

### `gradle:environment`
Configura Gradle wrapper.

```bash
curl -o gradlew https://raw.githubusercontent.com/gradle/gradle/master/gradle/wrapper/gradlew
curl -o gradle/wrapper/gradle-wrapper.properties https://raw.githubusercontent.com/gradle/gradle/v{{ .GRADLE_VERSION }}/gradle/wrapper/gradle-wrapper.properties
```

## Variables

| Variable | Default | Descripción |
|----------|---------|-------------|
| `GRADLE_VERSION` | 8.14 | Versión de Gradle |

## Dependencias

- `gradlew` (Gradle Wrapper)