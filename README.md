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
  Navigate, query, and modify Power BI reports (PBIR format) from the terminal, Python scripts, or AI assistants.
</p>

> [!WARNING]
> This project is under active development. Back up your report files before using the CLI on them.
> Use `pbir backup "Report.Report"` to create a snapshot before making changes.

---

## Install

```bash
git clone https://github.com/maxanatsko/pbir.tools && cd pbir.tools

# Install both packages (object model + CLI)
pip install -e ./pbir-object-model && pip install -e ./pbir-cli

# Or just the Python API
pip install -e ./pbir-object-model

# Or just the MCP server (includes object model)
pip install -e ./pbir-object-model && pip install -e ./pbir-mcp
```

Requires Python 3.10+. Optional: [Fabric CLI](https://learn.microsoft.com/fabric/cli/) for cloud operations.

---

## Quick Start

### pbir CLI

```bash
pbir ls                                        # Find reports in current directory
pbir tree "Report.Report" -v                   # Structure with fields
pbir model "Report.Report" -d                  # Discover model tables and columns
pbir add visual card "Report.Report/Page.Page" --title "Revenue" -d "Values:Sales.Revenue"
pbir validate "Report.Report"                  # Always validate after changes
```

See [CLI documentation](docs/cli/) for all commands.

### Python Object Model

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

Add to your Claude Code MCP settings:

```json
{
  "mcpServers": {
    "pbir": {
      "command": "pbir-mcp"
    }
  }
}
```

The MCP server exposes all report manipulation as tools for AI assistants. See [MCP documentation](docs/mcp/).

---

## Path Syntax

```
ReportName.Report/PageName.Page/VisualName.Visual
```

Type suffixes (`.Report`, `.Page`, `.Visual`) are required. Quote paths with spaces. Glob patterns (`**/*.Visual`) work with most commands.

Properties use dot notation after the path: `Visual.Visual.title.fontSize`

| Pattern | Meaning |
|---------|---------|
| `Report.Report` | A report |
| `Report.Report/Page.Page` | A page in a report |
| `Report.Report/Page.Page/Visual.Visual` | A visual on a page |
| `Report.Report/**/*.Visual` | All visuals (glob) |
| `Visual.Visual.title.fontSize` | Property access |
| `Report.Report/filter:Name` | Named filter |
| `Report.Report/bookmark:Name` | Named bookmark |
| `Workspace.Workspace/Report.Report` | Workspace destination |

---

## Documentation

| Area | What it covers | Link |
|------|---------------|------|
| **CLI Commands** | All `pbir` commands organized by group | [docs/cli/](docs/cli/) |
| **Object Model** | Python API: Report, Page, Visual, Theme, Filter | [docs/object-model/](docs/object-model/) |
| **MCP Server** | AI assistant integration via Model Context Protocol | [docs/mcp/](docs/mcp/) |

### CLI Command Reference

| Group | Commands | Purpose | Docs |
|-------|----------|---------|------|
| **Browse** | `ls` `find` `cat` `get` `model` | Explore reports, pages, visuals, and models | [browse.md](docs/cli/browse.md) |
| **Create** | `new` `add` `cp` `mv` | Create and duplicate reports, pages, visuals | [create.md](docs/cli/create.md) |
| **Modify** | `set` `rm` `visuals` `pages` | Format, position, resize, and remove objects | [modify.md](docs/cli/modify.md) |
| **Data** | `fields` `filters` `dax` `bookmarks` | Bind fields, manage filters, extension measures | [data.md](docs/cli/data.md) |
| **Theme** | `theme` `schema` | Inspect and modify themes, discover properties | [theme.md](docs/cli/theme.md) |
| **Ops** | `validate` `backup` `publish` `download` `script` | Validate, back up, publish, and automate | [ops.md](docs/cli/ops.md) |
| **Config** | `config` `setup` | CLI configuration and agent skill installation | [config.md](docs/cli/config.md) |
| **Report** | `report` | Rename, rebind, convert, merge, split reports | [report.md](docs/cli/report.md) |

Run `pbir <command> --help` for full usage.

---

## Safety

- **`--force` / `-f`** is required for destructive operations (`rm`) and bulk glob patterns (`set` with wildcards)
- **`pbir backup`** creates timestamped snapshots; **`pbir restore`** reverts to any backup
- **`pbir validate`** catches broken field references, schema violations, and structural issues
- **Report conversion** uses atomic staging (copy to temp, swap into place, rollback on failure)

## AI Disclaimer

This project was built with assistance from [Claude Code](https://claude.ai/claude-code) and [Gemini CLI](https://github.com/google-gemini/gemini-cli). AI-generated code has been reviewed and tested but may contain errors. Use at your own risk.

---

<p align="center">
  <strong>Kurt Buhler</strong> &middot; <a href="https://github.com/data-goblin">@data-goblin</a> &middot; <a href="https://data-goblins.com">Data Goblins</a>
  <br>
  <strong>Maxim Anatsko</strong> &middot; <a href="https://github.com/maxanatsko">@maxanatsko</a> &middot; <a href="https://maxanatsko.com">maxanatsko.com</a> &middot; <a href="https://www.linkedin.com/in/maxanatsko/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn" height="18" style="vertical-align: middle;"></a>
</p>

<p align="center">
  <sub>v0.9.0-beta &middot; Non-Commercial License <br> (c) 2025 Kurt Buhler (Data Goblins, Tabular Editor), Maxim Anatsko</sub>
</p>
