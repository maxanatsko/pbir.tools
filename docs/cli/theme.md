# Theme and Schema

Inspect and modify report themes, and discover what properties are available on visual types.

Themes control the default look of every visual in a report (colors, fonts, borders). Setting formatting at the theme level is preferred over formatting each visual individually -- it's more consistent and easier to maintain.

## theme

### Inspect

```bash
pbir cat "Report.Report/theme"                 # Full theme JSON
pbir theme colors "Report.Report"              # Color palette
pbir theme text-classes "Report.Report"        # Typography definitions
pbir theme fonts "Report.Report"               # Font families
```

### Modify Colors

```bash
pbir theme set-colors "Report.Report" --primary "#2B579A" --secondary "#217346"
pbir theme colors "Report.Report" --replace "#FF0000" "#CC0000"
pbir theme colors "Report.Report" --normalize
```

### Modify Typography

```bash
pbir theme set-text-classes "Report.Report" --class title --fontSize 20 --bold
pbir theme text-classes "Report.Report" --set label --fontSize 10
```

### Modify Visual Formatting

Set formatting at the theme level (applies to all visuals of a type or globally).

```bash
# Wildcard (all visual types)
pbir theme set-formatting "Report.Report" "*.border.radius" --value 8

# Specific visual type
pbir theme set-formatting "Report.Report" "card.*.border.show" --value true
pbir theme set-formatting "Report.Report" "lineChart.lineStyles.showMarker" --value true

# Push a visual's formatting to the theme
pbir theme push-visual "Report.Report/Page.Page/Visual.Visual"
```

### Background and Icons

```bash
pbir theme background "Report.Report" --image wallpaper.png
pbir theme icons "Report.Report" --list
pbir theme icons "Report.Report" --add icon.svg --name "custom-icon"
```

### Serialize / Build Workflow

Split a theme into editable files, modify, then rebuild:

```bash
pbir theme serialize "Report.Report" -o CustomTheme.Theme    # Split into files
# ... edit individual files ...
pbir theme build "CustomTheme.Theme"                          # Merge back
```

### Templates

```bash
pbir theme list-templates
pbir theme create-template "Report.Report" --name "my-brand"
pbir theme apply-template "Report.Report" --template "my-brand"
```

### Validate / Diff / Rename

```bash
pbir theme validate "Report.Report"
pbir theme diff "Report1.Report" "Report2.Report"
pbir theme rename "Report.Report" "New Theme Name"
```

## schema

Discover visual properties and manage PBIR schemas.

### Property Discovery

Use these commands to find the right container and property name before formatting.

```bash
pbir schema types                              # All available visual types
pbir schema containers card                    # Containers for a visual type
pbir schema describe card.title                # Properties with types, ranges, enums
pbir schema roles card                         # Data roles (field wells)
```

### Schema Management

```bash
pbir schema status                             # Compare local vs remote schema versions
pbir schema fetch --yes                        # Download latest schemas
pbir schema check "Report.Report"              # Check report schema versions
pbir schema upgrade "Report.Report"            # Upgrade to latest
pbir schema regenerate                         # Regenerate flattened schemas
```

### Typical Property Discovery Workflow

```bash
# 1. What containers exist for this visual type?
pbir schema containers lineChart

# 2. What properties does a container have?
pbir schema describe lineChart.lineStyles

# 3. What's currently set on a live visual?
pbir visuals format "Report.Report/Page.Page/Visual.Visual" -p lineStyles

# 4. Set the property
pbir set "Report.Report/Page.Page/Visual.Visual.lineStyles.showMarker" --value true
```
