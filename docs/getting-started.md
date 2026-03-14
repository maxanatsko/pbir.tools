# Getting Started

This guide is for Power BI users who are new to the terminal. If you're already comfortable with command-line tools, skip to [Install](#install).

---

## What is pbir?

**pbir** is a tool that lets you work with Power BI reports as files and folders instead of through the Power BI Desktop GUI.

With pbir, you can:
- Create reports, pages, and visuals from the terminal
- Bulk-format dozens of visuals in one command instead of clicking through each one
- Validate reports for broken fields, schema issues, and quality problems
- Publish reports to Fabric workspaces without opening Power BI Desktop
- Script complex report changes with Python

It works with [**PBIR format**](https://learn.microsoft.com/power-bi/developer/projects/projects-report#pbir-format) reports -- the folder-based format that Power BI Desktop uses when you save as a Power BI Project (.pbip).

---

## Terminal Basics

The **terminal** (also called **command line**, **command prompt**, or **shell**) is a text-based interface where you type commands instead of clicking buttons.

### How to Open a Terminal

**Mac:**
Press `Cmd + Space`, type **Terminal**, press Enter.

**Windows:**
Press `Win + R`, type **cmd**, press Enter. Or search for **Command Prompt** in the Start menu.

> Tip: On Windows, you can also use **PowerShell** or **Windows Terminal** -- both work the same way for pbir.

### Key Concepts

**Current directory.** The terminal always has a "current location" on your computer, like having a folder open in File Explorer. Commands run in this location.

```bash
# Show your current location
pwd              # Mac/Linux
cd               # Windows (just 'cd' with no arguments)
```

**Changing directories.** Use `cd` (change directory) to move to a different folder.

```bash
cd ~/Documents                         # Mac: go to Documents
cd C:\Users\YourName\Documents         # Windows: go to Documents
cd ..                                  # Go up one folder
```

**Flags and options.** Commands accept options (also called **flags**) that modify their behavior. They start with `-` or `--`.

```bash
pbir ls                    # Basic: list reports
pbir ls -v                 # With flag: list reports with more detail
pbir ls --verbose          # Same thing, longer name
```

**Quoting paths.** If a file or folder name contains spaces, wrap it in quotes.

```bash
pbir ls "My Report.Report"            # Quotes needed (has a space)
pbir ls Report.Report                  # No quotes needed (no spaces)
```

**Tab completion.** Press `Tab` while typing to auto-complete file and folder names. This saves typing and avoids typos.

---

## Install

### Standalone Binary (Recommended)

pbir is distributed as a standalone executable -- no Python installation required.

<!-- TODO: Update with download URLs when binaries are published to GitHub Releases -->

**Mac:**
1. Download the `pbir` binary
2. Move it to a location on your PATH:
   ```bash
   cp ~/Downloads/pbir /usr/local/bin/pbir
   ```
3. Verify it works:
   ```bash
   pbir --version
   ```

**Windows:**
1. Download `pbir.exe`
2. Create a folder for it and add it to your PATH:
   ```cmd
   mkdir %LOCALAPPDATA%\pbir
   copy %USERPROFILE%\Downloads\pbir.exe %LOCALAPPDATA%\pbir\pbir.exe
   ```
3. Add `%LOCALAPPDATA%\pbir` to your system PATH ([how to edit PATH on Windows](https://www.architectryan.com/2018/03/17/add-to-the-windows-path-variable/))
4. Open a **new** terminal window and verify:
   ```cmd
   pbir --version
   ```

### Build from Source

If you prefer to build from source (requires Python 3.10+):

```bash
git clone https://github.com/maxanatsko/pbir.tools && cd pbir.tools

# Install with pip
pip install -e ./pbir-object-model && pip install -e ./pbir-cli

# Or build a standalone binary
pip install -e "./pbir-cli[build]"
cd pbir-cli
python scripts/build_binary.py --clean
# Output: dist/pbir (Mac) or dist/pbir.exe (Windows)
```

---

## Your First 5 Minutes

### 1. Navigate to your report

Open a terminal and navigate to the folder containing your `.pbip` project:

```bash
cd "C:\Users\YourName\Documents\My Reports"     # Windows
cd ~/Documents/My\ Reports                       # Mac
```

### 2. Find reports

```bash
pbir ls
```

This lists all PBIR reports in the current folder. You should see something like:

```
Reports found:
  Sales.Report (3 pages, 12 visuals)
```

### 3. Explore the structure

```bash
pbir tree "Sales.Report" -v
```

This shows the full report tree -- pages, visuals, and the fields bound to each visual. Think of it like a table of contents for your report.

### 4. Check the data model

```bash
pbir model "Sales.Report" -d
```

This shows all the tables, columns, and measures available in the connected semantic model. You'll need these names when adding visuals or binding fields.

### 5. Validate the report

```bash
pbir validate "Sales.Report"
```

This checks for broken field references, schema issues, and structural problems. Run this after making any changes.

---

## Understanding Paths

pbir uses a **path syntax** to address objects inside reports. Think of it like a file path, but for report objects:

```
ReportName.Report / PageName.Page / VisualName.Visual
```

Each part has a **type suffix** that tells pbir what kind of object it is:

| Suffix | What it is |
|--------|-----------|
| `.Report` | A report |
| `.Page` | A page inside a report |
| `.Visual` | A visual on a page |
| `.Filter` | A filter |

**Examples:**

```bash
"Sales.Report"                              # The report itself
"Sales.Report/Overview.Page"                # A page called "Overview"
"Sales.Report/Overview.Page/Revenue.Visual" # A visual called "Revenue"
```

> Tip: Use `pbir ls "Sales.Report"` to see page names, and `pbir ls "Sales.Report/Overview.Page"` to see visual names. You don't need to memorize them.

---

## Common Workflows

### Create a new report

```bash
# Create a report connected to a semantic model
pbir new report "Sales.Report" -c "MyWorkspace/SalesModel.SemanticModel"

# Rename the default page
pbir pages rename "Sales.Report/Page 1.Page" "Overview"

# Add a card visual with data
pbir add visual card "Sales.Report/Overview.Page" --title "Revenue" -d "Values:Sales.Revenue"

# Validate
pbir validate "Sales.Report"
```

### Format visuals in bulk

Instead of clicking through each visual in Power BI Desktop:

```bash
# Set all visual titles to font size 14
pbir set "Sales.Report/**/*.Visual.title.fontSize" --value 14 -f

# Add borders to all visuals
pbir set "Sales.Report/**/*.Visual.border.show" --value true -f

# Set border radius on all visuals
pbir set "Sales.Report/**/*.Visual.border.radius" --value 8 -f
```

The `**/*.Visual` pattern means "all visuals in all pages". The `-f` flag is required for bulk operations (a safety measure).

### Back up before making changes

```bash
pbir backup "Sales.Report" -m "Before restyling"

# ... make changes ...

# If something goes wrong, restore:
pbir restore "Sales.Report"
```

---

## Next Steps

- [CLI Command Reference](cli/) -- Full documentation for every command
- [Workflows](cli/workflows.md) -- End-to-end walkthroughs for common tasks
- [Object Model](object-model/) -- Python API for scripting report changes
- [MCP Server](mcp/) -- Set up AI assistant integration

---

## Getting Help

- Run `pbir --help` for a list of all commands
- Run `pbir <command> --help` for help on any specific command
- File issues at the [GitHub repository](https://github.com/maxanatsko/pbir.tools/issues)
