# System Updates

Velox is a rolling release — you get the latest packages continuously, with no version upgrades needed.

## Full system update

```bash
sudo pacman -Syu
```

This updates packages from all configured repos (official, Chaotic-AUR, velox_repo, nemesis_repo) in one command.

## Including AUR packages

```bash
paru -Syu
```

`paru` updates both official and AUR packages.

## Updating Flatpaks

```bash
flatpak update
```

## Kernel updates

The `linux-velox` kernel updates like any other package via `paru -Syu`. After a kernel update, **reboot** to load the new kernel. DKMS modules (like NVIDIA) rebuild automatically during the update.

To check the running kernel:

```bash
uname -r
```

## Update frequency

Arch Linux / Velox packages move fast. It's good practice to update at least once a week. Avoid skipping updates for months — very large gaps can occasionally cause dependency conflicts.

## Before a major update

Take a snapshot first:

```bash
sudo snapper -c root create --description "before update $(date +%Y-%m-%d)"
sudo pacman -Syu
```

## Mirrors

Velox uses the standard Arch mirrorlist. To refresh mirrors for the fastest servers:

```bash
sudo reflector --country "United States" --latest 10 --sort rate --save /etc/pacman.d/mirrorlist
```
