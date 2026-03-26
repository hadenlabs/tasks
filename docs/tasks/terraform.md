# Terraform Module

Gestión y documentación de Terraform con [tfenv](https://github.com/tfutils/tfenv).

## Tasks

### `terraform:check`
Verifica que `terraform` y `tfenv` estén instalados.

### `terraform:environment`
Configura la versión de Terraform usando tfenv.

```bash
tfenv use {{ .TERRAFORM_VERSION }} || tfenv install {{ .TERRAFORM_VERSION }} && tfenv use {{ .TERRAFORM_VERSION }}
```

### `terraform:fmt`
Formatea archivos Terraform.

```bash
terraform fmt -recursive
```

### `terraform:doc`
Genera documentación con terraform-docs.

```bash
terraform-docs {{.TERRAFORM_PATH}} --config=.ci/config/.terraform-docs.yml
```

### `terraform:docs:all`
Genera documentación para todos los ambientes (core, prod, staging, dev).

## Docs por Ambiente

| Task | Stage | Region |
|------|-------|--------|
| `terraform:docs:core` | core | - |
| `terraform:docs:prod` | prod | us-east-1 |
| `terraform:docs:staging` | staging | us-east-1 |
| `terraform:docs:dev` | dev | us-east-1 |

## Variables Requeridas

| Variable | Descripción |
|----------|-------------|
| `AWS_PROFILE_NAME` | Perfil AWS |
| `TERRAFORM_PATH` | Ruta de archivos Terraform |
| `TERRAFORM_VERSION` | Versión de Terraform (default: 1.11.4) |

## Dependencias

- `terraform`
- `tfenv`
- `terraform-docs` (para documentación)
