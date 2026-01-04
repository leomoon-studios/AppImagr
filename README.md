# AppImagr

A simple Bash tool that acts as a package manager for GitHub-hosted AppImages. It automatically fetches the latest releases, installs them to `/usr/local/bin`, downloads icons, and creates desktop entries for full system integration.

**No configuration needed** — the app list and icons are fetched directly from this repository.

## Features

- **One-Command Install**: Install `appimagr` with a single `curl` command.
- **Always Up-to-Date**: App list (`apps.yaml`) and icons are fetched from GitHub on every run.
- **Self-Updating**: Update `appimagr` itself with `--update`.
- **Automated Downloads**: Fetches the latest AppImage releases from GitHub repositories.
- **System Integration**: Installs binaries to `/usr/local/bin` and creates `.desktop` files.
- **Icon Support**: Automatically downloads and installs SVG/PNG icons.
- **Architecture Filtering**: Automatically selects `x86_64` builds and filters out ARM versions.
- **apt-like Interface**: Shows what will be installed and asks for confirmation.
- **Cache Updates**: Automatically updates desktop database and icon caches.

## Installation

Install `appimagr` system-wide with a single command:

```bash
sudo curl -fL -o /usr/local/bin/appimagr https://raw.githubusercontent.com/leomoon-studios/AppImagr/master/appimagr && sudo chmod +x /usr/local/bin/appimagr
```

## Prerequisites

- Linux environment
- `curl`
- `sudo` privileges (for installation)

## Usage

```bash
# Show help (no sudo required)
appimagr --help

# List all available apps (no sudo required)
appimagr --list

# Install/update a specific app
sudo appimagr pcsx2

# Install/update multiple apps at once
sudo appimagr cura pcsx2 imhex

# Install/update all available apps
sudo appimagr --all

# Skip confirmation prompt (like apt -y)
sudo appimagr -y pcsx2
sudo appimagr --yes --all

# Update appimagr itself to the latest version
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
| `binary`           | Yes      | Name of the executable in `/usr/local/bin`             |
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
- `--help` and `--list` work without sudo.
- All other operations require `sudo` because they write to `/usr/local/`.
