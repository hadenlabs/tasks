# 🚀 How to use this project

## Add the Remote Taskfiles

To use the Taskfile templates in your project, include the remote Taskfiles in your project's `Taskfile.yaml`:

```yaml
version: "3"

includes:
  pre-commit:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/pre-commit/Taskfile.yml"
  github:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/github/Taskfile.yml"
  changelog:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/changelog/Taskfile.yml"
  confluence:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/confluence/Taskfile.yml"
  node:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/node/Taskfile.yml"
  python:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/python/Taskfile.yml"
  pnpm:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/pnpm/Taskfile.yml"
  terraform:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/terraform/Taskfile.yml"
  terragrunt:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/terragrunt/Taskfile.yml"
  git:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/git/Taskfile.yml"
  docs:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/docs/Taskfile.yml"
  docker:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/docker/Taskfile.yml"
  version:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/version/Taskfile.yml"
  plantuml:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/plantuml/Taskfile.yml"
  prettier:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/prettier/Taskfile.yml"
  sonar:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/sonar/Taskfile.yml"
  keybase:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/keybase/Taskfile.yml"
  multipass:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/multipass/Taskfile.yml"
  ssh:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/ssh/Taskfile.yml"
  openssl:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/openssl/Taskfile.yml"
  diagrams:
    taskfile: "https://raw.githubusercontent.com/hadenlabs/tasks/refs/heads/main/src/diagrams/Taskfile.yml"

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
  GROUP_NAME: { { group_name } }
  ORGANIZATION: { { organization } }
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
