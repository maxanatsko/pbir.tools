<p align="center">
  <img src="docs/pbir.gif" alt="pbir CLI demo" width="600">
</p>

<h1 align="center">pbir.tools</h1>

<p align="center">
  <strong>CLI + Python API + MCP Server for Power BI report manipulation</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-0.9.0--beta-blue" alt="Version">
  <img src="https://img.shields.io/badge/python-3.10+-green" alt="Python">
  <img src="https://img.shields.io/badge/license-Non--Commercial-orange" alt="License">
</p>

<p align="center">
  Create, format, validate, and publish Power BI reports with code or agents -- say goodbye and good riddance to the Format Pane!
</p>

> [!WARNING]
> This project is in beta release. To be safe, back up your report files manually before using the PBIR tools on them.


## What are pbir tools?

A collection of tools that let you work with Power BI reports in the [PBIR format](https://learn.microsoft.com/power-bi/developer/projects/projects-report#pbir-format). You can:

- **Browse** reports, pages, visuals, filters and their data bindings or formatting from the terminal
- **Create** reports, pages, visuals, filters, and more with a single command
- **Bulk-format** dozens of visuals at once instead of clicking through each one
- **Validate** reports for broken fields, schema issues, and quality problems
- **Enhance** your workflow with convenient helpers like converting between PBIP and PBIX, working with themes, thin-report measures, visual calculations, images, and more
- **Script** complex report changes with Python

> [!NOTE]
> New to the terminal? Start with the [Getting Started guide](docs/getting-started.md)
> It covers terminal basics, installation, and your first 5 minutes with pbir.

---

## Install

### Standalone Binary (Recommended)

Download the pbir binary for your platform -- no Python required.

<!-- TODO: Update with GitHub Releases URLs when binaries are published -->

**Mac:** Download `pbir`, then move it to your PATH:
```bash
cp ~/Downloads/pbir /usr/local/bin/pbir
pbir --version
```

**Windows:** Download `pbir.exe`, then add it to your PATH:
```cmd
mkdir %LOCALAPPDATA%\pbir
copy %USERPROFILE%\Downloads\pbir.exe %LOCALAPPDATA%\pbir\pbir.exe
```
Then add `%LOCALAPPDATA%\pbir` to your system PATH ([instructions](https://www.architectryan.com/2018/03/17/add-to-the-windows-path-variable/)). Open a new terminal and run `pbir --version`.

### Install from PyPi

Requires Python 3.10+. We also recommend `uv`.

```bash
pip install pbir-object-model, pbir-cli
# or
uv get pbir-object-model, pbir-cli
```

You also must download and install the [Fabric CLI](https://learn.microsoft.com/fabric/cli/) to work with connected models and download/publish reports.

---

## Quick Start

### pbir CLI

![PBIR-cli](docs/pbir.gif)

```bash
# Setup memory files, skills, etc.
pbir setup

# Find reports in current directory
pbir ls

# See structure with fields
pbir tree "Report.Report" -v

# Discover tables, columns, measures
pbir model "Report.Report" -d

# Example of adding a visual
pbir add visual card "Report.Report/Page.Page" --title "Revenue" -d "Values:Sales.Revenue"

# Always validate after changes
pbir validate "Report.Report"
```

> Tip: Run `pbir <command> --help` for help on any command.

See [CLI documentation](docs/cli/) for all commands, or the [Getting Started guide](docs/getting-started.md) if you're new to the terminal.

### Python Object Model

For scripting complex report changes:

```python
from pbir_object_model import Report

report = Report.load("Report.Report")
for page in report.pages:
    for visual in page.visuals:
        visual.title.show = True
        visual.save()
```

See [Object Model documentation](docs/object-model/).

### MCP Server

For AI assistant integration (Claude Code, Cursor, etc.):

```json
{
  "mcpServers": {
    "pbir": {
      "command": "pbir-mcp"
    }
  }
}
```

See [MCP documentation](docs/mcp/).

---

## Path Syntax

pbir uses paths to address objects inside reports, similar to file paths:

```
ReportName.Report/PageName.Page/VisualName.Visual
```

Each part has a **type suffix** (`.Report`, `.Page`, `.Visual`) that tells pbir what kind of object it is. Quote paths with spaces.

| Pattern | What it means |
|---------|--------------|
| `Sales.Report` | A report called "Sales" |
| `Sales.Report/Overview.Page` | The "Overview" page |
| `Sales.Report/Overview.Page/Revenue.Visual` | A visual called "Revenue" |
| `Sales.Report/**/*.Visual` | All visuals across all pages (glob pattern) |
| `Revenue.Visual.title.fontSize` | A property on a visual (dot notation) |
| `Sales.Report/filter:Year` | A filter named "Year" |
| `Sales.Report/bookmark:Q4` | A bookmark named "Q4" |
| `MyWorkspace.Workspace/Sales.Report` | A workspace destination (for publish/download) |

> Tip: Use `pbir ls "Sales.Report"` to see page names, and `pbir ls "Sales.Report/Overview.Page"` to see visual names.

---

## Documentation

| Area | What it covers | Who it's for |
|------|---------------|-------------|
| **[Getting Started](docs/getting-started.md)** | Terminal basics, install, first 5 minutes | New to the terminal |
| **[CLI Commands](docs/cli/)** | All `pbir` commands organized by group | Everyone |
| **[Workflows](docs/cli/workflows.md)** | End-to-end walkthroughs for common tasks | Learning by example |
| **[Object Model](docs/object-model/)** | Python API: Report, Page, Visual, Theme, Filter | Scripting & automation |
| **[MCP Server](docs/mcp/)** | AI assistant integration via Model Context Protocol | AI-assisted development |

### CLI Command Reference

| Group | Commands | What you can do | Docs |
|-------|----------|----------------|------|
| **Browse** | `ls` `find` `cat` `get` `model` | Explore reports and discover model fields | [browse.md](docs/cli/browse.md) |
| **Create** | `new` `add` `cp` `mv` | Create reports, pages, visuals; duplicate and move objects | [create.md](docs/cli/create.md) |
| **Modify** | `set` `rm` `visuals` `pages` | Format visuals, set properties, resize pages, remove objects | [modify.md](docs/cli/modify.md) |
| **Data** | `fields` `filters` `dax` `bookmarks` | Bind fields, manage filters, write DAX measures | [data.md](docs/cli/data.md) |
| **Theme** | `theme` `schema` | Edit theme colors/fonts, discover visual properties | [theme.md](docs/cli/theme.md) |
| **Ops** | `validate` `backup` `publish` `download` `script` | Validate, back up, publish to Fabric, automate with Python | [ops.md](docs/cli/ops.md) |
| **Config** | `config` `setup` | Configure the CLI, install AI agent skills | [config.md](docs/cli/config.md) |
| **Report** | `report` | Rename, rebind, convert formats, merge, split reports | [report.md](docs/cli/report.md) |

---

## Safety

- **`--force` / `-f`** is required for destructive operations (`rm`) and bulk glob patterns -- you can't accidentally delete or overwrite things
- **`pbir backup`** creates timestamped snapshots; **`pbir restore`** reverts to any backup
- **`pbir validate`** catches broken field references, schema violations, and structural issues -- run it after every change
- **Report conversion** uses atomic staging (copy to temp, swap into place, rollback on failure)

## AI Disclaimer

This project was built with assistance from [Claude Code](https://claude.ai/claude-code). AI-generated code has been reviewed and tested but may contain errors. Use at your own risk.

---

<p align="center">
  <strong>Kurt Buhler</strong> &middot; <a href="https://github.com/data-goblin">@data-goblin</a> &middot; <a href="https://data-goblins.com">Data Goblins</a>
  <br>
  <strong>Maxim Anatsko</strong> &middot; <a href="https://github.com/maxanatsko">@maxanatsko</a> &middot; <a href="https://maxanatsko.com">maxanatsko.com</a> &middot; <a href="https://www.linkedin.com/in/maxanatsko/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn" height="18" style="vertical-align: middle;"></a>
</p>

<p align="center">
  <sub>v0.9.0-beta &middot; Non-Commercial License <br> (c) 2025 Kurt Buhler (Data Goblins, Tabular Editor), Maxim Anatsko</sub>
</p>
