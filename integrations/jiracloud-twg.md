# Jira Cloud and TWG CLI quick guide

This guide covers using the TWG CLI to read, search, update, comment on, and transition Jira Cloud work items.

## TWG CLI setup

TWG is Atlassian's Teamwork Graph CLI. It can query Jira, Confluence, JSM, Bitbucket, Rovo search, users, projects, pull requests, and cross-product context.

Check that it is installed:

```bash
command -v twg
twg --version
```

If your shell cannot find it, try:

```bash
$HOME/.local/bin/twg --version
```

Log in:

```bash
twg login
```

Check setup and auth:

```bash
twg doctor
twg whoami
```

Show environment:

```bash
twg env
```

Update TWG:

```bash
twg update
```

## TWG help workflow

Use TWG help before guessing command names or options:

```bash
twg help jira
twg help describe "jira"
twg help describe "jira workitem"
twg help discover-skills "jira issue comments status transition"
```

Use JSON output for scripting:

```bash
twg jira workitem get PROJ-123 -o json | jq .
```

## Jira work items with TWG

Get one work item:

```bash
twg jira workitem get PROJ-123
```

Get full issue fields and comments:

```bash
twg jira workitem get PROJ-123 --full
```

Fetch specific fields:

```bash
twg jira workitem get PROJ-123 --fields summary,status,assignee,priority
```

Bulk get:

```bash
twg jira workitem bulk-get PROJ-123 PROJ-124
```

Fuzzy search:

```bash
twg jira workitem search "login failures" --limit 20
```

JQL query:

```bash
twg jira workitem query --jql "project = PROJ AND statusCategory != Done ORDER BY updated DESC"
```

Semantic Jira search through Rovo:

```bash
twg search "login failures" --app jira
```

## Comments, transitions, and updates

Before any mutation:

1. Resolve the site and issue key.
2. Read the current work item.
3. Discover exact command syntax with `twg help describe`.
4. For transitions, discover available transitions instead of guessing.
5. Verify the mutation with a follow-up read.

Inspect work item command help:

```bash
twg help describe "jira workitem"
```

Discover Jira field and transition guidance:

```bash
twg help describe "skill:twg-jira/references/fields-and-transitions.md"
twg help describe "skill:twg-jira/references/workitems.md"
twg help describe "skill:twg-jira/references/rich-content.md"
```

Create a basic task:

```bash
twg jira workitem create --space PROJ --type Task --summary "New task"
```

For comments, status changes, links, or custom fields, inspect the exact command first:

```bash
twg help "jira workitem comment"
twg help "jira workitem transition"
twg help "jira workitem link"
twg help "jira workitem update"
```

## Boards and sprints

Find board/backlog commands:

```bash
twg help jira board
twg help describe "jira board backlog query"
```

Query a board backlog:

```bash
twg jira board backlog query --board-id <board-id>
```

Use this when deciding what work to pick next from a real board/backlog instead of relying on broad activity.

## Cross-product context

Search across connected Atlassian and enterprise tools:

```bash
twg search "topic or project name"
```

Resolve fuzzy references:

```bash
twg resolve --query "project or issue name"
```

Find related context:

```bash
twg context
```

Find your recent work:

```bash
twg recently-viewed
twg work
```

Find pull requests:

```bash
twg pull-requests
twg prs
```

## Safe mutation rules

- Read current state before update, transition, link, comment, or delete.
- Use returned `customfield_*` IDs for custom fields, not display names.
- Discover transitions instead of guessing transition names or IDs.
- Keep Jira project/admin changes separate from ordinary issue updates.
- Verify every mutation with a native read and report the resulting key/URL.
- Commands such as project archive/delete require explicit `--yes`; do not use them casually.

## Daily workflow

Check auth:

```bash
twg doctor
twg whoami
```

Find an issue:

```bash
twg jira workitem get PROJ-123 --full
```

Search related work:

```bash
twg jira workitem search "search text" --limit 20
```

Query active work:

```bash
twg jira workitem query --jql "assignee = currentUser() AND statusCategory != Done ORDER BY updated DESC"
```

Before changing anything:

```bash
twg help describe "jira workitem"
twg help describe "skill:twg-jira/references/workitems.md"
```

Use JSON when you want to pipe to `jq`:

```bash
twg jira workitem get PROJ-123 --full -o json | jq .
```
