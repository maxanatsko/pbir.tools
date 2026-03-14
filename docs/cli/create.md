# Create and Duplicate

Create new reports, pages, visuals, and duplicate existing objects.

## new report

```bash
pbir new report "Sales.Report" -c "Workspace/Model.SemanticModel"
pbir new report "Sales.Report" --from-template my-dashboard
pbir new report --list-templates
```

New reports include:
- The **sqlbi** theme (professional colors, typography)
- A default **Page 1** with a textbox visual for the page title

After creation, rename the default page and start adding visuals:

```bash
pbir pages rename "Sales.Report/Page 1.Page" "Overview"
pbir add visual card "Sales.Report/Overview.Page" --title "Revenue" -d "Values:Sales.Revenue"
pbir validate "Sales.Report"
```

| Flag | Description |
|------|-------------|
| `-c, --connection` | Semantic model connection (`Workspace/Model.SemanticModel`) |
| `--from-template` | Create from a saved template |
| `--list-templates` | List available templates |
| `--thick` | Create thick report (embedded model, no connection needed) |

## add page

```bash
pbir add page "Report.Report" -n "Dashboard"
```

## add visual

```bash
pbir add visual card "Report.Report/Page.Page" --title "Revenue" -d "Values:Sales.Revenue"
pbir add visual lineChart "Report.Report/Page.Page" --title "Trend"
pbir add visual --list                         # Show all 38+ visual types
```

### Data Binding Shorthand (`-d`)

Bind fields during creation: `-d "Role:Table.Column"`

```bash
# Single field
pbir add visual card "path" -d "Values:Sales.Revenue"

# Multiple fields
pbir add visual columnChart "path" -d "Category:Products.Name" -d "Values:Sales.Revenue"
```

## add title / subtitle

Add textbox visuals positioned at the top of a page.

```bash
pbir add title "Report.Report/Page.Page" "Sales Dashboard"
pbir add subtitle "Report.Report/Page.Page" "Q4 2025 Performance"
```

## add filter

```bash
pbir add filter Date Year -r "Report.Report"           # Report-level filter
pbir add filter Date Month -p "Report.Report/Page.Page" # Page-level filter
```

## add annotation

```bash
pbir add annotation "Report.Report" --name version --value "1.0"
```

## add image

```bash
pbir add image "Report.Report" --source logo.png       # Add image to resources
```

## cp

Copy reports, themes, pages, visuals, filters, bookmarks, measures, or visual calculations.

```bash
# Reports
pbir cp "Source.Report" "Target.Report"
pbir cp "Source.Report" "Target.Report" --format pbix   # Copy and convert

# Theme
pbir cp "Source.Report/theme" "Target.Report/theme"

# Pages
pbir cp "Report.Report/Page1.Page" "Report.Report/Page2.Page"

# Extension measures
pbir cp "Source.Report" "Target.Report" --measures
pbir cp "Source.Report" "Target.Report" --measures --table Sales

# Visual calculations
pbir cp "R.Report/P.Page/V1.Visual" "R.Report/P.Page/V2.Visual" --viscalcs
```

| Flag | Description |
|------|-------------|
| `-F, --format` | Target format (`pbir`, `pbip`, `pbix`) for report copies |
| `-f, --force` | Force overwrite |
| `-m, --measures` | Copy extension measures between reports |
| `-t, --table` | Filter measures by table name |
| `--viscalcs` | Copy visual calculations between visuals |

## mv

Move or rename pages and visuals.

```bash
# Rename page
pbir mv "Report.Report/Old.Page" "Report.Report/New.Page"

# Move page between reports
pbir mv "Report1.Report/Page.Page" "Report2.Report/Page.Page"

# Rename visual
pbir mv "Report.Report/Page.Page/Old.Visual" "Report.Report/Page.Page/New.Visual"
```

To rename a report, use `pbir report rename "Report.Report" "New Name"`.

| Flag | Description |
|------|-------------|
| `-f, --force` | Force overwrite |
