# CLI Documentation

The `pbir` CLI lets you work with Power BI reports as files and folders instead of clicking through Power BI Desktop for every change.

That matters most in three situations:

- you need to inspect an unfamiliar report quickly
- you want to apply the same change across many visuals or pages
- you want a repeatable workflow for validation, backup, publish, and recovery

## How Most People Use `pbir`

Most report edits follow the same pattern:

1. Browse the report to understand its pages, visuals, fields, and current formatting.
2. Make a targeted change with a focused command such as `add`, `set`, `visuals`, `pages`, or `report`.
3. Run `pbir validate` before you consider the edit finished.
4. Back up, publish, or reopen the report only after the local version is clean.

If you are new to the command line, start with [../getting-started.md](../getting-started.md) first.

## Command Groups

| Group | Commands | When to use it | Docs |
|---|---|---|---|
| Browse & Query | `ls`, `tree`, `find`, `cat`, `get`, `model` | Understand what is in a report before editing | [browse.md](browse.md) |
| Create & Duplicate | `new`, `add`, `cp`, `mv` | Create reports, pages, visuals, or duplicate existing objects | [create.md](create.md) |
| Modify & Format | `set`, `rm`, `visuals`, `pages` | Change formatting, layout, visibility, and object state | [modify.md](modify.md) |
| Data & State | `fields`, `filters`, `dax`, `bookmarks`, `annotations` | Manage bindings, filters, measures, bookmarks, and metadata | [data.md](data.md) |
| Theme & Schema | `theme`, `schema` | Apply defaults at scale and discover valid property names | [theme.md](theme.md) |
| Operations | `validate`, `backup`, `restore`, `download`, `publish`, `open` | Protect, verify, move, and ship reports | [ops.md](ops.md) |
| Config | `config` | Control CLI behavior | [config.md](config.md) |
| Report-Wide Changes | `report` | Rename, rebind, convert, merge, and split whole reports | [report.md](report.md) |
| End-to-End Examples | Common workflows | See realistic command sequences, not isolated commands | [workflows.md](workflows.md) |

## Path Syntax

`pbir` addresses report objects with filesystem-style paths:

```text
Report.Report/Page.Page/Visual.Visual
```

Examples:

```text
Sales.Report
Sales.Report/Overview.Page
Sales.Report/Overview.Page/Revenue.Visual
Sales.Report/**/*.Visual
My Workspace.Workspace/Sales.Report
```

Use `.Workspace` paths when a command targets Fabric rather than a local folder.

## A Good Default Habit

If you are not sure what to do next, this is a safe baseline:

```bash
pbir tree "Sales.Report" -v
pbir backup "Sales.Report" -m "Before edits"
pbir validate "Sales.Report"
```

## Help

```bash
pbir --help
pbir <command> --help
```
