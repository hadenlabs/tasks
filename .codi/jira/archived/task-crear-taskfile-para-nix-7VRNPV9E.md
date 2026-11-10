# Task: Crear Taskfile para Nix

## Issue Metadata

- projectKey: PE
- issueType: Task
- summary: Implementar módulo Taskfile.yml para Nix según guía taskfile.md (R1–R15) y registrar en Taskfile raíz.
- component: DevOps
- labels: [taskfile, nix]
- parentEpic: 
- issueKey: PE-559
- jpdSource: NIP-23

## Scenario

El repositorio hadenlabs/tasks centraliza templates reutilizables de Taskfile.yml. Se necesita un módulo para Nix (src/nix/Taskfile.yml) que cumpla el contrato de determinismo de docs/guides/taskfile.md: template canónico, reglas duras R1–R15, tablas de decisión y checklist de validación, integrándose en el bloque includes: del Taskfile raíz bajo el namespace nix.


### Acceptance Tests

1. src/nix/Taskfile.yml inicia con --- en la línea 1 y version: "3" (R1).
2. Todas las tareas públicas tienen desc: y run: once; default queda exento de run: once (R2, R3).
3. Patrón check de dos niveles: agregador check con deps hacia check:<tool>, cada check:<tool> usa sh: command -v <tool> con msg: accionable (R4, R5).
4. check:vars usa sh: test -v VAR; nunca test {{.VAR}} sin comprobar existencia (R6).
5. fmt/fix/lint declaran deps sobre su check y colocan {{.CLI_ARGS}} al FINAL del comando (R7, R8).
6. cmds usa >- para comandos largos de una sola línea lógica y | para scripts bash con lógica (R9).
7. Helpers privados con prefijo _; setup declara deps sobre sus checks (R10, R13).
8. Sin versiones hardcodeadas en el módulo (R12) y sin checks duplicados de otros módulos (R15); nunca subcomandos inventados como uv run command -v (R14).
9. El módulo queda registrado en includes: del Taskfile.yml raíz con formato nix: ./src/nix/Taskfile.yml.
10. Pasan los 10 items del checklist de validación (sección 4 de la guía) y task -t src/nix/Taskfile.yml --list muestra todas las tareas públicas con desc.


### Sources

- docs/guides/taskfile.md
- https://taskfile.dev/
- https://github.com/go-task/task
- https://github.com/hadenlabs/tasks

- https://github.com/hadenlabs/tasks.git