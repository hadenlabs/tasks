# Git Module

Configuración y automatización de Git.

## Tasks

### `git:check`
Verifica que `git` esté instalado.

### `git:setup`
Configura Git: genera `.gitignore` y configura reviewers.

```bash
task git:ignore
task git:reviews
```

### `git:ignore`
Genera `.gitignore` usando [gitignore.io](https://gitignore.io) con patrones configurados.

```bash
curl -sL https://www.toptal.com/developers/gitignore/api/"{{.GIT_IGNORES}}"
echo "# custom git ignore files" >> .gitignore
echo "{{.GIT_IGNORES_CUSTOM}}" >> .gitignore
```

### `git:reviews`
Configura reviewers por defecto para el repositorio.

```bash
git config github.reviews {{.REVIEWERS}}
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `GIT_IGNORES` | Patrones de gitignore.io (python,node,go,zsh,sonar,java,maven,intellij+all,node,helm,terraform) |
| `GIT_IGNORES_CUSTOM` | Patrones adicionales personalizados |
| `REVIEWERS` | Usuarios default para reviews |

## Dependencias

- `git`
- `curl` (para gitignore.io)