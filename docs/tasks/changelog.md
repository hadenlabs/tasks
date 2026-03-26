# Changelog Module

Generación de changelogs usando [git-chglog](https://github.com/git-chglog/git-chglog).

## Tasks

### `changelog:check`
Verifica que `git-chglog` y `go` estén instalados.

### `changelog:install`
Instala `git-chglog` via Go.

```bash
go install github.com/git-chglog/git-chglog/cmd/git-chglog@v0.15
```

### `changelog:generate`
Genera el changelog completo.

```bash
git-chglog -o CHANGELOG.md
```

### `changelog:tag`
Genera el changelog para un tag específico.

```bash
git-chglog {{.APP_TAG}} > {{.TMP_CHANGELOG_FILENAME}}
```

### `changelog:next`
Genera el changelog para el próximo tag.

```bash
git-chglog --next-tag {{.APP_TAG}} --output CHANGELOG.md
```

## Dependencias

- `go` (para instalar git-chglog)
- `git-chglog` (para generar changelogs)