# File System Layout

Arch Linux follows the [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/fhs.shtml). Here's what matters day-to-day.

## Key directories

| Path | Contents |
|---|---|
| `/` | Root of the filesystem |
| `/boot` | Kernel, initramfs, GRUB files |
| `/etc` | System-wide configuration files |
| `/home/<user>` | Your personal files and config |
| `/usr` | Installed programs and libraries |
| `/usr/bin` | User executables |
| `/usr/lib` | Libraries |
| `/var` | Variable data (logs, cache, databases) |
| `/var/log` | Log files |
| `/tmp` | Temporary files (cleared on reboot) |
| `/mnt` | Temporary mount point |
| `/run` | Runtime data (PIDs, sockets) |

## Important config files

| File | Purpose |
|---|---|
| `/etc/pacman.conf` | pacman repos and settings |
| `/etc/pacman.d/mirrorlist` | Arch mirror list |
| `/etc/fstab` | Filesystem mounts |
| `/etc/hostname` | System hostname |
| `/etc/hosts` | Local DNS overrides |
| `/etc/locale.conf` | System locale |
| `/etc/vconsole.conf` | Console keyboard layout |
| `/etc/default/grub` | GRUB boot parameters |
| `/etc/sddm.conf.d/` | SDDM login screen config |
| `/etc/sudoers.d/` | sudo permissions |

## User config (~/)

KDE and most apps store config in `~/.config/`:

| Path | Contents |
|---|---|
| `~/.config/` | Application configs |
| `~/.config/autostart/` | Apps that launch at login |
| `~/.local/share/` | User data (themes, icons, etc.) |
| `~/.bashrc` | Bash shell config |

## Velox-specific paths

| Path | Purpose |
|---|---|
| `/usr/bin/velox-welcome` | Welcome app |
| `/usr/bin/velox-set-resolution` | Resolution wizard |
| `/usr/bin/velox-live-start` | Live session launcher |
| `/usr/bin/calamares-launcher` | Calamares wrapper |
| `/usr/lib/velox-installer/` | Install + post-install scripts |
| `/usr/share/sddm/themes/breeze-velox/` | SDDM login theme |
| `/etc/skel/` | Template files copied to new user homes |
