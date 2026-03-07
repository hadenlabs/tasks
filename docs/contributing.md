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

Your commit messages should serve these 3 important purposes:

- To speed up the reviewing process.
- To provide the least amount of necessary documentation
- To help the future maintainers.

Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0) to make `git log`{.interpreted-text role="command"} a little easier to follow. We use commitlint enforcing conventional commits (See more [here](https://github.com/conventional-changelog/commitlint))

**chore**: something just needs to happen, e.g. versioning

**docs**: documentation pages in `docs/` or docstrings

**feat**: new code in `./`

**fix**: code improvement in `./`

**refactor**: code movement in `./`

**style**: aesthetic changes

**test**: test case modifications in `test/`

Examples commit messages:

- chore: IN-698 implement model devices
- docs: IN-698 implement configuration settings
- feat: IN-698 create lambda function
- fix: IN-698 retry upload on failure
- refactor: IN-698 extract duplicate code
- style: IN-698 format files python
- test: IN-698 coverage around add permissions

**Keep it short and simple!**

### Branches

See [Github Flow](./contribute/github-flow.md).

### Documentation

Documentation is a part of the tasks code base. You can find the documentation files in the `doc/` subdirectory of the [main repository](https://github.com/hadenlabs/tasks). This means that the contribution process is the same for both the source code and documentation.

### Testing

See [Testing](./testing.md).

### Prompt templates

This repository includes prompt templates to help implement and maintain Taskfile templates (`src/*/Taskfile.yml`) consistently.

- Add a new tool Taskfile template:
  - Prompt: `provision/prompts/task/scaffold-template.prompt.md`
  - Output typically includes:
    - `src/<tool>/Taskfile.yml`
    - `Taskfile.yml` include registration
    - `docs/usage.md` include example update

- Update an existing tool Taskfile template:
  - Prompt: `provision/prompts/task/<tool>/implement-taskfile-v1.prompt.md`

General workflow:

1. Choose the prompt that matches your change.
2. Replace placeholders (e.g., `{{tool_name}}`).
3. Run the prompt in your coding agent and apply the changes to the repo.
4. Validate with:

```bash
task validate
```

Prompt conventions:

- Folder name matches the tool directory under `src/` (e.g., `src/terraform` -> `provision/prompts/task/terraform/`).
- Keep prompts ASCII and versioned (`*-v1.prompt.md`). If you need to change behavior in a breaking way, add `v2` instead of rewriting history.

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
