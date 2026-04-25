# Project Guidelines

## Overview

`motd` is a single-file Bash script (`./motd`) that displays a system information dashboard. It is intended to be run at login (Message of the Day) and outputs stats such as hostname, distro, kernel, health summary, CPU load, memory, disk, uptime, and pending updates.

A companion `install.sh` handles installation, upgrades, and uninstallation of `motd` to `/usr/local/bin`.

## Code Style

- Bash with `set -euo pipefail`
- Functions use snake_case
- Color-coded output via ANSI escape sequences (`$'\033[...]'` syntax), set to empty strings when stdout is not a terminal
- Commands that may fail in pipes or substitutions use `|| true` to stay compatible with `set -euo pipefail`
- Section headers use the double-separator style:
  ```
  # ──────────────────────────────────────────────────────────────────────────────
  # Section name
  # ──────────────────────────────────────────────────────────────────────────────
  ```

## Architecture

- **Single file**: All logic lives in `./motd` — no external scripts or libraries
- **Modes**: Default mode shows core stats; `--full` / `-f` enables extended output
- **Caching**: Public IP is cached for 3 hours under `${XDG_CACHE_HOME:-~/.cache}/motd-public-ip`
- **Health summary**: Computed from load, memory, swap, disk, and pending security updates; shown as OK or WARNING with reasons

## Conventions

- The color block must always use the `[ -t 1 ]` terminal check and fall back to empty strings
- CPU temperature is collected in `--full` mode via `sensors` (lm-sensors) or `/sys/class/thermal/thermal_zone0/temp` as a fallback
- Update `README.md` whenever the script changes (new features, new options, changed behavior)
- Format all Markdown files with `prettier` after editing (`prettier --write *.md`)

## Versioning

The version is hardcoded as `VERSION` near the top of `./motd`. Follow semver:

- **Patch** (`1.0.x`): bug fixes, behavioral tweaks, internal refactors
- **Minor** (`1.x.0`): new output fields or new options
- **No bump**: doc-only changes (`README.md`, `AGENTS.md`, comments)

## Validation

There is no test suite. After making changes, verify syntax with:

```sh
bash -n motd
```

Then do a quick smoke-test by running `./motd` and `./motd --full`.
