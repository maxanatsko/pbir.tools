# Browse and Query

Explore what's inside a report before making changes. These commands are read-only -- they won't modify anything.

> **What's a flag?** Options like `-v` or `--verbose` that you add after a command to change its behavior. Short form (`-v`) and long form (`--verbose`) do the same thing.

## ls

List reports, pages, or visuals.

```bash
pbir ls                                        # All reports in current directory
pbir ls "Report.Report"                        # Pages in report
pbir ls "Report.Report/Page.Page"              # Visuals on page
pbir ls "Report.Report" -v                     # Show fields used in visuals
pbir ls "Report.Report" --tree                 # Full structure tree
pbir ls --all pages                            # All pages across all reports
pbir ls --all visuals                          # All visuals across all reports
pbir ls "Report.Report" --images               # List image resources
```

| Flag | Description |
|------|-------------|
| `-v, --verbose` | Show fields used in visuals |
| `-T, --tree` | Display as tree (alias for `pbir tree`) |
| `-A, --all <type>` | Flat listing: `pages` or `visuals` |
| `-a, --alphabetical` | Sort alphabetically instead of tab order |
| `--json` | Machine-readable JSON output |
| `--images` | List image resources in the report |

## tree

Display report structure as a tree. Equivalent to `pbir ls --tree`.

```bash
pbir tree "Report.Report"                      # Full report tree
pbir tree "Report.Report" -v                   # Include fields
pbir tree "Report.Report" --include pages      # Pages only (no visuals)
pbir tree "Report.Report" --include filters,pages,visuals  # Custom
```

| Flag | Description |
|------|-------------|
| `-v, --verbose` | Show fields used in visuals |
| `-i, --include` | What to include: `filters`, `pages`, `visuals`, `fields` |
| `-a, --alphabetical` | Sort alphabetically |
| `--json` | Machine-readable JSON output |

## find

Find reports, pages, or visuals using glob patterns.

```bash
pbir find "*.Report"                           # All reports
pbir find "Report.Report/*.Page"               # All pages in report
pbir find "**/*.Visual"                        # All visuals in all reports
pbir find "**/card*.Visual"                    # Visuals with "card" in name
pbir find "**" --type visual                   # All visuals (filter by type)
pbir find "*.Report" --json                    # JSON output
pbir find "**/*.Page" --count                  # Just the count
```

| Flag | Description |
|------|-------------|
| `-t, --type` | Filter by type: `report`, `page`, `visual` |
| `--json` | JSON output |
| `-c, --count` | Show only match count |

## cat

Output raw JSON for a page, visual, annotations, or theme. Pipe to `jq` for processing.

```bash
pbir cat "Report.Report/Page.Page"             # Page JSON
pbir cat "Report.Report/Page.Page/Visual.Visual"  # Visual JSON
pbir cat "Report.Report/**/*.Visual"           # All visuals (glob)
pbir cat "Report.Report/theme"                 # Theme JSON
pbir cat "Report.Report/annotations"           # Report annotations
pbir cat "Report.Report/Page.Page/config"      # Page config
pbir cat "Report.Report/report"                # report.json

# Pipe to jq
pbir cat "Report.Report/Page.Page/Visual.Visual" | jq '.visual.query'
```

## get

Get properties from report, page, or visual using dot notation.

```bash
pbir get "Report.Report.settings"
pbir get "Report.Report/Page.Page.width"
pbir get "Report.Report/Page.Page/Visual.Visual.title.show"
pbir get "Visual.Visual.bg.color"                     # Using alias
pbir get "Visual.Visual.accentBar.hover.width"        # Interaction state
```

| Flag | Description |
|------|-------------|
| `--json` | Raw JSON output |
| `--structured` | Structured objects (measures, theme colors) |

### Property Aliases

| Alias | Expands to |
|-------|-----------|
| `bg` | `background` |
| `border` | `border` |
| `title` | `title` |
| `legend` | `legend` |
| `axis.x` | `categoryAxis` |
| `axis.y` | `valueAxis` |
| `colors` | `dataColors` |
| `labels` | `labels` |

### Selectors

Access interaction states and field-specific formatting:

```
Visual.Visual.title.hover.fontSize        # Hover state
Visual.Visual.labels.field(Sales.Revenue).color   # Per-field
```

## model

Identify the semantic model a report connects to, explore its schema, and execute DAX queries.

```bash
pbir model                                                 # List all reports and their models
pbir model "Report.Report"                                 # Show connection info
pbir model "Report.Report" -d                              # Full definition (tables, columns, measures)
pbir model "Report.Report" -d -t Sales                     # Filter to specific table
pbir model "Report.Report" -d --use-cache                  # Use cached definition
pbir model "Report.Report" -q "EVALUATE VALUES('Date'[Year])"   # DAX query
pbir model "Report.Report" -q "EVALUATE 'Sales'" -F json   # JSON output
pbir model --cache                                         # Cache all model definitions
```

| Flag | Description |
|------|-------------|
| `-d, --definition` | Get model definition (tables, columns, measures) |
| `-q, --query` | Execute DAX query |
| `-v, --verbose` | Full TMDL for `-d`, full JSON for `-q` |
| `-t, --table` | Filter to specific table |
| `-o, --output` | Save definition to file |
| `-F, --format` | Query output format: `table` or `json` |
| `-c, --cache` | Cache model definitions |
| `--use-cache` | Use cached definition |
| `--json` | Machine-readable JSON output |
