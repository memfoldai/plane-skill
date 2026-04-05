---
name: plane-cli
description: >-
  Manage Plane project management resources via the @memfoldai/plane-cli
  command-line tool. Covers projects, work items (issues), cycles (sprints),
  modules, initiatives, epics, milestones, labels, states, work logs, pages,
  intake items, work item types, work item properties, and workspace settings.

  Use when the user asks to:
  - create, list, update, delete, or search Plane work items or issues
  - manage Plane projects, cycles, sprints, modules, or initiatives
  - add or remove work items from cycles, modules, or milestones
  - create or manage epics, milestones, labels, or states in Plane
  - log time, add comments, links, or relations to work items
  - query workspace members or project features
  - interact with the Plane project management API via CLI
---

# Plane CLI

CLI for the [Plane](https://plane.so) project management API, providing ~113 commands across all resource types.

## Prerequisites

Install globally or use via npx:

```bash
npm install -g @memfoldai/plane-cli
# or
npx @memfoldai/plane-cli <command>
```

Required environment variables:

```bash
export PLANE_API_KEY="<personal-access-token>"      # Settings > API Tokens in Plane
export PLANE_WORKSPACE_SLUG="<workspace-slug>"
```

For self-hosted Plane instances:

```bash
export PLANE_BASE_URL="https://your-instance.com/api-key/mcp"
```

Verify setup:

```bash
plane-cli get-me
```

## Core Workflow

Most commands follow a pattern: list resources to discover UUIDs, then use those UUIDs in subsequent commands.

### 1. Discover project IDs

```bash
plane-cli list-projects --output json
```

### 2. Use project ID in downstream commands

```bash
plane-cli list-work-items --project-id <uuid>
plane-cli create-work-item --project-id <uuid> --name "Fix login bug"
```

### 3. Look up work items by human-readable identifier

```bash
plane-cli retrieve-work-item-by-identifier --project-identifier MP --issue-identifier 42
```

### Output formats

All commands support `--output <format>`:
- `text` (default) - human-readable
- `json` - structured, best for piping/parsing
- `markdown` - formatted tables
- `raw` - full MCP envelope

### Timeout

Override the default 30s timeout for slow operations:

```bash
plane-cli list-work-items --project-id <uuid> --timeout 60000
```

### Raw JSON passthrough

Any command accepts `--raw <json>` to pass the full request body directly, useful for fields not exposed as individual flags:

```bash
plane-cli create-work-item --raw '{"project_id":"<uuid>","name":"Task","priority":"high","description_html":"<p>Details</p>"}'
```

## Command Categories

### Projects (9 commands)

`list-projects`, `create-project`, `retrieve-project`, `update-project`, `delete-project`, `get-project-members`, `get-project-features`, `update-project-features`, `get-project-worklog-summary`

```bash
plane-cli create-project --name "Backend" --identifier "BE"
plane-cli get-project-members --project-id <uuid>
```

### Work Items (7 commands)

`list-work-items`, `create-work-item`, `retrieve-work-item`, `retrieve-work-item-by-identifier`, `update-work-item`, `delete-work-item`, `search-work-items`

```bash
plane-cli create-work-item --project-id <uuid> --name "Bug: login fails" --assignees <user-uuid>
plane-cli search-work-items --query "authentication"
plane-cli list-work-items --project-id <uuid> --priorities urgent,high --state-groups started
```

`list-work-items` supports advanced filtering: `--assignee-ids`, `--state-ids`, `--state-groups` (backlog/unstarted/started/completed/cancelled), `--priorities` (urgent/high/medium/low/none), `--label-ids`, `--type-ids`, `--cycle-ids`, `--module-ids`, `--created-by-ids`, `--query`.

### Work Item Sub-resources

- **Activities** (2): `list-work-item-activities`, `retrieve-work-item-activity`
- **Comments** (5): `list-work-item-comments`, `create-work-item-comment`, `retrieve-work-item-comment`, `update-work-item-comment`, `delete-work-item-comment`
- **Links** (5): `list-work-item-links`, `create-work-item-link`, `retrieve-work-item-link`, `update-work-item-link`, `delete-work-item-link`
- **Relations** (3): `list-work-item-relations`, `create-work-item-relation`, `remove-work-item-relation`

Relation types: blocking, blocked_by, duplicate, relates_to, start_before, start_after, finish_before, finish_after.

```bash
plane-cli create-work-item-comment --project-id <uuid> --work-item-id <uuid> --comment-html "<p>Deployed to staging</p>"
plane-cli create-work-item-relation --project-id <uuid> --work-item-id <uuid> --relation-type blocked_by --issues <blocker-uuid>
```

### Work Logs (4 commands)

`list-work-logs`, `create-work-log`, `update-work-log`, `delete-work-log`

```bash
plane-cli create-work-log --project-id <uuid> --work-item-id <uuid> --duration 120 --description "Debugging auth flow"
```

### Cycles (12 commands)

`list-cycles`, `create-cycle`, `retrieve-cycle`, `update-cycle`, `delete-cycle`, `list-archived-cycles`, `add-work-items-to-cycle`, `remove-work-item-from-cycle`, `list-cycle-work-items`, `transfer-cycle-work-items`, `archive-cycle`, `unarchive-cycle`

```bash
plane-cli create-cycle --project-id <uuid> --name "Sprint 1" --owned-by <uuid> --start-date 2025-01-01 --end-date 2025-01-14
plane-cli add-work-items-to-cycle --project-id <uuid> --cycle-id <uuid> --issue-ids <id1>,<id2>
plane-cli transfer-cycle-work-items --project-id <uuid> --cycle-id <old> --new-cycle-id <new>
```

### Modules (11 commands)

`list-modules`, `create-module`, `retrieve-module`, `update-module`, `delete-module`, `list-archived-modules`, `add-work-items-to-module`, `remove-work-item-from-module`, `list-module-work-items`, `archive-module`, `unarchive-module`

```bash
plane-cli create-module --project-id <uuid> --name "Auth Module" --start-date 2025-01-01 --target-date 2025-03-01
```

### Initiatives (5 commands)

Workspace-level, no project ID needed.

`list-initiatives`, `create-initiative`, `retrieve-initiative`, `update-initiative`, `delete-initiative`

```bash
plane-cli create-initiative --name "Q1 Goals" --start-date 2025-01-01 --end-date 2025-03-31
```

### Epics (5 commands)

`list-epics`, `create-epic`, `retrieve-epic`, `update-epic`, `delete-epic`

```bash
plane-cli create-epic --project-id <uuid> --name "User Authentication Overhaul" --priority high
```

### Milestones (8 commands)

`list-milestones`, `create-milestone`, `retrieve-milestone`, `update-milestone`, `delete-milestone`, `add-work-items-to-milestone`, `remove-work-items-from-milestone`, `list-milestone-work-items`

```bash
plane-cli create-milestone --project-id <uuid> --title "v2.0 Release" --target-date 2025-06-01
plane-cli add-work-items-to-milestone --project-id <uuid> --milestone-id <uuid> --issue-ids <id1>,<id2>
```

### Labels & States

- **Labels** (5): `list-labels`, `create-label`, `retrieve-label`, `update-label`, `delete-label`
- **States** (5): `list-states`, `create-state`, `retrieve-state`, `update-state`, `delete-state`

State groups: backlog, unstarted, started, completed, cancelled.

```bash
plane-cli create-label --project-id <uuid> --name "bug" --color "#ef4444"
plane-cli create-state --project-id <uuid> --name "In Review" --color "#f59e0b"
```

### Work Item Types & Properties

- **Types** (5): `list-work-item-types`, `create-work-item-type`, `retrieve-work-item-type`, `update-work-item-type`, `delete-work-item-type`
- **Properties** (5): `list-work-item-properties`, `create-work-item-property`, `retrieve-work-item-property`, `update-work-item-property`, `delete-work-item-property`

Property types: TEXT, DATETIME, DECIMAL, BOOLEAN, OPTION, RELATION, URL, EMAIL, FILE.

### Pages (4 commands)

`retrieve-workspace-page`, `create-workspace-page`, `retrieve-project-page`, `create-project-page`

### Intake Work Items (5 commands)

`list-intake-work-items`, `create-intake-work-item`, `retrieve-intake-work-item`, `update-intake-work-item`, `delete-intake-work-item`

### Workspace (4 commands)

`get-me`, `get-workspace-members`, `get-workspace-features`, `update-workspace-features`

## Full Command Reference

For complete flag details, parameters, and additional examples for every command, see [references/commands.md](references/commands.md).
