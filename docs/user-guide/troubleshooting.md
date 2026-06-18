# Troubleshooting

## System won't boot after update

Boot from a Velox live USB and chroot in:

```bash
sudo mount /dev/sdXY /mnt          # replace with your root partition
sudo arch-chroot /mnt
pacman -Syu                         # retry the update
exit
sudo reboot
```

Or if you have Snapper set up, select a snapshot from the GRUB boot menu.

## NVIDIA black screen after boot

Add `nvidia_drm.modeset=1` to your kernel parameters in `/etc/default/grub` (or via GRUB at boot with `e`):

```
GRUB_CMDLINE_LINUX_DEFAULT="... nvidia_drm.modeset=1"
```

Then regenerate GRUB:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## pacman key errors

```bash
sudo pacman-key --refresh-keys
sudo pacman-key --populate archlinux
sudo pacman -Sy archlinux-keyring
sudo pacman -Su
```

## Broken package database

```bash
sudo rm /var/lib/pacman/db.lck     # remove stale lock if pacman crashed
sudo pacman -Syu
```

## KDE Plasma crashes on login

Drop to a TTY (`Ctrl+Alt+F2`) and reset KDE config:

```bash
mv ~/.config/plasma-org.kde.plasma.desktop-appletsrc ~/.config/plasma-org.kde.plasma.desktop-appletsrc.bak
mv ~/.config/plasmashellrc ~/.config/plasmashellrc.bak
```

Then log back in.

## Audio not working

```bash
# Check PipeWire status
systemctl --user status pipewire pipewire-pulse wireplumber

# Restart if needed
systemctl --user restart pipewire pipewire-pulse wireplumber
```

## Failed systemd services

```bash
systemctl --failed           # list failed units
journalctl -xe               # full log with errors
journalctl -u <unit>         # log for a specific unit
```

## Getting help

- **Arch Wiki**: [wiki.archlinux.org](https://wiki.archlinux.org) — the most comprehensive Linux resource online
- **Arch Forums**: [bbs.archlinux.org](https://bbs.archlinux.org)
- **Velox GitHub**: [github.com/thelinutubetltos/velox-iso](https://github.com/thelinutubetltos/velox-iso)
- **The Linux Tube**: [youtube.com/@TheLinuxTube](https://youtube.com/@TheLinuxTube)
