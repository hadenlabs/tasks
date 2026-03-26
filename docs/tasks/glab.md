# glab

Generated from: `src/glab/Taskfile.yml`
---

## Tasks

| Task | Description |
| ---- | ----------- |
| `check` | Exist glab and dependencies |
| `check:glab` | Exist glab CLI |
| `setup` | Setup glab configuration |
| `version` | Show glab version |
| `auth:status` | Show glab authentication status |
| `auth:login` | Login to GitLab with glab |
| `auth:logout` | Logout from GitLab |
| `mr:list` | List merge requests |
| `mr:view` | View a merge request |
| `mr:create` | Create a merge request |
| `mr:approve` | Approve a merge request |
| `issue:list` | List issues |
| `issue:view` | View an issue |
| `issue:create` | Create an issue |
| `ci:list` | List CI/CD pipelines |
| `ci:view` | View a CI/CD pipeline |
| `ci:run` | Trigger a CI/CD pipeline |
| `repo:view` | View current repository information |
| `repo:fork` | Fork a repository |
| `config:list` | Show glab configuration |

## Task Details

### check

Exist glab and dependencies

**Dependencies:**
- task: check:glab

### check:glab

Exist glab CLI

**Preconditions:**
- Please Install glab CLI: https://gitlab.com/gitlab-org/cli

### setup

Setup glab configuration

**Commands:**
```bash
# Check if glab is already authenticated
if ! glab auth status &>/dev/null; then
  echo "glab not authenticated. Run 'glab auth login' to authenticate."
  glab auth login
else
  echo "glab already authenticated"
fi

```

**Dependencies:**
- task: check

### version

Show glab version

**Commands:**
```bash
glab --version
```

**Dependencies:**
- task: check

### auth:status

Show glab authentication status

**Commands:**
```bash
glab auth status
```

**Dependencies:**
- task: check

### auth:login

Login to GitLab with glab

**Commands:**
```bash
glab auth login
```

**Dependencies:**
- task: check

### auth:logout

Logout from GitLab

**Commands:**
```bash
glab auth logout
```

**Dependencies:**
- task: check

### mr:list

List merge requests

**Commands:**
```bash
glab mr list {{.FLAGS}}

```

**Dependencies:**
- task: check

### mr:view

View a merge request

**Commands:**
```bash
glab mr view {{.MR_NUMBER}}
```

**Dependencies:**
- task: check

### mr:create

Create a merge request

**Commands:**
```bash
glab mr create {{.FLAGS}}

```

**Dependencies:**
- task: check

### mr:approve

Approve a merge request

**Commands:**
```bash
glab mr approve {{.MR_NUMBER}}
```

**Dependencies:**
- task: check

### issue:list

List issues

**Commands:**
```bash
glab issue list {{.FLAGS}}

```

**Dependencies:**
- task: check

### issue:view

View an issue

**Commands:**
```bash
glab issue view {{.ISSUE_NUMBER}}
```

**Dependencies:**
- task: check

### issue:create

Create an issue

**Commands:**
```bash
glab issue create {{.FLAGS}}

```

**Dependencies:**
- task: check

### ci:list

List CI/CD pipelines

**Commands:**
```bash
glab ci list
```

**Dependencies:**
- task: check

### ci:view

View a CI/CD pipeline

**Commands:**
```bash
glab ci view {{.PIPELINE_ID}}
```

**Dependencies:**
- task: check

### ci:run

Trigger a CI/CD pipeline

**Commands:**
```bash
glab ci run {{.FLAGS}}

```

**Dependencies:**
- task: check

### repo:view

View current repository information

**Commands:**
```bash
glab repo view
```

**Dependencies:**
- task: check

### repo:fork

Fork a repository

**Commands:**
```bash
glab repo fork
```

**Dependencies:**
- task: check

### config:list

Show glab configuration

**Commands:**
```bash
glab config list
```

**Dependencies:**
- task: check