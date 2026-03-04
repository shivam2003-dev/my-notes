# Modern CLI Tools Cheatsheet (macOS + Linux)

This README explains each tool installed by the scripts, when to use it, and quick examples.

## Install

macOS:

```bash
./install.sh
```

Linux:

```bash
./install-linux.sh
```

Linux script supports `apt`, `dnf`, `pacman`, and `zypper`. It installs what is available and reports missing packages.

Poster gap check:
- Added `ss` support:
  - Linux via `iproute2` / `iproute`
  - macOS via `iproute2mac`
- `eza --tree` is a usage mode of `eza`, not a separate install.
- `curl/wget -> ss` in many posters is misleading; `ss` is for socket stats, not HTTP requests.

## Install one tool (individual command)

macOS (Homebrew):

```bash
brew install <tool>
```

Linux:

```bash
# apt (Debian/Ubuntu)
sudo apt-get install -y <package>

# dnf (Fedora/RHEL)
sudo dnf install -y <package>

# pacman (Arch)
sudo pacman -S --noconfirm --needed <package>

# zypper (openSUSE)
sudo zypper --non-interactive install --no-recommends <package>
```

Common package-name differences:

| Logical tool | macOS (brew) | apt | dnf | pacman | zypper |
|---|---|---|---|---|---|
| `ss` | `iproute2mac` | `iproute2` | `iproute` | `iproute2` | `iproute2` |
| `fd` | `fd` | `fd-find` | `fd-find` (or `fd`) | `fd` | `fd` |
| `bat` | `bat` | `bat` (command may be `batcat`) | `bat` | `bat` | `bat` |
| `git-delta` | `git-delta` | `git-delta` (or `delta`) | `git-delta` | `git-delta` | `git-delta` |
| `dust` | `dust` | `dust` (or `du-dust`) | `dust` | `dust` | `dust` |

Examples:

```bash
# macOS: install only ripgrep
brew install ripgrep

# Ubuntu/Debian: install only fd
sudo apt-get install -y fd-find

# Fedora: install only ss support
sudo dnf install -y iproute

# Arch: install only git-delta
sudo pacman -S --noconfirm --needed git-delta
```

## Tool cheatsheet

| Tool | Best used for | Use when | Quick example |
|---|---|---|---|
| `eza` | Better `ls` | You want readable file listings with icons/tree/git info | `eza -la --git` |
| `btop` | System monitor | You need live CPU/RAM/disk/process view | `btop` |
| `bat` | Better `cat` | You want syntax highlighting + line numbers for files | `bat install.sh` |
| `dust` | Disk usage by directory | You want fast, visual space usage | `dust -d 2` |
| `fd` | Better `find` | You need fast file search with simple syntax | `fd README` |
| `ripgrep` (`rg`) | Better `grep` | You want fast text search in code/logs | `rg "TODO"` |
| `procs` | Better `ps` | You need readable process inspection | `procs --sortd cpu` |
| `duf` | Better `df` | You want filesystem/disk free overview | `duf` |
| `gping` | Ping with graph | You want latency visualization over time | `gping google.com` |
| `mtr` | Ping + traceroute | You want network path + packet loss diagnostics | `mtr google.com` |
| `broot` | Interactive tree navigation | You want to browse dirs and open files quickly | `broot` |
| `git-delta` (`delta`) | Better git diffs | You want syntax-highlighted side-by-side diffs | `git -c core.pager=delta show` |
| `rsync` | Reliable file sync/copy | You need incremental, resumable local/remote copy | `rsync -avh src/ dest/` |
| `atuin` | Shell history search/sync | You want better command history and search | `atuin search docker` |
| `gdu` | Fast disk usage scanner | You need very fast cleanup triage on large folders | `gdu ~/Downloads` |
| `lnav` | Log navigator | You want to inspect logs with filters/SQL-like queries | `lnav /var/log/system.log` |
| `viddy` | Better `watch` | You want periodic command output with diff highlighting | `viddy "ls -la"` |
| `bpytop` | Terminal resource monitor | You want an alternative visual system monitor | `bpytop` |
| `ss` | Socket statistics | You want fast TCP/UDP socket inspection | `ss -tulpen` |

## Which tool to pick quickly

- Find files: `fd`
- Search inside files: `rg`
- Inspect file quickly: `bat`
- Check disk usage by folder: `dust` or `gdu`
- Check running processes: `procs` or `btop`
- Check network latency/path: `gping` or `mtr`
- Explore project tree interactively: `broot`
- Review git diffs cleanly: `git-delta`
- Parse large logs: `lnav`

## Optional setup

Enable `atuin` in zsh:

```bash
atuin init zsh >> ~/.zshrc
source ~/.zshrc
```
