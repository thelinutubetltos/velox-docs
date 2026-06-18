# Calamares Installer

Velox uses Calamares 3.4.2 as its graphical installer, built from the AUR and distributed via the `velox-packages` GitHub Releases repo.

## Module sequence

```
partition → mount → unpackfs → shellprocess → machineid → locale →
keyboard → localecfg → fstab → users → shellprocess@post →
displaymanager → networkcfg → hwclock → services-systemd → umount
```

## shellprocess — install.sh

Runs `/usr/lib/velox-installer/install.sh` inside the chroot immediately after files are unpacked.

**What it does:**

1. Installs and configures GRUB with Velox branding
2. Detects NVIDIA GPU via `lspci` — if found, installs `nvidia-open-dkms` + utilities
3. Disables SDDM autologin (removes `User=liveuser` from SDDM config)

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
| `archiso/airootfs/etc/calamares/modules/shellprocess-post.conf` | copy-autostart.sh config |
| `archiso/airootfs/etc/calamares/branding/velox/branding.desc` | Theme colors |
| `archiso/airootfs/etc/calamares/branding/velox/stylesheet.qss` | Dark QSS |
| `archiso/airootfs/usr/lib/velox-installer/install.sh` | Main install script |
| `archiso/airootfs/usr/lib/velox-installer/copy-autostart.sh` | Post-install user setup |
