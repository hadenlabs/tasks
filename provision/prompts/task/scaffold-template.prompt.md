# Prompt: Agregar nuevo Taskfile template a tasks

## Contexto

Estás trabajando dentro de este repositorio. Debes seguir estrictamente todas las reglas definidas en `AGENTS.md`.

Este repo es una colección de templates `Taskfile.yml` (go-task) + automatización del proyecto. Debes integrar la nueva herramienta como un template en `src/{{tool_name}}/Taskfile.yml` y registrarla en los includes.

No asumas convenciones fuera de `AGENTS.md`. No introduzcas cambios que rompan compatibilidad.

---

## Objetivo

Implementar el template de Taskfile para la herramienta: `{{tool_name}}`

La implementación debe integrarse con la arquitectura actual del repo (Taskfiles en `src/*/Taskfile.yml` + `includes` en `Taskfile.yml` + ejemplo de includes en `docs/usage.md`).

---

## Reglas Obligatorias (AGENTS.md)

Debes respetar estrictamente:

- Preferir `task` (go-task) y `uv` para workflows.
- Estilo Taskfile:
  - Usar `version: "3"`.
  - Usar `---` al inicio cuando aplique; para un nuevo Taskfile en `src/{{tool_name}}/Taskfile.yml`, inclúyelo.
  - Mantener YAML simple (2 espacios de indentación).
  - Añadir `desc:` para tareas user-facing.
  - Usar `preconditions:` para binarios/vars requeridas.
  - Preferir `{{.CLI_ARGS}}` cuando el task sea un wrapper extensible.
  - Usar `run: once` donde aplique (checks/setup/format/lint típicos del repo).
- No editar `README.md` directamente (es generado).
- No tocar/commit `.env`.
- No romper tareas existentes ni cambiar comportamiento global sin necesidad.

---

## Alcance de la Implementación

1. Crear `src/{{tool_name}}/Taskfile.yml`
   - Debe seguir patrones de templates existentes (ejemplo: `src/go/Taskfile.yml`, `src/node/Taskfile.yml`, `src/terraform/Taskfile.yml`).
   - Incluir como mínimo:
     - `check` (orquesta dependencias de checks)
     - `check:{{tool_name}}` (o `check:<binary>` real) con `preconditions: command -v ...`
   - Incluir tareas adicionales si la herramienta lo amerita (sin inventar):
     - `environment` (si requiere version manager/instalación de runtime)
     - `setup`, `fmt`, `lint`, `test`, `build`, `prettier` (solo si hay equivalente claro para esa herramienta)
   - No introducir un build system nuevo; reutiliza configuraciones existentes bajo `.ci/` si ya existen y aplica.

2. Registrar el template en el root `Taskfile.yml`
   - Agregar el include:
     - `{{tool_name}}: ./src/{{tool_name}}/Taskfile.yml`
   - Si el Taskfile nuevo requiere versión parametrizable, agregar en `vars:` del root:
     - `{{TOOL_UPPER}}_VERSION: <valor>` (solo si realmente se usa en el Taskfile)

3. Documentar el include remoto en `docs/usage.md`
   - Agregar una entrada en el bloque `includes:` con el formato existente:
     - `{{tool_name}}:`
       - `taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/{{tool_name}}/Taskfile.yml"`
   - Si agregaste `{{TOOL_UPPER}}_VERSION` (u otras vars), reflejarlo también en el bloque `vars:` del ejemplo (solo si aplica).

---

## Restricciones

- No explicar nada.
- No agregar texto adicional.
- No modificar `README.md` (se regenera con `task readme` fuera de este output).
- No cambiar estructura existente.
- Mantener estilo consistente con el código actual.

---

## Formato de Salida

Responder únicamente con:

1. Implementación completa de `src/{{tool_name}}/Taskfile.yml`
2. Snippet de actualización de `Taskfile.yml` (solo las secciones tocadas: `includes:` y `vars:` si aplica)
3. Snippet de actualización de `docs/usage.md` (solo las secciones tocadas: `includes:` y `vars:` si aplica)

- Agrupar por archivo.
- No agregar comentarios fuera del código.
