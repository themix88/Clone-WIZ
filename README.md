# Rclone-WIZ
### A Graphical Rclone Mount Helper for Linux

[![Version](https://img.shields.io/badge/Version-1.7.4-blue.svg)](https://github.com/themix88/Clone-WIZ/Rclone-WIZ)
[![License](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

Rclone-WIZ is a user-friendly graphical utility designed to simplify the process of configuring, scripting, and mounting cloud storage remotes using `rclone` on Linux systems. It aims to provide an intuitive interface for managing your cloud drives, making `rclone` more accessible to a wider audience.

## Disclaimer
Rclone-WIZ is an independent project and is not affiliated with, endorsed by, or sponsored by the official rclone team or its contributors.

## Features
Rclone-WIZ provides the following key features to streamline your cloud storage management:

*   **Simple Interface:** An intuitive graphical user interface for easy navigation.
*   **Integrated Rclone Config:** Launch `rclone config` directly from the app for seamless remote setup.
*   **Google Drive Tutorial:** A step-by-step guide for authenticating Google Drive as an example.
*   **Automated Script Generation:** Automatically generates and manages bash mount scripts for persistent usage.
*   **Autostart Configuration:** Easily add your mounted drives to system autostart with a single button click.

## Prerequisites
Before running Rclone-WIZ, ensure you have the following installed on your system:

*   `fuse3`: Filesystem in Userspace (FUSE) library, required for mounting.
*   `python-pyqt6`: Python bindings for the Qt 6 application framework.

> **Note:** `rclone` is **bundled and built from source** as part of the package — you do not need to install it separately.

## Installation

### Quick Start (run directly from source)
To run Rclone-WIZ directly without installing:

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/themix88/Clone-WIZ.git
    cd Clone-WIZ
    ```
2.  **Install `rclone` separately** (needed only for this method):
    ```bash
    sudo pacman -S rclone
    ```
3.  **Run the application:**
    ```bash
    python3 rclone-wiz.py
    ```

### Build and Install from Source (Arch Linux)
This method fetches and compiles everything from source — no AUR required.

**Build-time dependencies:** `go`, `git`, `base-devel`

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/themix88/Clone-WIZ.git
    cd Clone-WIZ
    ```
2.  **Build and install:**
    ```bash
    makepkg -si
    ```
    `makepkg` will automatically fetch `rclone` from its GitHub repository, compile it with Go, and bundle it into the package alongside Rclone-WIZ.

## Usage
Using Rclone-WIZ is straightforward:

1.  **Configure Rclone Remotes:** Use the "⚙️ Open Rclone Config" button to set up your cloud drives via the `rclone` terminal interface.
2.  **Select Remote:** Choose your desired cloud drive from the "Select Cloud Drive (Remote)" dropdown.
3.  **Choose Mount Folder:** Specify a local directory where the cloud drive will be mounted.
4.  **Review Script:** The generated bash script will be displayed for your review.
5.  **Save & Run:** Click "▶ Save & Run Script" to mount your drive.
6.  **Autostart (Optional):** Use "🚀 Add to Autostart" to automatically mount the drive on system login.

## Screenshots
*(Screenshots coming soon!)*

## License
This project is licensed under the GNU General Public License v3.0 (GPLv3).

---

## Troubleshooting: File Manager Thumbnails & Slow Mounts

> **As of v1.7.4-14, the generated mount script now includes `--no-modtime` and `--transfers 4` flags which significantly reduce the impact of this issue.** If you're still experiencing slowdowns, follow the additional steps below.

When a cloud drive is mounted via FUSE, aggressive file managers may treat it like a local disk and try to generate thumbnails for every media file simultaneously. This triggers a flood of per-file metadata and read requests, which can exhaust your RAM or cause temporary API rate-limit bans from your cloud provider.

The mount script mitigates this with:
- **`--no-modtime`** — skips fetching modification times per file, eliminating the most expensive per-file API call that thumbnail scanners trigger.
- **`--transfers 4`** — caps the number of concurrent download threads so thumbnail generators can't saturate the connection.

For an extra layer of protection, you can also restrict thumbnail generation in your file manager:

### Per Desktop Environment

### 1. Standalone Window Managers (Hyprland, Sway, i3)

First check if you're using "tumbler"

**Option A - Limit Thumbnail Size (Recommended)**: 
Instead of killing previews entirely, I edit the tumbler config (usually located at ~/.config/tumbler/tumbler.rc) so it stops choking on massive network files. Find the [VideoThumbnailer] or [FfmpegThumbnailer] section and change the unlimited setting (MaxFileSize=0) to a strict limit, like 2MB:

```bash
MaxFileSize=2048000
```

OR


**Option B: Remove Tumbler**
If you don't care about thumbnails anywhere on your system, you can just purge the service entirely:

```bash
sudo pacman -R tumbler
```

---

**Nautilus**
-
Nautilus is usually decent at respecting network boundaries, but FUSE mounts can sometimes trick it.

- Open Nautilus and navigate to Preferences -> Search & Preview.

- Under "Thumbnails," set it to Local Files Only.

- FUSE Override: If Nautilus still thumbnails the rclone mount, change the "Only for files smaller than" setting to 1 MB.

---

**Dolphin**
-
Dolphin uses the kio-fuse wrapper, which almost always treats an rclone mount as a standard local disk.

- Open Dolphin -> Configure Dolphin -> General -> Previews tab.

- Lower the "Skip previews for local files above:" setting to 1 MB or 2 MB.

---

**Thunar**
-


- Go to Edit -> Preferences -> Display tab.

- Under the "Default View" section, set Show thumbnails to Local Files Only or Never.


---

# !! RPM & DEB packages are not updated yet !!
