# Modify and Format

Change how visuals look, set properties, remove objects, and format pages. This is where bulk operations shine -- you can format every visual in a report with one command instead of clicking through the GUI.

> **Glob patterns** like `**/*.Visual` mean "match all visuals in all pages". When you use them with `set`, you must add `-f` (force) as a safety measure to confirm you want to change many things at once.

## set

Set properties on report, page, or visual. Glob patterns require `--force/-f`.

```bash
# Single visual
pbir set "Report.Report/Page.Page/Visual.Visual.title.text" --value "Revenue"
pbir set "Report.Report/Page.Page/Visual.Visual.title.fontSize" --value 14

# Page properties
pbir set "Report.Report/Page.Page.width" --value 1280

# Report settings
pbir set "Report.Report.settings.exportDataMode" --value AllowSummarized

# Bulk with glob (requires -f)
pbir set "Report.Report/**/*.Visual.title.show" --value false -f
pbir set "Report.Report/**/*.Visual.border.show" --value true -f

# JSON input
pbir set "path" --json '{"title": {"show": true, "text": "Sales"}}'
```

| Flag | Description |
|------|-------------|
| `-v, --value` | Property value |
| `-p, --property` | Property path (alternative to dot notation) |
| `--json` | Set value using raw JSON |
| `-f, --force` | Required for glob patterns (bulk operations) |

### Tip: Multi-Property Formatting

For setting multiple properties at once, use the dedicated subcommands instead:

```bash
pbir visuals title "path" --text "Sales" --fontSize 14 --show
pbir visuals background "path" --color "#F0F0F0" --transparency 10
```

## rm

Remove pages, visuals, filters, bookmarks, measures, visual calculations, fields, or theme. All removals require `--force/-f`.

```bash
# Pages and visuals
pbir rm "Report.Report/Page.Page" -f
pbir rm "Report.Report/Page.Page/Visual.Visual" -f

# Filters and bookmarks
pbir rm "Report.Report/filter:FilterName" -f
pbir rm "Report.Report/bookmark:BookmarkName" -f

# Extension measures
pbir rm "Report.Report" --measure "MeasureName" -f
pbir rm "Report.Report" --measures -f              # Remove ALL measures

# Visual calculations
pbir rm "R.Report/P.Page/V.Visual" --viscalc "Name" -f

# Bulk cleanup
pbir rm "Report.Report" --fields -f                # Clear all field bindings
pbir rm "Report.Report" --annotations -f           # Remove all annotations
pbir rm "Report.Report" --theme -f                 # Remove custom theme
pbir rm "Report.Report" --image logo.png -f        # Remove image resource
pbir rm "Report.Report" --all -f                   # Remove filters, bookmarks, annotations
```

| Flag | Description |
|------|-------------|
| `-f, --force` | Skip confirmation (required) |
| `--annotations` | Remove annotations |
| `--measures` | Remove ALL extension measures |
| `--measure <name>` | Remove a single measure |
| `--viscalc <name>` | Remove a single visual calculation |
| `--theme` | Remove custom theme |
| `--fields` | Clear all field bindings |
| `--image <name>` | Remove an image from resources |
| `-a, --all` | Remove filters, bookmarks, and annotations |

## visuals

Visual operations: positioning, formatting, data binding, conditional formatting, and more.

Create visuals with `pbir add visual`. Delete with `pbir rm`. Copy with `pbir cp`.

### Layout

```bash
pbir visuals position "path" --x 100 --y 50 --width 500 --height 300
pbir visuals resize "path" --width 500 --height 300
pbir visuals align "Report.Report/Page.Page" left Visual1 Visual2 Visual3
pbir visuals align "Report.Report/Page.Page" distribute-horizontal Visual1 Visual2 Visual3
pbir visuals snap "path" --grid 10
pbir visuals z-order "path" --front
pbir visuals z-order "path" --back
```

### Formatting

All formatting subcommands accept glob paths for bulk operations.

```bash
pbir visuals title "path" --text "Revenue" --fontSize 14 --show --bold
pbir visuals title "path" --no-show
pbir visuals subtitle "path" --text "YTD" --fontSize 10 --italic
pbir visuals background "path" --color "#F8F9FA" --transparency 10 --show
pbir visuals border "path" --color "#E0E0E0" --radius 8 --width 1 --show
pbir visuals shadow "path" --show --color "#00000020" --offset 2
pbir visuals padding "path" --top 10 --bottom 10 --left 12 --right 12
pbir visuals spacing "path" --value 8
pbir visuals header "path" --show
pbir visuals divider "path" --show --color "#CCCCCC"
```

### Chart-Specific

```bash
pbir visuals legend "path" --show --position top --fontSize 10
pbir visuals axis "path" --type category --show --fontSize 10
pbir visuals axis "path" --type value --show --gridlines
pbir visuals labels "path" --show --fontSize 10 --color "#333333"
pbir visuals sort "path" --by "Sales.Revenue" --direction descending
```

### Data Binding

```bash
pbir visuals bind "path" -a "Category:Products.Name"     # Add field to role
pbir visuals bind "path" -r "Values:Sales.Revenue"        # Remove field
pbir visuals bind "path" -c "Values"                       # Clear role
pbir visuals bind "path" --list-roles                      # Show available roles
```

### Conditional Formatting

```bash
pbir visuals cf "path" --list                              # List CF rules
pbir visuals cf "path" --measure "labels.color _Fmt.StatusColor"  # Apply
pbir visuals cf "path" --remove "labels.color"             # Remove
pbir visuals cf "source" --copy-to "target"                # Copy between visuals
```

### Scripted Visuals

```bash
pbir visuals deneb "path" --spec-file chart.json
pbir visuals python "path" --script-file script.py
pbir visuals r "path" --script-file script.r
```

### Inspection

```bash
pbir visuals format "path"                     # Show merged theme + visual formatting
pbir visuals format "path" -p lineStyles       # Filter to container
pbir visuals properties "path"                 # Tree view of properties
pbir visuals properties -s "marker"            # Fuzzy search
pbir visuals clear-formatting "path" --dry-run # Preview clearing overrides
pbir visuals hide "path" --no-show             # Hide visual
pbir visuals query "path"                      # Extract DAX query
```

## pages

Page operations: rename, resize, type, background, wallpaper, and more.

Create pages with `pbir add page`. Delete with `pbir rm`. Copy with `pbir cp`. Move between reports with `pbir mv`.

```bash
pbir pages rename "Report.Report/Old Name.Page" "New Name"
pbir pages resize "Report.Report/Page.Page" --width 1920 --height 1080
pbir pages type "Report.Report/Page.Page" --type 16:9     # Preset: 16:9, 4:3, letter, tooltip, custom
pbir pages background "Report.Report/Page.Page" --color "#F0F8FF"
pbir pages background "Report.Report/Page.Page" --image bg.png
pbir pages wallpaper "Report.Report/Page.Page" --color "#2B579A"
pbir pages move "Report.Report/Sales.Page" --to 1          # Reorder in tab bar
pbir pages active-page "Report.Report" "HomePage"           # Set landing page
pbir pages display "Report.Report/Page.Page" --mode FitToPage
pbir pages hide "Report.Report/Page.Page" --hidden
pbir pages interactions "Report.Report/Page.Page" --list
pbir pages interactions "Report.Report/Page.Page" --set Visual1 Visual2 none
```
