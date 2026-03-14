# Workflows

End-to-end walkthroughs for common tasks. Each workflow shows a complete sequence of commands you'd run to accomplish a real goal.

> New to the terminal? See the [Getting Started guide](../getting-started.md) first.

---

## Create a Report from Scratch

**Goal:** Create a new report connected to a semantic model, add a page with visuals, and validate it.

```bash
# 1. Create the report (connected to a semantic model in a Fabric workspace)
pbir new report "Sales.Report" -c "MyWorkspace/SalesModel.SemanticModel"

# 2. Rename the default page (new reports come with "Page 1")
pbir pages rename "Sales.Report/Page 1.Page" "Overview"

# 3. Discover what fields are available in the model
pbir model "Sales.Report" -d

# 4. Add visuals with data bindings
pbir add visual card "Sales.Report/Overview.Page" --title "Revenue" -d "Values:Invoices.Revenue Net"
pbir add visual card "Sales.Report/Overview.Page" --title "Quantity" -d "Values:Invoices.Quantity"
pbir add visual columnChart "Sales.Report/Overview.Page" --title "Revenue by Brand" \
  -d "Category:Brands.Brand" -d "Values:Invoices.Revenue Net"

# 5. Position the visuals
pbir visuals position "Sales.Report/Overview.Page/Revenue.Visual" --x 20 --y 80 --width 280 --height 180
pbir visuals position "Sales.Report/Overview.Page/Quantity.Visual" --x 320 --y 80 --width 280 --height 180
pbir visuals position "Sales.Report/Overview.Page/Revenue by Brand.Visual" --x 20 --y 280 --width 580 --height 350

# 6. Validate
pbir validate "Sales.Report"
```

---

## Bulk-Format All Visuals

**Goal:** Apply consistent formatting across every visual in a report -- something that would take many clicks in Power BI Desktop.

```bash
# Back up first (always a good idea before bulk changes)
pbir backup "Sales.Report" -m "Before bulk formatting"

# Set title font size on all visuals
pbir set "Sales.Report/**/*.Visual.title.fontSize" --value 14 -f

# Show borders on all visuals
pbir set "Sales.Report/**/*.Visual.border.show" --value true -f
pbir set "Sales.Report/**/*.Visual.border.radius" --value 8 -f
pbir set "Sales.Report/**/*.Visual.border.color" --value "#E0E0E0" -f

# Set background on all visuals
pbir set "Sales.Report/**/*.Visual.background.color" --value "#FFFFFF" -f

# Validate
pbir validate "Sales.Report"
```

> The `-f` flag is required for glob patterns (`**/*.Visual`). This is a safety measure to prevent accidental bulk changes.

---

## Style Through the Theme (Preferred)

**Goal:** Set formatting at the theme level so it applies automatically to all visuals. This is cleaner than setting properties on each visual individually.

```bash
# Set border radius for all visual types via theme
pbir theme set-formatting "Sales.Report" "*.border.radius" --value 8

# Set title font size for cards specifically
pbir theme set-formatting "Sales.Report" "card.title.fontSize" --value 16

# Set line chart markers on
pbir theme set-formatting "Sales.Report" "lineChart.lineStyles.showMarker" --value true

# Update theme colors
pbir theme set-colors "Sales.Report" --primary "#2B579A" --secondary "#217346"

# Check the result
pbir theme colors "Sales.Report"
pbir validate "Sales.Report"
```

---

## Add Extension Measures

**Goal:** Add DAX measures that live in the report (not the semantic model). Useful for formatting measures, conditional formatting, or calculations specific to this report.

```bash
# Add a formatting measure for conditional formatting
pbir dax measures add "Sales.Report" -t _Fmt -n "StatusColor" \
  -e 'IF([Revenue Net] > [Budget Amount], "good", "bad")' --data-type Text

# Add a KPI measure
pbir dax measures add "Sales.Report" -t Invoices -n "Revenue vs Budget" \
  -e '[Revenue Net] - [Budget Amount]'

# List all extension measures
pbir dax measures list "Sales.Report"

# Apply the formatting measure as conditional formatting on a visual
pbir visuals cf "Sales.Report/Overview.Page/Revenue.Visual" \
  --measure "labels.color _Fmt.StatusColor"
```

---

## Download, Edit, Publish

**Goal:** Download a report from a Fabric workspace, make local changes, and publish it back.

```bash
# Download from workspace
pbir download "MyWorkspace.Workspace/Sales.Report" -o ./working --format pbir
cd working

# Explore the report
pbir tree "Sales.Report" -v

# Make changes
pbir visuals title "Sales.Report/Overview.Page/Revenue.Visual" --text "Net Revenue" --bold
pbir pages rename "Sales.Report/Dashboard.Page" "Executive Summary"

# Validate before publishing
pbir validate "Sales.Report"

# Publish back
pbir publish "Sales.Report" "MyWorkspace.Workspace/Sales.Report" -f
```

> Requires [Fabric CLI](https://learn.microsoft.com/fabric/cli/) to be installed and authenticated.

---

## Audit a Report

**Goal:** Understand what's in an existing report -- useful for reviewing someone else's work or before making changes.

```bash
# Overall structure
pbir tree "Sales.Report" -v

# Theme colors
pbir theme colors "Sales.Report"

# All fields used across the report
pbir fields list "Sales.Report"

# All filters
pbir filters list "Sales.Report"

# All extension measures
pbir dax measures list "Sales.Report"

# Full validation (schema + fields + quality)
pbir validate "Sales.Report" --all
```

---

## Recover from Mistakes

**Goal:** Undo changes when something goes wrong.

```bash
# List available backups
pbir backup --list "Sales.Report"

# Restore the most recent backup
pbir restore "Sales.Report"

# Or restore a specific backup
pbir restore "Sales.Report" --backup-id 20260312T143000Z
```

If you haven't been making backups, and the report is in a git repository, use `git` to revert:

```bash
git diff                     # See what changed
git checkout -- "Sales.Report/"  # Revert all changes to the report
```
