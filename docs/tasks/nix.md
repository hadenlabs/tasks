# Nix Module

Gestión de Nix con [nix](https://nixos.org/), [nixfmt](https://github.com/NixOS/nixfmt), [statix](https://github.com/oppiliappan/statix) y flakes.

## Tasks

### `nix:default`

Lista todas las tareas públicas del módulo.

### `nix:check`

Verifica todas las dependencias (agregador de `check:nix`).

### `nix:check:nix`

Verifica que `nix` está instalado.

### `nix:check:flake`

Verifica que existe `flake.nix` en la raíz del proyecto.

### `nix:check:direnv`

Verifica que `direnv` está instalado.

### `nix:fmt`

Formatea archivos Nix. Usa `nix fmt` si existe `flake.nix`, si no `nixfmt`.

```bash
nix fmt {{.CLI_ARGS}}
nixfmt . {{.CLI_ARGS}}
```

### `nix:lint`

Linting de archivos Nix con `statix`.

```bash
statix check . {{.CLI_ARGS}}
```

### `nix:fix`

Corrige problemas de lint de Nix con `statix`.

```bash
statix fix . {{.CLI_ARGS}}
```

### `nix:test`

Ejecuta `nix flake check`.

```bash
nix flake check
```

### `nix:show`

Muestra los outputs del flake.

```bash
nix flake show
```

### `nix:lock`

Bloquea las entradas del flake.

```bash
nix flake lock
```

### `nix:update`

Actualiza las entradas del flake.

```bash
nix flake update
```

### `nix:develop`

Entra al shell de desarrollo de Nix.

```bash
nix develop
```

### `nix:run`

Ejecuta una orden dentro del shell de desarrollo de Nix.

```bash
nix develop --command {{.CMD}}
```

```bash
task nix:run CMD="task test"
```

### `nix:direnv`

Permite direnv para el proyecto.

```bash
direnv allow
```

### `nix:install`

Instala Nix si no está presente (instalador DeterminateSystems).

```bash
if command -v nix >/dev/null 2>&1; then
  echo "nix is already installed"
else
  command -v curl >/dev/null 2>&1 || { echo "curl is required to install Nix"; exit 1; }
  curl -fsSL https://install.determinate.systems/nix | sh -s -- install
fi
```

The installer runs the official DeterminateSystems script via curl|sh without checksum verification — standard practice for Nix installs; verify https://install.determinate.systems/ if your threat model requires it.

### `nix:reinstall`

Reinstala `nixfmt` y `statix` en el perfil de Nix.

```bash
nix profile remove nixpkgs#nixfmt nixpkgs#statix 2>/dev/null || true
nix profile install nixpkgs#nixfmt nixpkgs#statix
```

### `nix:gc`

Recolecta basura de Nix.

```bash
nix-collect-garbage
```

### `nix:setup`

Configura el tooling de Nix (depende de `check:nix` y `_setup`).

## Dependencias

- `nix`
- `nixfmt`
- `statix`
- `direnv` (opcional, para flakes)