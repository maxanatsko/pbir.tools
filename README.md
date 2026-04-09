<p align="center">
  <img src="pbir.gif" alt="pbir demo" width="700">
</p>

<h1 align="center">pbir.tools</h1>

<p align="center">
  <strong>CLI tools for Power BI report automation; built for humans, optimized for agents</strong>
</p>

<p align="center">
  Browse, edit, validate, and publish PBIR reports from the terminal, or let Claude Code, Codex, or Copilot drive the CLI after running <code>pbir setup</code>.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-0.9.7-blue" alt="Version">
  <img src="https://img.shields.io/badge/Power_BI-F2C811?logo=powerbi&logoColor=000" alt="Power BI">
  <img src="https://img.shields.io/badge/Microsoft_Fabric-008272" alt="Microsoft Fabric">
  <img src="https://img.shields.io/badge/license-proprietary-lightgrey" alt="License">
</p>

> [!WARNING]
> `pbir.tools` is in beta. Back up reports before large edits, especially bulk formatting, conversion, merge, and publish workflows.

> [!IMPORTANT]
> After installing `pbir`, run `pbir setup` to install agent skills, or install them from the [power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) marketplace.

---

## Install

### PyPI (recommended)

```bash
uv tool install pbir-cli
```

Or with pip: `pip install pbir-cli`

<details>
<summary><strong>Native installers (macOS / Windows)</strong></summary>

Download the latest macOS or Windows installer from [GitHub Releases](https://github.com/maxanatsko/pbir.tools/releases).

**macOS**

1. Download the installer from Releases and try to open it once.
2. If macOS blocks it because the installer is unsigned or not notarized, open:
   `System Settings` => `Privacy & Security`
3. Scroll down to the `Security` section. You should see a message about the blocked installer there.
4. Click `Open Anyway`.
5. When macOS asks again, click `Open` and finish the installation.

**Windows**

1. Download the installer from Releases and run it.
2. If Windows Defender SmartScreen shows `Windows protected your PC`, click `More info`.
3. Confirm the installer name, then click `Run anyway`.
4. Finish the installation.

</details>

After installation, open a new terminal and verify:

```bash
pbir --version
```

## Quick Start

```bash
pbir ls                                                                # List reports
pbir tree "Sales.Report" -v                                            # Full structure with fields
pbir model "Sales.Report" -d                                           # Model schema
pbir add visual card "Sales.Report/Overview.Page" --title "Revenue"    # Add a visual
pbir set "Sales.Report/**/*.Visual.title.fontSize" --value 14 -f      # Bulk formatting
pbir validate "Sales.Report"                                           # Health check
pbir publish "Workspace.Workspace/Sales.Report"                        # Deploy to Fabric
```

`pbir` works with PBIR-format reports stored as folders, using paths like:

```text
Report.Report/Page.Page/Visual.Visual
```

## Agents & Plugins

`pbir` is built for agents. Install the **Power BI plugins** from the [power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) marketplace to give your agent skills for reports, semantic models, themes, DAX, and Fabric.

<details>
<summary><strong>Option 1: Install the marketplace directly</strong> (recommended)</summary>

Follow the instructions at [power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) to install the full marketplace plugin with all skills, agents, and hooks. After installing, run `/plugin` in your agent to enable auto-update and install plugins.

**Claude Code:**
```bash
claude plugin marketplace add data-goblin/power-bi-agentic-development
```

**GitHub Copilot CLI:**
```bash
copilot plugin install data-goblin/power-bi-agentic-development
```

</details>

<details>
<summary><strong>Option 2: Install plugins via <code>pbir setup</code></strong></summary>

```bash
pbir setup
```

This auto-detects which agents you have installed (Claude Code, Cursor, Copilot, Gemini CLI, Codex) and lets you pick which plugins to install interactively.

```bash
pbir setup --all                                # Install all plugins, all agents
pbir setup --agent claude-code --all            # Claude Code only, all plugins
pbir setup --plugin reports --plugin fabric-cli # Specific plugins
```

</details>

After installing, your agent can create reports, format visuals, bind fields, manage themes, filters, bookmarks, and publish to Fabric -- all driven by `pbir` commands.

## Documentation

| Area | What it covers |
|---|---|
| [Getting Started](getting-started.md) | Install, terminal basics, first 5 minutes |
| [CLI Docs](cli/README.md) | Command groups and task-based CLI reference |
| [CLI Workflows](cli/workflows.md) | End-to-end report workflows |

## Safety

- **We recommend you take a manual backup (copy + paste) of your report folder before using `pbir` for the first time.**
- Run `pbir backup "Report.Report"` before larger edits.
- Run `pbir validate "Report.Report"` after every mutation.
- Use `-f` only when you intend bulk or destructive operations.
- Treat publish, convert, merge, and split commands as higher-risk workflows.

## Disclaimer

This software is provided "as is", without warranty of any kind, express or implied. The authors are not responsible for any data loss, report corruption, or other damages resulting from the use of this tool. You use `pbir` at your own risk. Always maintain independent backups of your reports before making changes, especially with bulk operations, format conversion, merge, and publish workflows.

## Support

- Run `pbir --help` for the top-level CLI surface.
- Run `pbir <command> --help` for command-specific usage.
- [Report issues](https://github.com/maxanatsko/pbir.tools/issues)

---

<p align="center">
  <a href="https://github.com/data-goblin">Kurt Buhler</a> & <a href="https://github.com/maxanatsko">Maxim Anatsko</a>
</p>
