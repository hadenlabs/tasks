## ADDED Requirements

### Requirement: Graphify CLI availability check

The system SHALL verify that the `graphify` command is available before running graphify tasks.

#### Scenario: Graphify is installed

- **WHEN** user runs `task ai:check:graphify`
- **THEN** system verifies `graphify` command exists and exits successfully

#### Scenario: Graphify is not installed

- **WHEN** user runs `task ai:check:graphify` and graphify is not installed
- **THEN** system displays error message "Please Install graphify" and exits with error

### Requirement: Graphify environment installation

The system SHALL install graphify via `uv tool install graphifyy` if not already present.

#### Scenario: Fresh installation

- **WHEN** user runs `task ai:environment:graphify` and graphify is not installed
- **THEN** system runs `uv tool install graphifyy` silently

#### Scenario: Already installed

- **WHEN** user runs `task ai:environment:graphify` and graphify is already installed
- **THEN** system skips installation silently

### Requirement: Graphify setup with OpenCode platform

The system SHALL register graphify skill with OpenCode and handle initialization.

#### Scenario: First-time setup

- **WHEN** user runs `task ai:setup:graphify` and graphify is not configured
- **THEN** system runs `graphify install --platform opencode --project`

#### Scenario: Existing setup

- **WHEN** user runs `task ai:setup:graphify` and graphify is already configured
- **THEN** system updates graphify configuration

### Requirement: Graph initialization

The system SHALL initialize a knowledge graph for the current project.

#### Scenario: Initialize new graph

- **WHEN** user runs `task ai:graphify:init`
- **THEN** system runs `graphify extract .` to build the initial graph

### Requirement: Graph synchronization

The system SHALL update an existing knowledge graph with recent changes.

#### Scenario: Update existing graph

- **WHEN** user runs `task ai:graphify:sync`
- **THEN** system runs `graphify extract . --update` to refresh changed files

### Requirement: Graph query operations

The system SHALL support querying the knowledge graph for code understanding.

#### Scenario: Query graph

- **WHEN** user runs `task ai:graphify:query Q="question"`
- **THEN** system runs `graphify query "question"` and returns results

#### Scenario: Explain concept

- **WHEN** user runs `task ai:graphify:explain CONCEPT="concept"`
- **THEN** system runs `graphify explain "concept"` and returns explanation

#### Scenario: Find path between concepts

- **WHEN** user runs `task ai:graphify:path SOURCE="A" TARGET="B"`
- **THEN** system runs `graphify path "A" "B"` and returns shortest path

### Requirement: Git hook installation

The system SHALL install git hooks for automatic graph rebuild on commit.

#### Scenario: Install hooks

- **WHEN** user runs `task ai:graphify:hook`
- **THEN** system runs `graphify hook install` to enable auto-rebuild

### Requirement: Combined setup task

The system SHALL provide a single task that performs complete graphify setup.

#### Scenario: Full setup

- **WHEN** user runs `task ai:setup:graphify`
- **THEN** system runs check, environment, and setup tasks in sequence