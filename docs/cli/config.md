# Configuration and Setup

Configure how pbir behaves and install AI agent skills. Most users won't need to change configuration -- the defaults work out of the box.

## config

Configuration stored at `~/.config/pbir/config.json`. Override with `PBIR_CONFIG` env var.

```bash
pbir config show                                           # Show current config
pbir config init                                           # Create default config
pbir config set debug true                                 # Set a value
pbir config set output json
pbir config paths                                          # Show resolved paths
```

| Command | Description |
|---------|-------------|
| `show` | Show current configuration |
| `init` | Create default `config.json` |
| `set` | Set a configuration value |
| `paths` | Show paths for config, templates, schemas |

## setup

Install agent skills for AI coding assistants (Claude Code, Cursor, etc.).

```bash
pbir setup                                                 # Install skills
pbir setup --claude-code                                   # Claude Code specific
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PBIR_QUIET=1` | Suppress animations, tips, and spinners |
| `PBIR_DEBUG=1` | Enable Python tracebacks on error |
| `PBIR_CONFIG` | Override config file path |

## Global Flags

Place these **before** the subcommand:

```bash
pbir -q ls "Report.Report"                                 # Quiet mode
pbir --debug validate "Report.Report"                      # Debug mode
```

| Flag | Description |
|------|-------------|
| `-q, --quiet` | Reduce output |
| `--debug` | Enable debug logging |
| `-V, --version` | Show version |
| `-i, --interactive` | Launch interactive browser mode |
| `--show-legacy` | Find legacy format reports |
