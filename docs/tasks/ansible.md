# Ansible Module

Gestión de infraestructura con Ansible y Vault.

## Tasks

### `ansible:check`
Verifica Ansible.

### Vault Commands

| Task | Descripción |
|------|-------------|
| `ansible:encrypt:inventory:core` | Encripta inventory core |
| `ansible:encrypt:inventory:dev` | Encripta inventory dev |
| `ansible:encrypt:inventory:staging` | Encripta inventory staging |
| `ansible:encrypt:inventory:prod` | Encripta inventory prod |
| `ansible:decrypt:inventory` | Desencripta inventory |
| `ansible:encrypt:vars` | Encripta variables |
| `ansible:decrypt:vars` | Desencripta variables |

### Collections y Roles

| Task | Descripción |
|------|-------------|
| `ansible:collection` | Instala/actualiza collections |
| `ansible:update:core` | Instala roles stage core |
| `ansible:update:sandbox` | Instala roles stage sandbox |
| `ansible:update:dev` | Instala roles stage dev |
| `ansible:update:staging` | Instala roles stage staging |
| `ansible:update:prod` | Instala roles stage prod |

### Playbooks

| Task | Descripción |
|------|-------------|
| `ansible:playbook:sandbox` | Ejecuta playbook sandbox |
| `ansible:playbook:dev` | Ejecuta playbook dev |
| `ansible:playbook:staging` | Ejecuta playbook staging |
| `ansible:playbook:prod` | Ejecuta playbook prod |
| `ansible:management` | Ejecuta playbook de gestión |

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `KEYBASE_PROJECT_PATH` | Ruta base del proyecto |
| `ANSIBLE_PATH` | Ruta de archivos Ansible |
| `ANSIBLE_COLLECTIONS` | Collections a instalar |
| `STAGE` | Ambiente |
| `INVENTORY` | Inventory a usar |
| `USER` | Usuario SSH |
| `TAGS` | Tags de Ansible |

## Dependencias

- `ansible`
- `ansible-vault`
- `ansible-galaxy`