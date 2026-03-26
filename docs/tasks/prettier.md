# Prettier Module

Automatic code formatting with [Prettier](https://prettier.io).

## Tasks

### `prettier:check`
Verify `prettier` is installed.

### `prettier:all`
Format all supported files.

```bash
prettier --config=.ci/linters/prettier.config.cjs \
  --ignore-path=.ci/linters/.prettierignore \
  '**/*.{js?(on),scss,less,ts?(x),md,y?(a)ml,gql,graphql?(s),mjml}' \
  --write=true --list-different
```

### `prettier:code`
Format code files.

```bash
# Extensions: js, ts, jsx, tsx, md, yml, yaml, gql, graphql, mjml
```

### `prettier:sass`
Format Sass/SCSS files.

```bash
# Extensions: *.scss
```

### `prettier:less`
Format Less files.

```bash
# Extensions: *.less
```

## Configuration

- Config: `.ci/linters/prettier.config.cjs`
- Ignore: `.ci/linters/.prettierignore`

## Dependencies

- `prettier`