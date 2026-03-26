# GitHub Module

Automatización de GitHub CLI.

## Tasks

### `github:add-git-submodules`
Agrega submodules desde GitHub basados en un patrón.

**Variables requeridas:**
- `ORG`: Organización GitHub
- `REGEXP`: Patrón regex para buscar repos
- `TARGET_DIR`: Directorio destino

```bash
export TASK_CONFIRM="y"
repo_url=$(gh repo view "{{.ORG}}/{{.REGEXP}}" --json url -q .url)
git submodule add "$repo_url" "{{.TARGET_DIR}}/{{.REGEXP}}"
git submodule update --init --recursive
```

### `github:remove-git-submodules`
Remueve submodules y limpia archivos relacionados.

**Variables requeridas:**
- `TARGET_DIR`: Directorio de los submodules
- `REGEXP`: Patrón para filtrar submodules

### `github:create-release`
Crea un release en GitHub.

**Variables requeridas:**
- `NEXT_VERSION`: Versión del release (ej: 1.0.0)

```bash
gh release create $NEXT_VERSION -F changelogs/CHANGELOG.rst
```

### `github:create-release-with-notes`
Crea release con notas generadas automáticamente.

```bash
gh release create "$NEXT_VERSION" --generate-notes
```

### `github:merge-pull-requests`
Squash y merge de PRs elegibles, cierra PRs conflictivos.

### `github:diff-changes`
Muestra diff excluyendo patrones específicos.

**Variables requeridas:**
- `BASE_BRANCH`: Rama base para diff
- `EXCLUDES`: Patrones a excluir (comma-separated)

## Dependencias

- `gh` (GitHub CLI)