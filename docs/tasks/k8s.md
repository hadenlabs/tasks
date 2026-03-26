# Kubernetes Module

Gestión de recursos Kubernetes.

## Tasks

### `k8s:destroy-stuck-ns`
Remueve finalizers de namespaces Kubernetes stuck en estado Terminating.

```bash
# Encuentra namespaces en estado Terminating
# Para cada namespace:
# 1. Obtiene definición en JSON
# 2. Remueve finalizers
# 3. Actualiza via kubectl proxy
# 4. Limpia archivos temporales
```

**Variables:**

| Variable | Default | Descripción |
|----------|---------|-------------|
| `PROXY_PORT` | 8001 | Puerto para kubectl proxy |

## Requisitos

- `kubectl` configurado con acceso al cluster
- Permisos para modificar namespaces