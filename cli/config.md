# Configuration

These commands control how `pbir` behaves on your machine.

Most users will only touch this section occasionally, usually when:

- setting a preferred output or debug mode
- checking where config and schema files live

## `pbir config`

`pbir config` manages the CLI configuration file.

```bash
pbir config show
pbir config init
pbir config set debug true
pbir config set output json
pbir config paths
```

Use it when you want to make the CLI behave more predictably across sessions instead of passing the same flags every time.

| Command | Purpose |
|---|---|
| `show` | Display the current configuration |
| `init` | Create a default config file |
| `set` | Update a config value |
| `paths` | Show where config, templates, and schemas are resolved from |

## Environment Variables

Environment variables are useful when you want the same behavior in every terminal session.

| Variable | Purpose |
|---|---|
| `PBIR_QUIET=1` | Reduce tips, spinners, and animations |
| `PBIR_DEBUG=1` | Show debug logging and tracebacks |
| `PBIR_CONFIG` | Override the config file path |

## Global Flags

Global flags go before the subcommand:

```bash
pbir -q ls "Report.Report"
pbir --debug validate "Report.Report"
```

| Flag | Purpose |
|---|---|
| `-q`, `--quiet` | Reduce non-essential output |
| `--debug` | Enable debug logging |
| `-V`, `--version` | Show version |
| `-i`, `--interactive` | Launch interactive browser mode |
| `--show-legacy` | Include legacy reports that still need conversion |
