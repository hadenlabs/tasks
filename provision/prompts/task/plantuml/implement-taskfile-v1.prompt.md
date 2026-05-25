# Prompt: Actualizar Taskfile template de plantuml

## Contexto

Estas trabajando dentro de este repositorio. Debes seguir estrictamente todas las reglas definidas en `AGENTS.md`.

Este repo es una coleccion de templates `Taskfile.yml` (go-task) + automatizacion del proyecto. Debes mantener el template en `src/plantuml/Taskfile.yml` y su documentacion de uso remoto.

No asumas convenciones fuera de `AGENTS.md`. No introduzcas cambios que rompan compatibilidad.

---

## Objetivo

Actualizar y estandarizar el template de Taskfile para la herramienta: `plantuml`

La implementacion debe integrarse con la arquitectura actual del repo (Taskfiles en `src/*/Taskfile.yml` + `includes` en `Taskfile.yml` + ejemplo de includes en `docs/usage.md`).

---

## Reglas Obligatorias (AGENTS.md)

Debes respetar estrictamente:

- Preferir `task` (go-task) y `uv` para workflows.
- Estilo Taskfile:
  - Usar `version: "3"`.
  - Usar `---` al inicio cuando aplique; para `src/plantuml/Taskfile.yml`, incluyelo.
  - Mantener YAML simple (2 espacios de indentacion).
  - Anadir `desc:` para tareas user-facing.
  - Usar `preconditions:` para binarios/vars requeridas.
  - Preferir `{{.CLI_ARGS}}` cuando el task sea un wrapper extensible.
  - Usar `run: once` donde aplique (checks/setup/format/lint tipicos del repo).
- No editar `README.md` directamente (es generado).
- No tocar/commit `.env`.
- No romper tareas existentes ni cambiar comportamiento global sin necesidad.

---

## Alcance de la Implementacion

1. Crear o actualizar `src/plantuml/Taskfile.yml`
   - Debe seguir patrones de templates existentes.
   - Incluir como minimo:
     - `check` (orquesta dependencias de checks)
     - `check:plantuml` (o `check:<binary>` real) con `preconditions: command -v ...`
   - Incluir tareas adicionales solo si existe un equivalente claro para plantuml.
   - No introducir un build system nuevo.

2. Registrar (si aplica) el template en el root `Taskfile.yml`
   - Agregar el include:
     - `plantuml: ./src/plantuml/Taskfile.yml`
   - Solo si el Taskfile requiere version parametrizable, agregar en `vars:` del root:
     - `PLANTUML_VERSION: <valor>`

3. Documentar el include remoto en `docs/usage.md`
   - Agregar una entrada en el bloque `includes:` con el formato existente:
     - `plantuml:`
       - `taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/plantuml/Taskfile.yml"`
   - Si agregaste `PLANTUML_VERSION` (u otras vars), reflejarlo tambien en el bloque `vars:` del ejemplo (solo si aplica).

---

## Restricciones

- No explicar nada.
- No agregar texto adicional.
- No modificar `README.md`.
- No cambiar estructura existente.
- Mantener estilo consistente con el codigo actual.

---

## Formato de Salida

Responder unicamente con:

1. Implementacion completa de `src/plantuml/Taskfile.yml`
2. Snippet de actualizacion de `Taskfile.yml` (solo las secciones tocadas: `includes:` y `vars:` si aplica)
3. Snippet de actualizacion de `docs/usage.md` (solo las secciones tocadas: `includes:` y `vars:` si aplica)

- Agrupar por archivo.
- No agregar comentarios fuera del codigo.