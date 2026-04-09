# Getting Started

There are two ways to use `pbir`: directly from the terminal (human), or through an AI coding agent (agent-driven). Both start with installing the CLI.

## Install

### PyPI (recommended)

```bash
uv tool install pbir-cli
```

Or with pip: `pip install pbir-cli`

### Native installers

Download the latest installer for your platform from [GitHub Releases](https://github.com/maxanatsko/pbir.tools/releases), run it, then open a new terminal.

<details>
<summary>If your OS blocks the installer</summary>

**macOS:** Try to open the installer once. Go to `System Settings` => `Privacy & Security` => scroll to the `Security` section => click `Open Anyway`.

**Windows:** Click `More info` on the SmartScreen dialog => click `Run anyway`.

</details>

Verify the installation:

```bash
pbir --version
```

---

## Getting Started: Humans

For Power BI users who want to use `pbir` directly from the terminal.

### Terminal basics

- **macOS:** open `Terminal`
- **Windows:** open `PowerShell`, `Command Prompt`, or `Windows Terminal`
- `cd` changes folders. Quote paths with spaces. Press `Tab` to autocomplete.

### Your first 5 minutes

**1. Go to the folder containing your PBIR report**

```bash
cd "C:\Users\YourName\Documents\My Reports"
```

**2. Find reports**

```bash
pbir ls
```

**3. Inspect the structure**

```bash
pbir tree "Sales.Report" -v
```

This shows pages, visuals, and the fields each visual uses.

**4. Inspect the semantic model**

```bash
pbir model "Sales.Report" -d
```

Use this before adding or rebinding visuals so you know the table, column, and measure names available.

**5. Make a change**

```bash
pbir add visual card "Sales.Report/Overview.Page" --title "Revenue" -d "Values:Sales.Revenue"
```

**6. Validate**

```bash
pbir validate "Sales.Report"
```

### Path syntax

`pbir` addresses report objects with filesystem-style paths:

```text
Report.Report/Page.Page/Visual.Visual
```

Examples:

```text
Sales.Report                              # A report
Sales.Report/Overview.Page                # A page
Sales.Report/Overview.Page/Revenue.Visual # A visual
Sales.Report/**/*.Visual                  # All visuals (glob)
```

Quote paths when names contain spaces.

### Safe workflow

```bash
pbir backup "Sales.Report"           # Before edits
# ... make changes ...
pbir validate "Sales.Report"         # After edits
pbir restore "Sales.Report"          # If something goes wrong
```

---

## Getting Started: Agents

For users of Claude Code, GitHub Copilot, Cursor, Gemini CLI, Codex, or similar AI coding agents.

### Step 1: Install skills

Your agent needs **skills** that teach it how to use `pbir` effectively.

**Option A: `pbir setup`** (recommended)

```bash
pbir setup
```

This auto-detects your installed agents and lets you pick which Power BI plugins to install. Skills, hooks, and agent configs are written to the right directories for each agent.

```bash
pbir setup --all                                # All plugins, all agents
pbir setup --agent claude-code --all            # Claude Code only
pbir setup --plugin reports --plugin fabric-cli # Specific plugins
```

**Option B: Install the marketplace directly**

Install the full [power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) marketplace, which includes skills for reports, semantic models, themes, DAX, Fabric CLI, and more.

```bash
# Claude Code
claude plugin marketplace add data-goblin/power-bi-agentic-development

# GitHub Copilot CLI
copilot plugin install data-goblin/power-bi-agentic-development
```

### Step 2: Navigate to your report

Open your agent in the folder containing your PBIR report. The agent needs to be in the right directory to find reports.

```bash
cd "path/to/my/reports"
```

### Step 3: Ask your agent

Once skills are installed, just describe what you want. The agent will use `pbir` commands under the hood. Examples:

- "List all reports and show me the structure of Sales.Report"
- "Add a card visual showing Revenue to the Overview page"
- "Set all title font sizes to 14 across the report"
- "Create a new report connected to spaceparts-dev/spaceparts-otc-full.SemanticModel"
- "Add a TopN filter for the top 10 customers by revenue"
- "Validate the report and fix any issues"

The agent will discover properties, bind fields, format visuals, and validate -- all through the CLI. You don't need to know the commands yourself.

### Step 4: Review and validate

After the agent makes changes, always ask it to validate:

- "Validate the report"
- "Show me the tree view with fields"

Or run it yourself:

```bash
pbir validate "Sales.Report" --all
```

---

## Next Steps

- [CLI Docs](cli/README.md) -- full command reference
- [CLI Workflows](cli/workflows.md) -- end-to-end workflows
- [power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) -- full plugin marketplace

## Help

- `pbir --help`
- `pbir <command> --help`
- [Report issues](https://github.com/maxanatsko/pbir.tools/issues)
