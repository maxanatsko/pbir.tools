# CLI Command Reference

The `pbir` CLI lets you work with Power BI reports from the terminal -- navigating, creating, and formatting reports using commands instead of the Power BI Desktop GUI.

New to the terminal? Start with the [Getting Started guide](../getting-started.md).

## Install

```bash
pip install -e ./pbir-object-model && pip install -e ./pbir-cli
pbir --help
```

## Command Groups

| Group | Commands | Docs |
|-------|----------|------|
| **Browse & Query** | `ls` `tree` `find` `cat` `get` `model` | [browse.md](browse.md) |
| **Create & Duplicate** | `new` `add` `cp` `mv` | [create.md](create.md) |
| **Modify & Format** | `set` `rm` `visuals` `pages` | [modify.md](modify.md) |
| **Data & Filters** | `fields` `filters` `dax` `bookmarks` `annotations` | [data.md](data.md) |
| **Theme & Schema** | `theme` `schema` | [theme.md](theme.md) |
| **Operations** | `validate` `backup` `restore` `publish` `download` `script` | [ops.md](ops.md) |
| **Configuration** | `config` `setup` | [config.md](config.md) |
| **Report** | `report rename` `report rebind` `report convert` `report merge` `report split-pages` | [report.md](report.md) |

## Path Syntax

```
ReportName.Report/PageName.Page/VisualName.Visual
```

Type suffixes (`.Report`, `.Page`, `.Visual`) are required. Quote paths with spaces.

- **Glob patterns**: `Report.Report/**/*.Visual` (requires `-f` with `set`)
- **Dot notation**: `Visual.Visual.title.fontSize`
- **Named objects**: `Report.Report/filter:Name`, `Report.Report/bookmark:Name`
- **Workspace paths**: `Workspace.Workspace/Report.Report`

## Global Flags

| Flag | Description |
|------|-------------|
| `-q, --quiet` | Reduce output |
| `--debug` | Enable debug logging |
| `-V, --version` | Show version |
| `--json` | Machine-readable output (where supported) |

Run `pbir <command> --help` for full usage of any command.
