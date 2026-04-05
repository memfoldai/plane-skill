# Plane CLI Command Reference

Complete reference for all `plane-cli` commands, organized by resource type.

All commands accept global flags: `--output <text|markdown|json|raw>` and `--timeout <ms>`.
Pass `--raw <json>` on any command to supply the full request body as JSON, bypassing individual flags.

---

## Table of Contents

- [Projects](#projects)
- [Work Items](#work-items)
- [Work Item Activities](#work-item-activities)
- [Work Item Comments](#work-item-comments)
- [Work Item Links](#work-item-links)
- [Work Item Relations](#work-item-relations)
- [Work Logs](#work-logs)
- [Cycles](#cycles)
- [Modules](#modules)
- [Initiatives](#initiatives)
- [Intake Work Items](#intake-work-items)
- [Labels](#labels)
- [States](#states)
- [Work Item Types](#work-item-types)
- [Work Item Properties](#work-item-properties)
- [Pages](#pages)
- [Epics](#epics)
- [Milestones](#milestones)
- [Workspace](#workspace)

---

## Projects

### list-projects

List all projects in the workspace.

```
plane-cli list-projects [--cursor <cursor>] [--per-page <1-100>] [--expand <fields>] [--fields <fields>] [--order-by <field>]
```

### create-project

```
plane-cli create-project --name <name> --identifier <id> [--description <desc>] [--project-lead <uuid>] [--default-assignee <uuid>]
```

Optional via `--raw`: emoji, cover_image, module_view, cycle_view, issue_views_view, page_view, intake_view, guest_view_all_features, archive_in, close_in, timezone, external_source, external_id, is_issue_type_enabled.

### retrieve-project

```
plane-cli retrieve-project --project-id <uuid>
```

### update-project

```
plane-cli update-project --project-id <uuid> [--name <name>] [--description <desc>] [--project-lead <uuid>] [--default-assignee <uuid>]
```

Additional fields via `--raw`: identifier, emoji, cover_image, module_view, cycle_view, issue_views_view, page_view, intake_view, guest_view_all_features, archive_in, close_in, timezone, external_source, external_id, is_issue_type_enabled, is_time_tracking_enabled, default_state, estimate.

### delete-project

```
plane-cli delete-project --project-id <uuid>
```

### get-project-worklog-summary

Returns work log summary with work item IDs and durations.

```
plane-cli get-project-worklog-summary --project-id <uuid>
```

### get-project-members

```
plane-cli get-project-members --project-id <uuid> [--params <json>]
```

### get-project-features

Returns enabled/disabled feature flags for a project.

```
plane-cli get-project-features --project-id <uuid>
```

### update-project-features

```
plane-cli update-project-features --project-id <uuid> [--epics <bool>] [--modules <bool>] [--cycles <bool>] [--views <bool>]
```

Additional fields via `--raw`: pages, intakes, work_item_types.

---

## Work Items

### list-work-items

List work items in a project. When filters are provided, uses advanced search endpoint.

```
plane-cli list-work-items [--project-id <uuid>] [--query <text>] [--assignee-ids <ids>] [--state-ids <ids>] [--state-groups <groups>]
```

Filter options (trigger advanced search): `--assignee-ids`, `--state-ids`, `--state-groups` (backlog,unstarted,started,completed,cancelled), `--priorities` (urgent,high,medium,low,none), `--label-ids`, `--type-ids`, `--cycle-ids`, `--module-ids`, `--is-archived`, `--created-by-ids`, `--query`, `--workspace-search`, `--limit`.

List-only options: `--cursor`, `--per-page`, `--expand`, `--fields`, `--order-by`, `--external-id`, `--external-source`.

### create-work-item

```
plane-cli create-work-item --project-id <uuid> --name <name> [--assignees <ids>] [--labels <ids>] [--type-id <uuid>]
```

Additional fields via `--raw`: point, description_html, description_stripped, priority, start_date, target_date, sort_order, is_draft, external_source, external_id, parent, state, estimate_point, type.

### retrieve-work-item

```
plane-cli retrieve-work-item --project-id <uuid> --work-item-id <uuid> [--expand <fields>] [--fields <fields>]
```

### retrieve-work-item-by-identifier

Look up by project identifier + sequence number (e.g., MP-42).

```
plane-cli retrieve-work-item-by-identifier --project-identifier <id> --issue-identifier <number> [--expand <fields>] [--fields <fields>]
```

### update-work-item

```
plane-cli update-work-item --project-id <uuid> --work-item-id <uuid> [--name <name>] [--assignees <ids>] [--labels <ids>]
```

Additional fields via `--raw`: type_id, point, description_html, description_stripped, priority, start_date, target_date, sort_order, is_draft, external_source, external_id, parent, state, estimate_point, type.

### delete-work-item

```
plane-cli delete-work-item --project-id <uuid> --work-item-id <work-item-id>
```

### search-work-items

Search across the entire workspace by text.

```
plane-cli search-work-items --query <text> [--expand <fields>] [--fields <fields>] [--order-by <field>]
```

---

## Work Item Activities

### list-work-item-activities

```
plane-cli list-work-item-activities --project-id <uuid> --work-item-id <uuid> [--params <json>]
```

### retrieve-work-item-activity

```
plane-cli retrieve-work-item-activity --project-id <uuid> --work-item-id <uuid> --activity-id <uuid>
```

---

## Work Item Comments

### list-work-item-comments

```
plane-cli list-work-item-comments --project-id <uuid> --work-item-id <uuid> [--params <json>]
```

### retrieve-work-item-comment

```
plane-cli retrieve-work-item-comment --project-id <uuid> --work-item-id <uuid> --comment-id <uuid>
```

### create-work-item-comment

```
plane-cli create-work-item-comment --project-id <uuid> --work-item-id <uuid> [--comment-html <html>] [--comment-json <json>] [--access <INTERNAL|EXTERNAL>]
```

### update-work-item-comment

```
plane-cli update-work-item-comment --project-id <uuid> --work-item-id <uuid> --comment-id <uuid> [--comment-html <html>] [--comment-json <json>]
```

### delete-work-item-comment

```
plane-cli delete-work-item-comment --project-id <uuid> --work-item-id <uuid> --comment-id <uuid>
```

---

## Work Item Links

### list-work-item-links

```
plane-cli list-work-item-links --project-id <uuid> --work-item-id <uuid> [--params <json>]
```

### retrieve-work-item-link

```
plane-cli retrieve-work-item-link --project-id <uuid> --work-item-id <uuid> --link-id <uuid>
```

### create-work-item-link

```
plane-cli create-work-item-link --project-id <uuid> --work-item-id <uuid> --url <url>
```

### update-work-item-link

```
plane-cli update-work-item-link --project-id <uuid> --work-item-id <uuid> --link-id <uuid> [--url <url>]
```

### delete-work-item-link

```
plane-cli delete-work-item-link --project-id <uuid> --work-item-id <uuid> --link-id <uuid>
```

---

## Work Item Relations

Relation types: blocking, blocked_by, duplicate, relates_to, start_after, start_before, finish_after, finish_before.

### list-work-item-relations

```
plane-cli list-work-item-relations --project-id <uuid> --work-item-id <uuid>
```

Returns grouped lists: blocking, blocked_by, duplicate, relates_to, start_after, start_before, finish_after, finish_before.

### create-work-item-relation

```
plane-cli create-work-item-relation --project-id <uuid> --work-item-id <uuid> --relation-type <type> --issues <id1,id2>
```

### remove-work-item-relation

```
plane-cli remove-work-item-relation --project-id <uuid> --work-item-id <uuid> --related-issue <uuid>
```

---

## Work Logs

### list-work-logs

```
plane-cli list-work-logs --project-id <uuid> --work-item-id <uuid> [--params <json>]
```

### create-work-log

```
plane-cli create-work-log --project-id <uuid> --work-item-id <uuid> [--duration <minutes>] [--description <text>]
```

### update-work-log

```
plane-cli update-work-log --project-id <uuid> --work-item-id <uuid> --work-log-id <uuid> [--duration <minutes>] [--description <text>]
```

### delete-work-log

```
plane-cli delete-work-log --project-id <uuid> --work-item-id <uuid> --work-log-id <uuid>
```

---

## Cycles

### list-cycles

```
plane-cli list-cycles --project-id <uuid> [--params <json>]
```

### create-cycle

```
plane-cli create-cycle --project-id <uuid> --name <name> --owned-by <uuid> [--description <desc>] [--start-date <ISO8601>] [--end-date <ISO8601>]
```

Additional via `--raw`: external_source, external_id, timezone.

### retrieve-cycle

```
plane-cli retrieve-cycle --project-id <uuid> --cycle-id <uuid>
```

### update-cycle

```
plane-cli update-cycle --project-id <uuid> --cycle-id <uuid> [--name <name>] [--description <desc>] [--start-date <ISO8601>] [--end-date <ISO8601>]
```

### delete-cycle

```
plane-cli delete-cycle --project-id <uuid> --cycle-id <uuid>
```

### list-archived-cycles

```
plane-cli list-archived-cycles --project-id <uuid> [--params <json>]
```

### add-work-items-to-cycle

```
plane-cli add-work-items-to-cycle --project-id <uuid> --cycle-id <uuid> --issue-ids <id1,id2>
```

### remove-work-item-from-cycle

```
plane-cli remove-work-item-from-cycle --project-id <uuid> --cycle-id <uuid> --work-item-id <uuid>
```

### list-cycle-work-items

```
plane-cli list-cycle-work-items --project-id <uuid> --cycle-id <uuid> [--params <json>]
```

### transfer-cycle-work-items

Transfer incomplete work items from one cycle to another.

```
plane-cli transfer-cycle-work-items --project-id <uuid> --cycle-id <source-uuid> --new-cycle-id <target-uuid>
```

### archive-cycle

```
plane-cli archive-cycle --project-id <uuid> --cycle-id <uuid>
```

### unarchive-cycle

```
plane-cli unarchive-cycle --project-id <uuid> --cycle-id <uuid>
```

---

## Modules

### list-modules

```
plane-cli list-modules --project-id <uuid> [--params <json>]
```

### create-module

```
plane-cli create-module --project-id <uuid> --name <name> [--description <desc>] [--start-date <ISO8601>] [--target-date <ISO8601>]
```

Additional via `--raw`: status (backlog, planned, in-progress, paused, completed, cancelled), lead, members, external_source, external_id.

### retrieve-module

```
plane-cli retrieve-module --project-id <uuid> --module-id <uuid>
```

### update-module

```
plane-cli update-module --project-id <uuid> --module-id <uuid> [--name <name>] [--description <desc>] [--start-date <ISO8601>]
```

### delete-module

```
plane-cli delete-module --project-id <uuid> --module-id <uuid>
```

### list-archived-modules

```
plane-cli list-archived-modules --project-id <uuid> [--params <json>]
```

### add-work-items-to-module

```
plane-cli add-work-items-to-module --project-id <uuid> --module-id <uuid> --issue-ids <id1,id2>
```

### remove-work-item-from-module

```
plane-cli remove-work-item-from-module --project-id <uuid> --module-id <uuid> --work-item-id <uuid>
```

### list-module-work-items

```
plane-cli list-module-work-items --project-id <uuid> --module-id <uuid> [--params <json>]
```

### archive-module

```
plane-cli archive-module --project-id <uuid> --module-id <uuid>
```

### unarchive-module

```
plane-cli unarchive-module --project-id <uuid> --module-id <uuid>
```

---

## Initiatives

Workspace-level (no project-id needed).

### list-initiatives

```
plane-cli list-initiatives [--params <json>]
```

### create-initiative

```
plane-cli create-initiative --name <name> [--description-html <html>] [--start-date <ISO8601>] [--end-date <ISO8601>]
```

Additional via `--raw`: logo_props, state (DRAFT, PLANNED, ACTIVE, COMPLETED, CLOSED), lead.

### retrieve-initiative

```
plane-cli retrieve-initiative --initiative-id <uuid>
```

### update-initiative

```
plane-cli update-initiative --initiative-id <uuid> [--name <name>] [--description-html <html>] [--start-date <ISO8601>] [--end-date <ISO8601>]
```

### delete-initiative

```
plane-cli delete-initiative --initiative-id <uuid>
```

---

## Intake Work Items

### list-intake-work-items

```
plane-cli list-intake-work-items --project-id <uuid> [--params <json>]
```

### create-intake-work-item

```
plane-cli create-intake-work-item --project-id <uuid> --data <json>
```

### retrieve-intake-work-item

Use the `issue` field from the intake response, not the intake work item ID.

```
plane-cli retrieve-intake-work-item --project-id <uuid> --work-item-id <uuid> [--params <json>]
```

### update-intake-work-item

```
plane-cli update-intake-work-item --project-id <uuid> --work-item-id <uuid> --data <json>
```

### delete-intake-work-item

```
plane-cli delete-intake-work-item --project-id <uuid> --work-item-id <uuid>
```

---

## Labels

### list-labels

```
plane-cli list-labels --project-id <uuid> [--params <json>]
```

### create-label

```
plane-cli create-label --project-id <uuid> --name <name> [--color <hex>] [--description <desc>] [--parent <uuid>]
```

### retrieve-label

```
plane-cli retrieve-label --project-id <uuid> --label-id <uuid>
```

### update-label

```
plane-cli update-label --project-id <uuid> --label-id <uuid> [--name <name>] [--color <hex>] [--description <desc>]
```

### delete-label

```
plane-cli delete-label --project-id <uuid> --label-id <uuid>
```

---

## States

State groups: backlog, unstarted, started, completed, cancelled.

### list-states

```
plane-cli list-states --project-id <uuid> [--params <json>]
```

### create-state

```
plane-cli create-state --project-id <uuid> --name <name> --color <hex> [--description <desc>] [--sequence <int>]
```

Additional via `--raw`: group, is_triage, default, external_source, external_id.

### retrieve-state

```
plane-cli retrieve-state --project-id <uuid> --state-id <uuid>
```

### update-state

```
plane-cli update-state --project-id <uuid> --state-id <uuid> [--name <name>] [--color <hex>] [--description <desc>]
```

### delete-state

```
plane-cli delete-state --project-id <uuid> --state-id <uuid>
```

---

## Work Item Types

### list-work-item-types

```
plane-cli list-work-item-types --project-id <uuid> [--params <json>]
```

### create-work-item-type

```
plane-cli create-work-item-type --project-id <uuid> --name <name> [--description <desc>] [--project-ids <ids>] [--is-epic <bool>]
```

### retrieve-work-item-type

```
plane-cli retrieve-work-item-type --project-id <uuid> --work-item-type-id <uuid>
```

### update-work-item-type

```
plane-cli update-work-item-type --project-id <uuid> --work-item-type-id <uuid> [--name <name>] [--description <desc>] [--project-ids <ids>]
```

### delete-work-item-type

```
plane-cli delete-work-item-type --project-id <uuid> --work-item-type-id <uuid>
```

---

## Work Item Properties

### list-work-item-properties

```
plane-cli list-work-item-properties --project-id <uuid> --type-id <uuid> [--params <json>]
```

### create-work-item-property

Property types: TEXT, DATETIME, DECIMAL, BOOLEAN, OPTION, RELATION, URL, EMAIL, FILE.

```
plane-cli create-work-item-property --project-id <uuid> --type-id <uuid> --display-name <name> --property-type <type> [--relation-type <ISSUE|USER>]
```

Additional via `--raw`: description, is_required, default_value, settings, is_active, is_multi, validation_rules, options.

Settings for TEXT: `{"display_format": "single-line"|"multi-line"|"readonly"}`.
Settings for DATETIME: `{"display_format": "MMM dd, yyyy"|"dd/MM/yyyy"|"MM/dd/yyyy"|"yyyy/MM/dd"}`.

### retrieve-work-item-property

```
plane-cli retrieve-work-item-property --project-id <uuid> --type-id <uuid> --work-item-property-id <uuid>
```

### update-work-item-property

```
plane-cli update-work-item-property --project-id <uuid> --type-id <uuid> --work-item-property-id <uuid> [--display-name <name>] [--property-type <type>]
```

### delete-work-item-property

```
plane-cli delete-work-item-property --project-id <uuid> --type-id <uuid> --work-item-property-id <uuid>
```

---

## Pages

### retrieve-workspace-page

```
plane-cli retrieve-workspace-page --page-id <uuid>
```

### retrieve-project-page

```
plane-cli retrieve-project-page --project-id <uuid> --page-id <uuid>
```

### create-workspace-page

```
plane-cli create-workspace-page --name <name> --description-html <html> [--access <int>] [--color <color>] [--is-locked <bool>]
```

### create-project-page

```
plane-cli create-project-page --project-id <uuid> --name <name> --description-html <html> [--access <int>] [--color <color>]
```

---

## Epics

### list-epics

```
plane-cli list-epics --project-id <uuid> [--cursor <cursor>] [--per-page <1-100>]
```

### create-epic

```
plane-cli create-epic --project-id <uuid> --name <name> [--assignees <ids>] [--labels <ids>] [--point <int>]
```

Additional via `--raw`: type_id, description_html, description_stripped, priority, start_date, target_date, sort_order, is_draft, external_source, external_id, parent, state, estimate_point.

### retrieve-epic

```
plane-cli retrieve-epic --project-id <uuid> --epic-id <uuid>
```

### update-epic

```
plane-cli update-epic --project-id <uuid> --epic-id <uuid> [--name <name>] [--assignees <ids>] [--labels <ids>]
```

### delete-epic

```
plane-cli delete-epic --project-id <uuid> --epic-id <uuid>
```

---

## Milestones

### list-milestones

```
plane-cli list-milestones --project-id <uuid> [--params <json>]
```

### create-milestone

```
plane-cli create-milestone --project-id <uuid> --title <title> [--target-date <ISO8601>]
```

### retrieve-milestone

```
plane-cli retrieve-milestone --project-id <uuid> --milestone-id <uuid>
```

### update-milestone

```
plane-cli update-milestone --project-id <uuid> --milestone-id <uuid> [--title <title>] [--target-date <ISO8601>]
```

### delete-milestone

```
plane-cli delete-milestone --project-id <uuid> --milestone-id <uuid>
```

### add-work-items-to-milestone

```
plane-cli add-work-items-to-milestone --project-id <uuid> --milestone-id <uuid> --issue-ids <id1,id2>
```

### remove-work-items-from-milestone

```
plane-cli remove-work-items-from-milestone --project-id <uuid> --milestone-id <uuid> --issue-ids <id1,id2>
```

### list-milestone-work-items

```
plane-cli list-milestone-work-items --project-id <uuid> --milestone-id <uuid> [--params <json>]
```

---

## Workspace

### get-me

Returns current authenticated user info.

```
plane-cli get-me
```

### get-workspace-members

```
plane-cli get-workspace-members
```

### get-workspace-features

```
plane-cli get-workspace-features
```

### update-workspace-features

```
plane-cli update-workspace-features [--project-grouping <bool>] [--initiatives <bool>] [--teams <bool>] [--customers <bool>] [--wiki <bool>]
```

Additional via `--raw`: pi.
