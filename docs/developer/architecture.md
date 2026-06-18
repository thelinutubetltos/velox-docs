# Architecture Overview

Velox Linux is produced by two GitHub repositories and a local package staging area.

## Repositories

| Repo | Purpose | Local path |
|---|---|---|
| `thelinutubetltos/velox-iso` | ISO build config, airootfs overlay, build script | `~/velox-iso` |
| `thelinutubetltos/velox_repo` | Package PKGBUILDs (kernel, calamares, welcome, wallpapers) | `~/velox_repo` |

## Package distribution

| Repo name | Hosting | Contents |
|---|---|---|
| `velox_repo` | GitHub Pages | `velox-welcome`, `velox-wallpapers` (small packages) |
| `velox-packages` | GitHub Releases (`velox-packages` tag) | `linux-velox`, `linux-velox-headers`, `calamares` (large packages) |
| `velox-local` | Local filesystem (`~/velox-local-repo/x86_64/`) | Build-machine-only copy of large packages |

## ISO build pipeline

```
velox-iso/archiso/          ← airootfs overlay + config
        ↓
build-the-iso.sh            ← copies to ~/velox-build, calls mkarchiso
        ↓
mkarchiso                   ← pacstraps base system, overlays airootfs, squashes
        ↓
~/velox-Out/velox-v1.0-x86_64.iso
```

## Live session layout

The live ISO boots as `liveuser` into KDE Plasma (Wayland). The filesystem is:

- **Read-only squashfs** mounted from the ISO
- **Read-write overlay** (tmpfs, 4 GB) for any runtime changes

Key autostart chain:
```
liveuser KDE session
  ├── velox-welcome.desktop  → opens Velox Welcome app
  └── calamares.desktop      → /usr/bin/velox-live-start
                                  ├── /usr/bin/velox-set-resolution (5s, then Qt dialog)
                                  └── sudo -n /usr/bin/calamares-launcher
                                          └── /usr/bin/calamares
```

## Installed system

After Calamares finishes, `install.sh` runs in a chroot:

1. Installs GRUB with Velox branding
2. Detects NVIDIA GPU via `lspci` — installs `nvidia-open-dkms` if found
3. Disables SDDM autologin

Then `copy-autostart.sh` runs (after users are created):

1. Writes `velox-welcome.desktop` to new user's `~/.config/autostart/`
2. Writes `set-resolution.desktop` to new user's `~/.config/autostart/`
3. Sets SDDM theme to `breeze-velox`
4. Removes `liveuser` from the installed system
