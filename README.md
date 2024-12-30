<!--


  ** DO NOT EDIT THIS FILE
  **
  ** 1) Make all changes to `provision/generator/README.yaml`
  ** 2) Run`task readme` to rebuild this file.
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **


  -->

[![Latest Release](https://img.shields.io/github/release/hadenlabs/tasks)](https://github.com/hadenlabs/tasks/releases) [![Lint](https://img.shields.io/github/workflow/status/hadenlabs/tasks/lint-code)](https://github.com/hadenlabs/tasks/actions?workflow=lint-code) [![CI](https://img.shields.io/github/workflow/status/hadenlabs/tasks/ci)](https://github.com/hadenlabs/tasks/actions?workflow=ci) [![Test](https://img.shields.io/github/workflow/status/hadenlabs/tasks/test)](https://github.com/hadenlabs/tasks/actions?workflow=test) [![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit) [![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow)](https://conventionalcommits.org) [![KeepAChangelog](https://img.shields.io/badge/changelog-Keep%20a%20Changelog%20v1.0.0-orange)](https://keepachangelog.com)

# tasks

# 📦 Taskfile Templates Repository

This repository contains reusable **Taskfile templates** to standardize and simplify common tasks like running pre-commit hooks, generating changelogs, creating GitHub releases, and more.

## Requirements

This is a list of plugins that need to be installed previously to enjoy all the goodies of this configuration:

- [gomplate](https://github.com/hairyhenderson/gomplate)
- [python](https://www.python.org)
- [taskfile](https://github.com/go-task/task)

## Usage

# 🚀 How to use this project

## Add the Remote Taskfiles

To use the Taskfile templates in your project, include the remote Taskfiles in your project's `Taskfile.yaml`:

```yaml
version: "3"

includes:
  pre-commit:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/pre-commit/Taskfile.yml"
  github:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/github/Taskfile.yml"
  changelog:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/changelog/Taskfile.yml"
  confluence:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/confluence/Taskfile.yml"
  python:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/python/Taskfile.yml"
  git:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/git/Taskfile.yml"
  docs:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/docs/Taskfile.yml"
  docker:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/docker/Taskfile.yml"
  version:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/version/Taskfile.yml"
  plantuml:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/plantuml/Taskfile.yml"
  prettier:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/prettier/Taskfile.yml"
  sonar:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/sonar/Taskfile.yml"
  keybase:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/keybase/Taskfile.yml"
  multipass:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/multipass/Taskfile.yml"
  diagrams:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/main/diagrams/Taskfile.yml"

env:
  DOCKER:
    sh: docker --version 2> /dev/null || echo "not exist"
  PYTHON:
    sh: python --version 2> /dev/null || echo "not exist"
  GO:
    sh: go version 2> /dev/null || echo "not exist"
  NODE:
    sh: node --version 2> /dev/null || echo "not exist"
  APP_TAG:
    sh: git describe --tags $(git rev-list --tags --max-count=1) 2> /dev/null || echo "0.0.0"
  README_YAML: provision/generators/README.yaml
  README_TEMPLATE: provision/templates/README.tpl.md
  README_INCLUDES: file://

vars:
  PROJECT_NAME: { { project } }
  GROUP_NAME: hadenlabs
  ORGANIZATION: hadenlabs
  DOCKER_PLATFORM: linux/amd64
  REVIEWERS: luismayta
  PYTHON_VERSION: 3.11.5
  NODE_VERSION: 18.18.2
  TERRAFORM_VERSION: 1.8.4
  GIT_IGNORES: python,node,go,zsh,sonar,java,maven,intellij+all,terraform,linux
  GOLANGCI_VERSION: 1.42.0
  README_FILE: README.md
  GIT_IGNORES_CUSTOM: |
    bin
    .scannerwork
    .secrets
    public
    TMP_CHANGELOG.md
    .task
    .terraform.lock.hcl
    *.lock.hcl
    *.zip
    .external_modules
    vendor

dotenv:
  - .env

tasks:
  default:
    deps:
      - task: check
    cmds:
      - cmd: echo Application {{.PROJECT_NAME}}
        silent: true
      - task: version:default
      - task: summary
      - cmd: task -l
    silent: true

  summary:
    desc: "Summary information"
    cmds:
      - echo Go available {{.GO}}
      - echo Python available {{.PYTHON}}
      - echo Docker available {{.DOCKER}}
      - echo Node available {{.NODE}}
    silent: true

  check:
    desc: "Check all dependencies"
    deps:
      - python:check
      - changelog:check
      - git:check
      - docs:check

  readme:
    run: once
    desc: Generate Readme
    silent: true
    cmds:
      - >-
        gomplate --file {{.README_TEMPLATE}} --out {{.README_FILE}} --datasource config={{.README_YAML}} --datasource includes={{.README_INCLUDES}}


      - task: prettier

  prettier:
    run: once
    desc: Execute prettier files
    cmds:
      - task: prettier:all

  upgrade:
    run: once
    desc: Execute upgrade packages
    cmds:
      - poetry update
      - poetry run pre-commit autoupdate

  setup:
    desc: Setup dependences of project
    cmds:
      - >-
        [ -e ".env" ] || cp -rf .env.example .env


      - task: python:setup
      - task: python:precommit
      - task: git:setup

  environment:
    desc: Setup environment of project
    cmds:
      - task: python:environment
```

## Examples

<!-- Space: Projects -->
<!-- Parent: Tasks -->
<!-- Title: Examples Tasks -->
<!-- Label: Examples -->
<!-- Include: ./../disclaimer.md -->
<!-- Include: ac:toc -->

### Common

## Help

**Got a question?**

File a GitHub [issue](https://github.com/hadenlabs/tasks/issues).

## Contributing

See [Contributing](./docs/contributing.md).

## Module Versioning

This Module follows the principles of [Semantic Versioning (SemVer)](https://semver.org/).

Using the given version number of `MAJOR.MINOR.PATCH`, we apply the following constructs:

1. Use the `MAJOR` version for incompatible changes.
1. Use the `MINOR` version when adding functionality in a backwards compatible manner.
1. Use the `PATCH` version when introducing backwards compatible bug fixes.

### Backwards compatibility in `0.0.z` and `0.y.z` version

- In the context of initial development, backwards compatibility in versions `0.0.z` is **not guaranteed** when `z` is increased. (Initial development)
- In the context of pre-release, backwards compatibility in versions `0.y.z` is **not guaranteed** when `y` is increased. (Pre-release)

## Copyright

Copyright © 2018-2024 [Hadenlabs](https://hadenlabs.com)

## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## License

The code and styles are licensed under the LGPL-3.0 license [See project license.](LICENSE).

## Don't forget to 🌟 Star 🌟 the repo if you like tasks

[Your feedback is appreciated](https://github.com/hadenlabs/tasks/issues)
