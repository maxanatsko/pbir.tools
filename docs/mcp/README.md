# MCP Server

MCP (Model Context Protocol) server for Power BI report manipulation. Exposes the pbir object model as tools for AI assistants like Claude Code, Cursor, and others.

## Install

### From Source

```bash
pip install -e ./pbir-object-model
pip install -e ./pbir-mcp

# Verify
pbir-mcp --help
```

### Standalone Binary

Build a standalone executable (no Python required on target):

```bash
pip install -e "./pbir-mcp[build]"
cd pbir-mcp
python scripts/build_binary.py --clean

# Output: dist/pbir-mcp (~23 MB)
```

| Platform | Output |
|----------|--------|
| macOS | `dist/pbir-mcp` |
| Windows | `dist/pbir-mcp.exe` |
| Linux | `dist/pbir-mcp` |

---

## Configuration

### Claude Code

Add to your `.mcp.json` or Claude Code MCP settings:

```json
{
  "mcpServers": {
    "pbir": {
      "command": "pbir-mcp"
    }
  }
}
```

Or use the standalone binary:

```json
{
  "mcpServers": {
    "pbir": {
      "command": "/path/to/dist/pbir-mcp"
    }
  }
}
```

Or run as a Python module:

```json
{
  "mcpServers": {
    "pbir": {
      "command": "python",
      "args": ["-m", "pbir_mcp"]
    }
  }
}
```

---

## Session Workflow

The MCP server uses stateful sessions to maintain access to reports. The typical workflow:

```
1. Create a session for a report
2. Describe/inspect the report structure
3. Make changes (create visuals, bind data, format, etc.)
4. Save changes to disk
5. Destroy the session
```

### Example

```text
pbi_session(operation="create", report_path="MyReport.Report")
  -> session_id: "a1b2c3d4"

pbi_report(operation="describe", session_id="a1b2c3d4")
  -> Markdown structure overview

pbi_visual(operation="create", session_id="a1b2c3d4",
           page_name="Overview", visual_type="card",
           x=100, y=100, title="Revenue")
  -> Creates a card visual

pbi_visual_data(operation="bind", session_id="a1b2c3d4",
                page_name="Overview", visual_name="Revenue",
                role="Values", entity="Sales", property_name="TotalRevenue")
  -> Binds data to the visual

pbi_report(operation="save", session_id="a1b2c3d4")
  -> Persists changes to disk

pbi_session(operation="destroy", session_id="a1b2c3d4")
  -> Releases resources
```

### Session Details

- **TTL**: 30 minutes of idle time (configurable via `--ttl-seconds` or `PBIR_MCP_TTL_SECONDS`)
- **Max Sessions**: 10 concurrent (configurable via `--max-sessions` or `PBIR_MCP_MAX_SESSIONS`)
- **Auto-cleanup**: Expired sessions are removed automatically
- **Thread-safe**: Session manager is thread-safe

> [!IMPORTANT]
> Always save before destroying a session. Unsaved changes are lost when a session is destroyed.
> Sessions are ephemeral -- server restart loses all sessions.
> Use `enable_history=True` in session creation for undo/redo support.

---

## Available Tools

### Session Management

| Tool | Operations |
|------|-----------|
| `pbi_session` | `create`, `destroy`, `list` |

### Report Operations

| Tool | Operations |
|------|-----------|
| `pbi_report` | `describe`, `info`, `save`, `get_changes`, `set_property`, `undo`, `redo`, `clone`, `create` |

### Page Operations

| Tool | Operations |
|------|-----------|
| `pbi_page` | `list`, `describe`, `create`, `delete`, `clone`, `set_property`, `render_ascii` |
| `pbi_page_layout` | `find_empty_region`, `detect_collisions` |
| `pbi_page_config` | `interactions`, `tooltips`, `drillthrough`, `backgrounds` |
| `pbi_annotation` | `add`, `remove`, `list` |

### Visual Operations

| Tool | Operations |
|------|-----------|
| `pbi_visual` | `list`, `create`, `delete`, `clone`, `move`, `set_property`, `get_info`, `make_group`, `rename` |
| `pbi_visual_data` | `bind`, `unbind`, `can_bind`, `get_valid_roles`, `get_required_roles` |
| `pbi_visual_layer` | z-order, `snap`, `align`, `copy_formatting` |
| `pbi_visual_action` | `set`, `get`, `clear` click actions |

### Bookmarks, Filters, Theme, Formatting

| Tool | Operations |
|------|-----------|
| `pbi_bookmark` | `list`, `get`, `create`, `delete`, `set_property` |
| `pbi_filter` | `list`, `create`, `delete`, TopN, RelativeDate, `set_values`, `set_where` |
| `pbi_filter_pane` | `show`, `hide`, `expand`, `collapse` |
| `pbi_theme` | colors, visualStyles, text classes, fonts, icons, background image |
| `pbi_cond_format` | `apply`, `get`, `clear` conditional formatting |

### Query, Schema, Model, Validation, I/O

| Tool | Operations |
|------|-----------|
| `pbi_query` | find/query visuals, pages, filters, bookmarks |
| `pbi_schema` | visual types, containers, field type checks |
| `pbi_bulk` | bulk update/delete visuals and pages |
| `pbi_model` | resolve/search fields, list tables/fields, relationships |
| `pbi_validation` | validate report/page/visual (schema, fields, QA) |
| `pbi_discovery` | find reports, detect formats |
| `pbi_io` | connection info, export `.pbix` |

---

## Example: Creating a Dashboard

```python
# 1. Create session
create_session("Sales.Report")
# -> session_id: "abc123"

# 2. Check structure
describe_report("abc123")

# 3. Create KPI cards
create_visual("abc123", "Overview", "card", 20, 20, title="Revenue")
create_visual("abc123", "Overview", "card", 320, 20, title="Orders")
create_visual("abc123", "Overview", "card", 620, 20, title="Profit")

# 4. Bind data
bind_field("abc123", "Overview", "Revenue", "Values", "Sales", "TotalRevenue")
bind_field("abc123", "Overview", "Orders", "Values", "Sales", "OrderCount")
bind_field("abc123", "Overview", "Profit", "Values", "Sales", "GrossProfit")

# 5. Style
set_visual_property("abc123", "Overview", "Revenue", "title.fontSize", 14)
set_visual_property("abc123", "Overview", "Revenue", "background.color", "#F0F0F0")

# 6. Save and cleanup
save_report("abc123")
destroy_session("abc123")
```
