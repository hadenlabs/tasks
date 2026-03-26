<!-- Space: Projects -->
<!-- Parent: Tasks -->
<!-- Title: Contributing Tasks -->
<!-- Label: Tasks -->
<!-- Label: Contributing -->
<!-- Include: disclaimer.md -->
<!-- Include: ac:toc -->

# How To Contribute

Contributions to tasks are welcome.

Feel free to use all of the contribution options:

- Contribute to tasks repositories on [GitHub](https://github.com/hadenlabs/tasks). See [GitHub flow](./contribute/github-flow.md).

## Getting Started

### Development

In general, MRs are welcome. We follow the typical "fork-and-pull" [Github flow](./contribute/github-flow.md).

1. **Fork** the repo on Github
2. **Clone** the project to your own machine
3. **Commit** changes to your own branch using [Github Flow](./contribute/github-flow.md)
4. **Push** your work back up to your fork

5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to rebase the latest changes from "upstream" before making a pull request!

## Styleguides

### Git Commit Messages

The commit conventions for this repository are defined in `.goji.json`. This file serves as the source of truth for:

- **Allowed commit types** and their corresponding emoji
- **Allowed scopes** (e.g., `core`, `accounts`, `ci`)
- **Sign-off requirements** (required in this repo)
- **Maximum subject length** (100 characters)

While this repo follows the spirit of [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0), the exact convention is defined by `.goji.json`. Refer to that file for the authoritative list of types, scopes, and emoji.

#### Format

```
<type> <emoji> (<scope>): <subject>
```

#### Examples (aligned with current .goji.json)

- `feat ✨ (core): add new authentication module`
- `fix 🐛 (accounts): resolve login timeout issue`
- `docs 📚 (core): update API documentation`
- `refactor 🎨 (ci): simplify build pipeline logic`
- `ci 👷 (ci): add GitHub Actions workflow`

#### Guidelines

- Use only the types and scopes configured in `.goji.json` — do not invent custom types or scopes
- Use only the emoji defined for each type in `.goji.json`
- Keep the subject concise and within the maximum length (100 characters)
- Include a sign-off (`-s` flag) when committing, as sign-off is required by this repo
- Focus the subject on what changed and why

**Note:** `infobot.toml` configures Jira issue and branch integration separately from commit conventions.

### Branches

See [Github Flow](./contribute/github-flow.md).

### Documentation

Documentation is a part of the tasks code base. You can find the documentation files in the `doc/` subdirectory of the [main repository](https://github.com/hadenlabs/tasks). This means that the contribution process is the same for both the source code and documentation.

### Testing

See [Testing](./testing.md).

### Code Submission

1.  See if a [Pull Request](https://github.com/hadenlabs/tasks/pulls) exists
    - Add some comments or review the code to help it along
    - Don\'t be afraid to comment when logic needs clarification
2.  Create a Fork and open a [Pull Request](https://github.com/hadenlabs/tasks/pulls) if needed

### Code Review

- Anyone can review code
- Any [Pull Request](https://github.com/hadenlabs/tasks/pulls) should be closed or merged within a week

### Code Acceptance

Try to keep history as linear as possible using a [rebase] merge strategy.
