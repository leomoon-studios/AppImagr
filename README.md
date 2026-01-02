# AppImagr

A simple, configurable Bash script to automate the installation of AppImages from GitHub Releases. It downloads the latest version, installs it to `/usr/local/bin`, sets up user-provided icons, and creates desktop entries for system-wide integration.

## Features

- **Automated Downloads**: Fetches the latest release from GitHub repositories.
- **System Integration**: Installs binaries to `/usr/local/bin` and creates `.desktop` files.
- **Icon Support**: Installs user-provided SVG icons to the system icon theme directory.
- **Architecture Filtering**: Automatically selects `x86_64` builds and filters out ARM versions.
- **Configurable**: Easily add or remove apps using a YAML-like configuration file (`apps.yaml`).
- **Cache Updates**: Automatically updates desktop database and icon caches.

## Prerequisites

- Linux environment.
- `curl`
- `sudo` privileges.

## Usage

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    cd <repo-name>
    ```

2.  **Prepare Icons:**
    Place your SVG icons in the `icons/` directory. Ensure the filenames match what you specify in `apps.yaml`.

3.  **Configure Apps:**
    Edit `apps.yaml` to define the applications you want to install.

4.  **Run the Script:**
    ```bash
    chmod +x appimagr.sh
    sudo ./appimagr.sh
    ```

## Configuration (`apps.yaml`)

The configuration uses a simple YAML-like format.

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

- **name**: The display name of the application (used in the Desktop Entry).
- **repo**: The URL to the GitHub repository.
- **binary**: The name you want the executable to have in `/usr/local/bin`.
- **icon**: Relative path to the icon file (must be provided by the user).
- **comment**: (Optional) Tooltip description for the app.
- **categories**: (Optional) Semicolon-separated list of menu categories.
- **startup_wm_class**: (Optional) Helps the window manager group windows.
- **mime_type**: (Optional) Semicolon-separated list of file types the app can open.

### Example

```yaml
- name: PCSX2
  repo: https://github.com/PCSX2/pcsx2
  binary: pcsx2
  icon: icons/pcsx2.svg
  comment: Playstation 2 Emulator
  categories: Game;Emulator;
  startup_wm_class: PCSX2
```

## Directory Structure

```
.
├── apps.yaml           # Configuration file
├── icons/              # Folder containing .svg icons
├── appimagr.sh         # Main installer script
└── README.md           # This file
```

## Notes

- The script currently filters for `.AppImage` files and excludes filenames containing "arm" to target x86_64 systems.
- It requires `sudo` because it writes to `/usr/local/`.
