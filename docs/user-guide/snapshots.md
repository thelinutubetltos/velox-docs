# Snapshots with Snapper

Velox supports Btrfs snapshots via Snapper + grub-btrfs, set up through velox-welcome. This lets you roll back your system to any previous state directly from the GRUB boot menu.

!!! warning "Btrfs required"
    Snapshots only work if you installed Velox on a **Btrfs** filesystem. If you chose ext4 during installation, this section does not apply.

## Setting up snapshots

In velox-welcome → **Snapshots** tab, click **Set up Snapper**. This:

1. Installs `snapper` and `grub-btrfs`
2. Creates a Snapper configuration for `/`
3. Enables the `grub-btrfs` service so snapshots appear in GRUB

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

## Rolling back from GRUB

Reboot → in the GRUB menu select **Arch Linux Snapshots** → choose the snapshot you want to boot into → once booted, make the snapshot permanent:

```bash
sudo snapper -c root rollback
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Automatic snapshots

Snapper can take automatic timeline snapshots (hourly, daily, weekly). Enable with:

```bash
sudo systemctl enable --now snapper-timeline.timer snapper-cleanup.timer
```
