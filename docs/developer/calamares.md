# Calamares Installer

Velox uses Calamares 3.4.2 as its graphical installer, built from the AUR and distributed via the `velox-packages` GitHub Releases repo.

## Module sequence

```
partition → mount → shellprocess@btrfs → unpackfs → shellprocess →
machineid → locale → keyboard → localecfg → fstab → users →
shellprocess@post → displaymanager → networkcfg → hwclock →
services-systemd → umount
```

## shellprocess@btrfs — setup-btrfs.sh

Runs `/usr/lib/velox-installer/setup-btrfs.sh` on the **host** (not in chroot) between `mount` and `unpackfs`. Only acts when the root partition is Btrfs — exits immediately on ext4.

**What it does (Btrfs only):**

1. Saves any child mounts (EFI at `/boot/efi`)
2. Unmounts the raw Btrfs partition from `/tmp/calamares-root`
3. Creates four subvolumes: `@`, `@home`, `@snapshots`, `@log`
4. Sets `@` as the Btrfs default subvolume (kernel mounts it without `rootflags`)
5. Remounts `/tmp/calamares-root` on `@` with `noatime,compress=zstd`
6. Mounts `@home → /home`, `@snapshots → /.snapshots`, `@log → /var/log`
7. Restores child mounts (EFI etc.)

unpackfs then unpacks the squashfs into this subvolume layout, putting `/home` content into `@home`, `/var/log` into `@log`, etc.

## shellprocess — install.sh

Runs `/usr/lib/velox-installer/install.sh` inside the chroot immediately after files are unpacked.

**What it does:**

1. Installs and configures GRUB with Velox branding
2. Detects NVIDIA GPU via `lspci` — if found, installs `nvidia-open-dkms` + utilities
3. Detects Btrfs root — if Btrfs: adds `btrfs` mkinitcpio module, installs `snapper`, writes `/etc/snapper/configs/root`, enables snapshot timers
4. Disables SDDM autologin (removes `User=liveuser` from SDDM config)

**NVIDIA detection:**

```bash
if lspci | grep -i nvidia | grep -iv "audio\|usb\|bridge" | grep -qiE "VGA|3D|Display"; then
    pacman -S --noconfirm nvidia-open-dkms nvidia-utils nvidia-settings
fi
```

**GRUB entries:**

- Standard entry (always added)
- NVIDIA entry (added only if NVIDIA GPU detected)
- Fallback entry

## shellprocess@post — copy-autostart.sh

Runs `/usr/lib/velox-installer/copy-autostart.sh` after the `users` module creates the new user.

**What it does:**

1. Writes `velox-welcome.desktop` to the new user's `~/.config/autostart/`
2. Writes `set-resolution.desktop` to the new user's `~/.config/autostart/`
3. Sets SDDM theme to `breeze-velox`
4. Removes `liveuser` from the installed system (`userdel -r liveuser`)

## Dark theme

Calamares uses a custom dark theme defined in `branding/velox/`:

- `branding.desc` — sidebar colors using PascalCase keys (`SidebarBackground`, `SidebarText`, etc.) required by Calamares 3.4.2
- `stylesheet.qss` — full dark QSS: `#1a1a1a` background, `#e0e0e0` text, `#6dab74` green accents

## Autolaunch in live session

Calamares launches automatically via `velox-live-start`:

1. `calamares.desktop` in liveuser's autostart calls `/usr/bin/velox-live-start`
2. `velox-live-start` runs `/usr/bin/velox-set-resolution` (resolution wizard)
3. After the wizard, `velox-live-start` execs `sudo -n /usr/bin/calamares-launcher`
4. `calamares-launcher` sets required env vars and execs `calamares`

The desktop icon uses `pkexec /usr/bin/calamares-launcher` for manual launch.

## polkit + sudoers

`liveuser` can launch Calamares without a password via:

- `/etc/sudoers.d/liveuser` — NOPASSWD for calamares and calamares-launcher
- `/etc/polkit-1/rules.d/49-calamares.rules` — pkexec returns YES for liveuser

## Key config files

| File | Purpose |
|---|---|
| `archiso/airootfs/etc/calamares/settings.conf` | Module sequence |
| `archiso/airootfs/etc/calamares/modules/shellprocess.conf` | install.sh config |
| `archiso/airootfs/etc/calamares/modules/shellprocess-btrfs.conf` | setup-btrfs.sh config (chroot: false) |
| `archiso/airootfs/etc/calamares/modules/shellprocess-post.conf` | copy-autostart.sh config |
| `archiso/airootfs/etc/calamares/branding/velox/branding.desc` | Theme colors |
| `archiso/airootfs/etc/calamares/branding/velox/stylesheet.qss` | Dark QSS |
| `archiso/airootfs/usr/lib/velox-installer/install.sh` | Main install script |
| `archiso/airootfs/usr/lib/velox-installer/setup-btrfs.sh` | Btrfs subvolume setup (runs on host) |
| `archiso/airootfs/usr/lib/velox-installer/copy-autostart.sh` | Post-install user setup |
