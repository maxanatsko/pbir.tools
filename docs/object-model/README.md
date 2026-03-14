# Python Object Model

Python API for programmatic Power BI report manipulation.

## Install

```bash
pip install -e ./pbir-object-model
```

## Quick Start

```python
from pbir_object_model import Report

# Load a report
report = Report.load("MyReport.Report")

# Iterate pages and visuals
for page in report.pages:
    print(f"Page: {page.display_name}")
    for visual in page.visuals:
        print(f"  {visual.name} ({visual.visual_type})")

# Modify a visual
visual = report.pages[0].visuals[0]
visual.title.text = "New Title"
visual.title.show = True
visual.save()
```

Always call `.save()` after mutations.

---

## Core Classes

### Report

Main entry point. Load with `Report.load(path)`.

```python
from pbir_object_model import Report

report = Report.load("Report.Report")

# Properties
report.display_name
report.export_data_mode

# Collections
report.pages          # List of Page objects
report.filters        # Report-level filters
report.bookmarks      # Report bookmarks
report.theme          # Theme object
report.semantic_model # SemanticModel object

# Methods
report.save()
report.validate()
report.clone("NewReport.Report")
report.add_extension_measure(table="Sales", name="Total", expression="SUM('Sales'[Revenue])")
```

### Page

Access via `report.pages`.

```python
page = report.pages[0]

# Properties
page.display_name
page.width
page.height
page.is_hidden
page.display_option    # DisplayOption.FitToPage, FitToWidth, ActualSize
page.page_type         # PageType.Default, Tooltip, DrillThrough

# Collections
page.visuals           # List of Visual objects
page.filters           # Page-level filters

# Methods
page.save()
page.delete()
page.clone("NewPage")
page.add_visual(visual_type="card", x=100, y=100, width=300, height=200)
```

### Visual

Access via `page.visuals`.

```python
visual = page.visuals[0]

# Position and size
visual.x, visual.y, visual.width, visual.height
visual.visual_type     # "card", "lineChart", "columnChart", etc.
visual.name

# Schema-driven components (~170+ containers)
visual.title           # .show, .text, .fontSize, .fontFamily, .bold, .italic, .color
visual.subtitle        # Same as title
visual.background      # .show, .color, .transparency
visual.border          # .show, .color, .radius, .width
visual.legend          # .show, .position, .fontSize
visual.categoryAxis    # (alias: axis.x)
visual.valueAxis       # (alias: axis.y)
visual.labels          # .show, .fontSize, .color

# Field binding
visual.add_field(role="Values", entity="Sales", property_name="Revenue")
visual.remove_field(role="Values", entity="Sales", property_name="Revenue")

# Methods
visual.save()
visual.delete()
visual.clone("NewVisual")
```

### Theme

Access via `report.theme`.

```python
theme = report.theme

# Color palette
theme.data_colors      # List of hex color strings

# Theme cascade priority:
# Visual override > VisualType theme > Wildcard theme > Schema default
```

### Filter

Factory methods for different filter types.

```python
from pbir_object_model import Filter

# Basic filter
f = Filter.categorical(entity="Date", property_name="Year")
f.is_hidden = True
f.is_locked = False

# Other types
Filter.relative_date(entity="Date", property_name="Date")
Filter.top_n(entity="Products", property_name="Name", count=10)
Filter.advanced(entity="Sales", property_name="Revenue")

# Add to report or page
report.filters.append(f)
page.filters.append(f)
```

### Bookmark

```python
from pbir_object_model import Bookmark

bm = Bookmark(name="HighRevenue", display_name="High Revenue View")
bm.capture_data = True
bm.capture_display = True
bm.capture_current_page = True
report.bookmarks.append(bm)
```

### SemanticModel

Introspect the connected data model.

```python
model = report.semantic_model

model.tables               # List of table names
model.search("revenue")   # Find fields by name
model.resolve_field("Sales", "Revenue")  # Validate a field exists
model.summary()           # LLM-optimized summary
```

### Field

```python
from pbir_object_model import Field

field = Field("Sales", "Revenue")
field.entity         # "Sales"
field.property       # "Revenue"
field.full_name      # "Sales.Revenue"
```

---

## Layout Utilities

Align and distribute visuals programmatically.

```python
from pbir_object_model import align_left, align_top, distribute_h, distribute_v

visuals = page.visuals

align_left(visuals)           # Align all to leftmost x
align_top(visuals)            # Align all to topmost y
distribute_h(visuals)         # Even horizontal spacing
distribute_v(visuals)         # Even vertical spacing
```

Available: `align_left`, `align_right`, `align_center_h`, `align_top`, `align_bottom`, `align_center_v`, `distribute_h`, `distribute_v`.

---

## Conditional Formatting

Builder pattern for applying conditional formatting to visuals.

```python
# Gradient (2-color)
visual.apply_conditional_formatting(
    container="labels",
    property_name="color",
    min_color="#FF0000",
    max_color="#00FF00",
    input_measure=Field("Sales", "Revenue")
)

# Rules-based
visual.apply_conditional_formatting(
    container="labels",
    property_name="color",
    rules=[
        {"condition": ">", "value": 1000, "color": "good"},
        {"condition": "<", "value": 0, "color": "bad"},
    ]
)
```

Theme color names for CF: `good`, `bad`, `neutral`, `minColor`, `midColor`, `maxColor`.

---

## Validation

```python
issues = report.validate()
for issue in issues:
    print(f"[{issue.severity}] {issue.category}: {issue.message}")
```

Checks schema compliance, field references, and quality (overlaps, hidden visuals, field counts).

---

## Enums

Type-safe enums for configuration:

| Enum | Values |
|------|--------|
| `PageVisibility` | `Visible`, `Hidden` |
| `DisplayOption` | `FitToPage`, `FitToWidth`, `ActualSize` |
| `FilterType` | `Categorical`, `RelativeDate`, `TopN`, `Advanced` |
| `InteractionType` | `Filter`, `Highlight`, `None` |
| `PageType` | `Default`, `Tooltip`, `DrillThrough` |
| `SortDirection` | `Ascending`, `Descending` |
