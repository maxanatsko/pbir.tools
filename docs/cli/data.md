# Data, Fields, Filters, DAX, and Bookmarks

Connect visuals to data (fields), control what data users see (filters), add report-level calculations (DAX measures), and save report states (bookmarks).

> **Fields** are columns and measures from your semantic model. When you "bind" a field to a visual, you're telling that visual what data to show -- like dragging a column into a field well in Power BI Desktop.

## fields

List, search, replace, and manage field references across reports.

```bash
pbir fields list                                           # All reports
pbir fields list "Report.Report"                           # All fields in report
pbir fields list "Report.Report/Page.Page"                 # Fields on a page
pbir fields find "revenue" "Report.Report"                 # Fuzzy search
pbir fields replace "Report.Report" --from "Sales.Revenue" --to "Finance.Revenue"
pbir fields rename "Report.Report" --field "Sales.Revenue" --display-name "Total Revenue"
pbir fields add "Report.Report/Page.Page/Visual.Visual" --role Values --field "Sales.Revenue"
pbir rm "Report.Report" --fields -f                        # Clear all field bindings
```

## filters

Filter operations and filter pane management.

Add filters with `pbir add filter`. Delete with `pbir rm "Report.Report/filter:Name" -f`.

```bash
# List
pbir filters list                                          # All reports
pbir filters list "Report.Report"                          # All filters in report
pbir filters list "Report.Report/Page.Page"                # Page-level filters

# Set and clear
pbir filters set "Report.Report/Date.Filter" --type RelativeDate
pbir filters set "Report.Report/Year.Filter" --values 2024 2025
pbir filters clear "Report.Report/Year.Filter"

# Visibility and locking
pbir filters hide "Report.Report/Month.Filter"
pbir filters lock "Report.Report/Date.Filter"
pbir filters unlock "Report.Report/Date.Filter"

# Rename
pbir filters rename "Report.Report/OldName.Filter" "New Display Name"
```

### Filter Pane

```bash
pbir filters pane-hide "Report.Report"                     # Hide filter pane
pbir filters pane-collapse "Report.Report"                 # Collapse by default
pbir filters pane-get "Report.Report"                      # Current pane config
pbir filters pane-set "Report.Report" --width 320          # Style (theme level)
pbir filters pane-card "Report.Report" -s Applied --bg-color "#E3F2FD"
```

## dax

DAX operations: extension measures (thin report measures) and visual calculations.

### Extension Measures

Extension measures are DAX measures stored in the report (not the semantic model). They appear in `reportExtensions.json`.

```bash
pbir dax measures list "Report.Report"
pbir dax measures add "Report.Report" -t Sales -n "Total Revenue" -e "SUM('Sales'[Revenue])"
pbir dax measures add "Report.Report" -t _Fmt -n "StatusColor" -e 'IF([Sales]>[Target],"good","bad")' --data-type Text
pbir dax measures update "Report.Report" -n "Total Revenue" -e "SUMX('Sales', [Qty] * [Price])"
pbir dax measures rename "Report.Report" -n "OldName" --new-name "NewName"
pbir dax measures clear "Report.Report"                    # Remove all measures
pbir dax measures json "Report.Report"                     # Raw JSON
```

### Visual Calculations

Visual calculations are DAX calculations embedded directly in a visual (e.g., `RUNNINGSUM`, `RANK`).

```bash
pbir dax viscalcs list "Report.Report"
pbir dax viscalcs add "R.Report/P.Page/V.Visual" -n "RunningTotal" -e "RUNNINGSUM([Revenue])"
pbir dax viscalcs update "R.Report/P.Page/V.Visual" -n "RunningTotal" -e "RUNNINGSUM([Net Revenue])"
```

## bookmarks

Bookmark operations: list, rename, configure data/display capture, set affected visuals.

Delete bookmarks with `pbir rm "Report.Report/bookmark:Name" -f`.

```bash
# List and rename
pbir bookmarks list "Report.Report"
pbir bookmarks rename "Report.Report" "OldName" "New Name"

# Data capture (filter/slicer state)
pbir bookmarks data "Report.Report" "BookmarkName" --on
pbir bookmarks data "Report.Report" "BookmarkName" --off

# Display capture (visual visibility)
pbir bookmarks display "Report.Report" "BookmarkName" --off

# Current page capture
pbir bookmarks current-page "Report.Report" "BookmarkName" --off

# Affected visuals
pbir bookmarks visuals "Report.Report" "BookmarkName" --all
```

## annotations

Annotations are metadata key-value pairs attached to reports, pages, or visuals. They are ignored by Power BI and can be used for documentation, versioning, or automation metadata.

```bash
pbir annotations list "Report.Report"
pbir annotations update "Report.Report" version "2.0"
pbir annotations rename "Report.Report" old_key new_key
pbir add annotation "Report.Report" --name version --value "1.0"
pbir rm "Report.Report" --annotations -f                   # Remove all
```
