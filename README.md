# motd

A single-file Bash script that displays a system information dashboard at login (Message of the Day).

Shows hostname, distro, kernel, health summary, CPU load, memory, disk, uptime, and more. Supports a `--full` mode for extended output including swap, Docker containers, logged-in users, private/public IP, last login, and failed SSH logins.

## Install

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/milvasic/motd/refs/heads/main/install.sh)"
```

This installs `motd` to `/usr/local/bin`. If a previous version is already installed, the installer will prompt you to upgrade.

For non-interactive environments (CI, scripts), pass `--yes` to auto-approve upgrades:

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/milvasic/motd/refs/heads/main/install.sh)" -- --yes
```

`wget` is also supported if `curl` is not available.

## Uninstall

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/milvasic/motd/refs/heads/main/install.sh)" -- --uninstall
```

This removes `motd` from `/usr/local/bin`.

## Usage

```
motd [show] [--full | -f]
motd update
```

| Command  | Description                                                              |
| -------- | ------------------------------------------------------------------------ |
| `show`   | Display the system dashboard (default — `motd` with no args runs `show`) |
| `update` | Self-update `motd` to the latest version via its installer               |

Run `motd` with no arguments for the standard view. Pass `--full` (or `-f`) for the extended view.

### Standard output

| Field           | Description                                         |
| --------------- | --------------------------------------------------- |
| Health          | OK or WARNING, with reasons (load/memory/disk/etc.) |
| Load avg        | 1m / 5m / 15m load averages with process count      |
| Memory          | Used vs total in GiB with percentage bar            |
| Disk            | Root filesystem used vs total with percentage bar   |
| Uptime          | Human-readable uptime                               |
| Pending updates | Available and security updates (Ubuntu/Debian)      |

### Full mode (`--full` / `-f`)

Includes everything in the standard view, plus:

| Field      | Description                                                           |
| ---------- | --------------------------------------------------------------------- |
| CPU temp   | CPU temperature (via `sensors` or `/sys/class/thermal`, if available) |
| Swap       | Swap used vs total with percentage bar                                |
| Docker     | Number of running containers (if Docker is present)                   |
| Users      | Logged-in users and unique count                                      |
| Private IP | First IP from `hostname -I`                                           |
| Public IP  | Public IP address (requires `curl`, cached 3 hours)                   |
| Last login | Last login time for the current user                                  |
| Failed SSH | Failed SSH login attempts in the last 10 days (requires `journalctl`) |

### Auto-run on login

To show the dashboard automatically on each login, add to `~/.bashrc` or `~/.zshrc`:

```sh
motd
```

Or for full mode:

```sh
motd --full
```

### Self-update

```sh
motd update
```

Downloads and runs the latest installer from GitHub, replacing the installed binary.

## Optional dependencies

`motd` works without any extra packages. The following are used when available:

| Tool         | Purpose                                              |
| ------------ | ---------------------------------------------------- |
| `figlet`     | Renders the hostname as a large ASCII banner         |
| `curl`       | Fetches public IP (in `--full` mode, cached 3 hours) |
| `lm-sensors` | CPU temperature in `--full` mode                     |

If optional tools are missing and the terminal is interactive, `motd` will offer to install them via `apt`.

## License

[MIT](LICENSE)
