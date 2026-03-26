# Multipass Module

Gestión de VMs con [Multipass](https://multipass.run).

## Tasks

### `multipass:check`
Verifica Multipass.

### `multipass:make:path`
Crea directorio de build.

### `multipass:launch`
Lanza VM.

```bash
multipass launch --verbose \
  --name {{.MULTIPASS_LAUNCH_NAME}} \
  --cloud-init {{.MULTIPASS_CLOUD_INIT_FILE}} \
  {{.MULTIPASS_ARGS}}
```

### `multipass:launch:k8s:master`
Lanza K8s master con Calico.

### `multipass:launch:k8s:node`
Lanza K8s worker node.

### `multipass:launch:traefik`
Despliega Traefik ingress.

### `multipass:make:certificates`
Genera certificados SSL.

```bash
mkcert '*.k8s.local'
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `BUILD_PATH` | Directorio de build |
| `MULTIPASS_LAUNCH_NAME` | Nombre de VM |
| `MULTIPASS_CLOUD_INIT_FILE` | Cloud-init config |
| `MULTIPASS_ARGS` | Args adicionales |
| `KUBECONFIG` | Path kubeconfig |
| `MOUNT_PATH_K8S_LOCAL` | Path a montar |

## Dependencias

- `multipass`
- `mkcert` (para certificados)
- `kubectl` (para K8s)