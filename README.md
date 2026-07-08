# AppImagr

A simple Bash tool that acts as a package manager for GitHub-hosted AppImages. It automatically fetches the latest releases, installs them to `~/Applications`, downloads icons, and creates desktop entries for full system integration.

**No configuration needed** — the app list and icons are fetched directly from this repository.

**No sudo required** — everything is installed to user-local directories by default. Use `--system` for system-wide installation.

## Features

- **One-Command Install**: Install `appimagr` with a single `curl` command.
- **Always Up-to-Date**: App list (`apps.yaml`) and icons are fetched from GitHub on every run.
- **Self-Updating**: Update `appimagr` itself with `--update`.
- **Automated Downloads**: Fetches the latest AppImage releases from GitHub repositories.
- **User-Local Installation**: Installs AppImages to `~/Applications` and creates `.desktop` files in `~/.local/share/applications`.
- **System-Wide Installation**: Optional `--system` flag installs to `/opt/appimages` for all users (requires sudo).
- **Custom Bin Directory**: Optional `--bin-dir` flag overrides the AppImage install directory independently of `--system`.
- **Icon Support**: Automatically downloads and installs SVG/PNG icons to `~/.local/share/icons`.
- **Architecture Filtering**: Automatically selects `x86_64` builds and filters out ARM versions.
- **apt-like Interface**: Shows what will be installed and asks for confirmation.
- **Cache Updates**: Automatically updates desktop database and icon caches.

## Installation

Install `appimagr` with a single command:

```bash
sudo curl -fL -o /usr/local/bin/appimagr https://raw.githubusercontent.com/leomoon-studios/AppImagr/refs/heads/master/appimagr && sudo chmod +x /usr/local/bin/appimagr
```

## Prerequisites

- Linux environment
- `curl`
- `sudo` privileges (only for `--update` or `--system`)

## Usage

```bash
# Show help
appimagr --help

# List all available apps
appimagr --list

# Install/update a specific app (user-local)
appimagr pcsx2

# Install/update multiple apps at once
appimagr cura pcsx2 imhex

# Install/update all available apps
appimagr --all

# Skip confirmation prompt (like apt -y)
appimagr -y pcsx2
appimagr --yes --all

# Install system-wide to /opt/appimages (requires sudo)
sudo appimagr --system pcsx2
sudo appimagr -s --all

# Install AppImage to a custom directory
appimagr --bin-dir ~/bins pcsx2

# System-wide icons/desktop entries, but custom AppImage directory
sudo appimagr --system --bin-dir /opt/custom pcsx2

# Update appimagr itself to the latest version (requires sudo)
sudo appimagr --update
```

The script will show you a list of apps to be installed and ask for confirmation before proceeding (similar to `apt`). Use `-y` or `--yes` to skip the prompt.

## How It Works

1. **Fetches `apps.yaml`** from this GitHub repository to get the list of available apps.
2. **Matches your request** against the app list (case-insensitive).
3. **Shows confirmation** of what will be installed.
4. **Downloads the latest AppImage** from each app's GitHub releases.
5. **Downloads the icon** from this repository.
6. **Creates a `.desktop` file** for system menu integration.
7. **Updates system caches** (desktop database and icon cache).

## Available Apps

Run `appimagr --list` to see all available applications, or check the [apps.yaml](apps.yaml) file.

## Contributing

Want to add a new app? Open a PR that adds an entry to `apps.yaml` and the corresponding icon to the `icons/` directory.

### App Configuration Format (`apps.yaml`)

```yaml
- name: Application Name
  repo: https://github.com/username/repository
  binary: binary-name-on-system
  icon: icons/icon-filename.svg
  comment: Description of the app
  categories: Utility;Development;
  startup_wm_class: AppName
  mime_type: application/x-extension;
```

| Field              | Required | Description                                            |
| ------------------ | -------- | ------------------------------------------------------ |
| `name`             | Yes      | Display name (used in desktop entry)                   |
| `repo`             | Yes      | GitHub repository URL                                  |
| `binary`           | Yes      | Name of the AppImage in `~/Applications`               |
| `icon`             | Yes      | Path to icon file in this repo (e.g., `icons/app.svg`) |
| `comment`          | No       | Tooltip description                                    |
| `categories`       | No       | Semicolon-separated menu categories                    |
| `startup_wm_class` | No       | Window manager class for grouping                      |
| `mime_type`        | No       | Semicolon-separated MIME types                         |

### Example Entry

```yaml
- name: PCSX2
  repo: https://github.com/PCSX2/pcsx2
  binary: pcsx2
  icon: icons/pcsx2.svg
  comment: Playstation 2 Emulator
  categories: Game;Emulator;
  startup_wm_class: PCSX2
```

## Repository Structure

```
.
├── apps.yaml           # App definitions (fetched remotely)
├── icons/              # App icons (fetched remotely)
├── appimagr            # Main script
└── README.md
```

## Notes

- The script filters for `.AppImage` files and excludes filenames containing "arm" to target x86_64 systems.
- AppImages are saved with the `.AppImage` extension.
- **User-local (default)**: `~/Applications`, `~/.local/share/applications`, `~/.local/share/icons`
- **System-wide (`--system`)**: `/opt/appimages`, `/usr/local/share/applications`, `/usr/local/share/icons`
- Only `--update` and `--system` require sudo. `--bin-dir` alone does not require sudo unless the target path requires elevated permissions.
