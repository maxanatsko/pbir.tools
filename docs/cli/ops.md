# Operations

Validate reports, create backups, publish to Fabric workspaces, and automate with Python scripts. The most important command here is `validate` -- run it after every change to catch problems early.

## validate

Validate report structure and schemas. Run after every mutation.

```bash
pbir validate "Report.Report"                              # Schema validation
pbir validate "Report.Report" --qa                         # Schema + quality checks
pbir validate "Report.Report" --all                        # All checks
pbir validate "Report.Report" --fields                     # Check fields exist in model
pbir validate "Report.Report" --strict                     # Warnings become errors
```

### What It Checks

| Check | Flag | Description |
|-------|------|-------------|
| JSON syntax | (default) | Valid JSON in all report files |
| Schema compliance | (default) | Files match Microsoft PBIR schemas |
| Required fields | (default) | `name`, `displayName`, `visualType` present |
| Field validation | `--fields` | Fields exist in the connected semantic model |
| Quality | `--qa` | Overlaps, hidden visuals, visual filters, field counts |
| All | `--all` | Schema + fields + quality |

| Flag | Description |
|------|-------------|
| `-v, --verbose` | Detailed validation info |
| `-o, --output` | Write validation report to file |
| `-s, --strict` | Treat warnings as errors (exit 1) |
| `--json` | Machine-readable JSON summary |
| `-t, --tree` | Tree view of results |
| `--allow-download-schemas` | Download missing schemas during validation |
| `--update-schemas` | Update `$schema` properties to latest |

### Schema Version Errors

If validation shows `SCHEMA_ERROR` with messages about unexpected schema versions:

```bash
pbir schema fetch --yes              # Download latest schemas
pbir schema upgrade "Report.Report"  # Upgrade report schema references
pbir validate "Report.Report"        # Re-validate
```

## backup / restore

Create and restore report backups.

```bash
# Create
pbir backup "Report.Report"
pbir backup "Report.Report" -m "Before restyle"

# List
pbir backup --list
pbir backup --list "Report.Report"

# Prune old backups
pbir backup --prune "Report.Report"
pbir backup --prune "Report.Report" --keep 5

# Restore
pbir restore "Report.Report"                               # Restore latest
pbir restore "Report.Report" --backup-id 20260312T143000Z  # Specific backup
pbir restore "Report.Report" -f                            # Skip confirmation
```

## download

Download a report from a Fabric workspace. Requires [Fabric CLI](https://learn.microsoft.com/fabric/cli/).

```bash
pbir download "My Workspace.Workspace"                     # List reports in workspace
pbir download "My Workspace.Workspace/Report.Report" -o ./tmp --format pbir
pbir download "My Workspace.Workspace/Report.Report" -o ./tmp --format pbip --auto-deploy --open
```

| Flag | Description |
|------|-------------|
| `-o, --output` | Output directory |
| `-F, --format` | Format: `pbir` (default) or `pbip` |
| `--auto-deploy` | Enable auto-deploy |
| `--open` | Open in Power BI Desktop after download |

## publish

Publish a local report to a Fabric workspace.

```bash
pbir publish "Report.Report" "My Workspace.Workspace/Report.Report"
pbir publish "Report.Report" "My Workspace.Workspace/Report.Report" -f -o
pbir publish                                               # Uses active connection
```

| Flag | Description |
|------|-------------|
| `-f, --force` | Force overwrite |
| `-o, --open` | Open in browser after publishing |

### Workspace Path Syntax

Destinations use the `.Workspace` suffix:

```
"My Workspace.Workspace/Report.Report"
```

## script

Execute Python scripts against the PBIR object model for complex automation.

```bash
# Run a script file
pbir script my_script.py "Report.Report/Page.Page"

# Inline execution
pbir script --execute "print(context.page.display_name)" "Report.Report/Page.Page"

pbir script --execute "
for page in context.report.pages:
    for visual in page.visuals:
        visual.title.show = True
        visual.save()
" "Report.Report"

# Object model docs
pbir script --object-model
pbir script --object-model -c visual

# Examples
pbir script --list-examples
pbir script --example batch-format
```

Scripts receive a `context` object with `.report`, `.page`, or `.visual` depending on the path.

| Flag | Description |
|------|-------------|
| `-x, --execute` | Execute inline Python code |
| `--object-model` | Show object model documentation |
| `-c, --component` | Show specific component docs |
| `-e, --example` | Show example script |
| `-l, --list-examples` | List available examples |
