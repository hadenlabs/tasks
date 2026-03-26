# Skills Module

Gestión de skills para OpenCode usando openskills.

## Tasks

### `skills:check`
Verifica que Bun esté instalado.

### `skills:setup`
Instala skills desde repositorios configurados.

```bash
for repository in {{.SKILL_REPOS}}; do
  bunx -y openskills install $repository --yes
done
```

### `skills:sync`
Sincroniza skills instalados.

```bash
bunx -y openskills sync --yes
```

## Variables

| Variable | Descripción |
|----------|-------------|
| `SKILL_REPOS` | Repositorios de skills a instalar |

Repositorios por defecto:
- `https://gitlab.infosisglobal.com/architecture/skills.git`
- `https://github.com/sanjay3290/ai-skills`

## Dependencias

- `bun`