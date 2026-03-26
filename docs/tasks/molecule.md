# Molecule Module

Testing de roles Ansible con [Molecule](https://molecule.readthedocs.io).

## Tasks

### `molecule:check`
Verifica Molecule.

### `molecule:test`
Ejecuta test para escenario específico.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run molecule test --scenario-name={{.SCENARIO}} --destroy=always
```

### `molecule:test:all`
Ejecuta todos los escenarios.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run molecule test --all --destroy=always
```

### `molecule:scenario`
Inicializa escenario.

```bash
{{ .PYTHON_PACKAGE_MANAGER }} run molecule init scenario {{.SCENARIO}} \
  --driver-name=ec2 \
  --provisioner-name=ansible \
  --role-name={{ .PROJECT_NAME }}
```

### `molecule:scenario:test`
Ejecuta test con escenario.

## Variables

| Variable | Descripción |
|----------|-------------|
| `SCENARIO` | Nombre del escenario |
| `PROJECT_NAME` | Nombre del rol |

## Dependencias

- `molecule`
- `ansible`