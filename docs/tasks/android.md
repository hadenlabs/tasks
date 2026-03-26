# Android Module

Gestión de proyectos Android.

## Tasks

### `android:check`
Verifica Gradle y ANDROID_HOME.

### `android:setup`
Instala dependencias.

```bash
./gradlew clean install
```

### `android:fmt`
Formatea código con ktlint.

```bash
./gradlew ktlintFormat
```

### `android:lint`
Ejecuta linter.

```bash
./gradlew ktlintCheck
```

### `android:fix`
Corrige violaciones de lint.

```bash
./gradlew ktlintFormat
```

### `android:dependencies`
Muestra dependencias.

```bash
./gradlew dependencies
```

### `android:clean`
Limpia build.

```bash
./gradlew clean
```

### `android:build`
Build proyecto.

```bash
./gradlew clean build
```

### `android:test`
Ejecuta tests.

```bash
./gradlew clean test
```

### `android:test:allure`
Ejecuta tests con Allure.

```bash
./gradlew clean test allureServe
```

### `android:run`
Ejecuta aplicación.

```bash
./gradlew bootRun
```

### `android:publish`
Publica paquetes.

```bash
./gradlew publish
```

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `ANDROID_HOME` | Directorio SDK de Android |
| `GRADLE_VERSION` | Versión de Gradle |

## Dependencias

- `gradlew`
- Android SDK