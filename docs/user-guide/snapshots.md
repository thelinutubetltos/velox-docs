# Snapshots with Snapper

Velox supports Btrfs snapshots via Snapper. When you install on a Btrfs filesystem, Snapper is configured automatically during installation.

!!! warning "Btrfs required"
    Snapshots only work if you installed Velox on a **Btrfs** filesystem. If you chose ext4 during installation, this section does not apply.

## Installing on Btrfs

During installation, select **btrfs** as the filesystem in the Calamares partition step. Velox will automatically:

1. Create subvolumes with the recommended layout:

    | Subvolume    | Mountpoint   |
    |---|---|
    | `@`          | `/`          |
    | `@home`      | `/home`      |
    | `@snapshots` | `/.snapshots`|
    | `@log`       | `/var/log`   |

2. Install `snapper` and configure automatic timeline snapshots
3. Enable `snapper-timeline.timer` and `snapper-cleanup.timer`

## Taking a manual snapshot

```bash
sudo snapper -c root create --description "before update"
```

## Listing snapshots

```bash
sudo snapper -c root list
```

## Rolling back from the terminal

```bash
# Roll back to snapshot number 5
sudo snapper -c root undochange 5..0
```

## Automatic snapshots

Snapper is configured to take hourly snapshots and keep the last 5 hourly + 7 daily. Change limits in `/etc/snapper/configs/root`:

```bash
sudo nano /etc/snapper/configs/root
```

Key settings:

```
TIMELINE_LIMIT_HOURLY="5"
TIMELINE_LIMIT_DAILY="7"
```

## Rolling back from GRUB

To enable snapshot boot entries in GRUB, install `grub-btrfs`:

```bash
sudo pacman -S grub-btrfs inotify-tools
sudo systemctl enable --now grub-btrfs.path
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

Then on reboot, select **Velox Linux Snapshots** in GRUB, choose a snapshot, boot into it, and make it permanent:

```bash
sudo snapper -c root rollback
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
