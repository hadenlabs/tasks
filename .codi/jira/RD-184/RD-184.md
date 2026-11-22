# Task: Implementar tuicr para code review en tasks

## Issue Metadata

- projectKey: RD
- issueType: Task
- summary: Agregar tareas review:start, review:sessions y review:feedback al Taskfile en src/review/
- component: QA
- labels: []
- parentEpic:
- issueKey: RD-184
- jpdSource:

## Scenario

El repositorio tasks es una colección de Taskfiles reutilizables organizados por herramienta bajo src/*/Taskfile.yml. Actualmente no existe un módulo para code review.

Se necesita integrar tuicr (CLI de code review) como un nuevo módulo src/review/Taskfile.yml, agnóstico a la herramienta, que permita:

- Iniciar una sesión de code review sobre los cambios actuales.
- Listar las sesiones de review disponibles como JSON.
- Obtener el feedback de la sesión de review activa.

### Acceptance Tests

1. Crear src/review/Taskfile.yml siguiendo el patrón del repo (version "3", check, preconditions, run: once).
2. Implementar la tarea review:start que ejecute `tuicr -w` para iniciar una sesión de review.
3. Implementar la tarea review:sessions que ejecute `tuicr review list --repo .` y devuelva las sesiones como JSON.
4. Implementar la tarea review:feedback que obtenga la sesión activa con jq y ejecute `tuicr review comments --repo . --session <slug>`.
5. Verificar que las tareas se ejecutan correctamente con `task review:start`, `task review:sessions` y `task review:feedback`.

### Sources

- https://github.com/hadenlabs/tasks.git

- https://github.com/hadenlabs/tasks.git