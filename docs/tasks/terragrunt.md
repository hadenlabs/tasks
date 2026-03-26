# Terragrunt Module

Gestión de Terraform con [Terragrunt](https://terragrunt.gruntwork.io).

## Tasks

### `terragrunt:check`
Verifica Terragrunt y Terraform.

### `terragrunt:fmt`
Formatea archivos HCL.

```bash
terragrunt hcl format
```

### `terragrunt:setup`
Instala y configura Terraform.

```bash
tfenv install {{.TERRAFORM_VERSION}}
tfenv use {{.TERRAFORM_VERSION}}
```

### `terragrunt:init`
Inicializa Terragrunt.

### `terragrunt:plan`
Genera plan de cambios.

### `terragrunt:apply`
Aplica cambios.

### `terragrunt:destroy`
Destruye recursos.

### `terragrunt:refresh`
Refresca estado.

### `terragrunt:unlock`
Desbloquea state.

```bash
terragrunt force-unlock {{.LOCK_ID}}
```

### Módulos

| Task | Descripción |
|------|-------------|
| `terragrunt:module:apply` | Aplica módulo específico |
| `terragrunt:module:destroy` | Destruye módulo específico |

### Commands

| Task | Descripción |
|------|-------------|
| `terragrunt:import` | Importa recurso |
| `terragrunt:state` | State commands |
| `terragrunt:output` | Muestra outputs |

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `AWS_PROFILE_NAME` | Perfil AWS |
| `ORGANIZATION` | Organización |
| `TERRAFORM_PATH` | Ruta Terraform |
| `TERRAFORM_VERSION` | Versión Terraform |
| `STAGE` | Ambiente |
| `REGION` | Región (default: us-east-1) |

## Dependencias

- `terragrunt`
- `terraform`
- `tfenv`