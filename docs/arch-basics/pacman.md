# pacman Cheat Sheet

`pacman` is the Arch Linux package manager. It's fast, simple, and powerful.

## Essential commands

### Installing packages

```bash
sudo pacman -S firefox              # install a package
sudo pacman -S firefox vlc gimp     # install multiple packages
sudo pacman -S --needed firefox     # skip if already installed
```

### Removing packages

```bash
sudo pacman -R firefox              # remove package only
sudo pacman -Rs firefox             # remove package + unused dependencies
sudo pacman -Rns firefox            # remove + deps + config files
```

### Updating the system

```bash
sudo pacman -Syu                    # sync databases + full upgrade
sudo pacman -Sy                     # sync databases only (don't do this without -u)
```

!!! warning
    Never run `pacman -Sy <package>` (sync without upgrade) — it can create partial upgrades that break your system. Always use `-Syu`.

### Searching

```bash
pacman -Ss firefox                  # search remote repos
pacman -Qs firefox                  # search installed packages
pacman -Si firefox                  # info on repo package
pacman -Qi firefox                  # info on installed package
```

### File queries

```bash
pacman -Ql firefox                  # list files owned by package
pacman -Qo /usr/bin/firefox         # which package owns a file
pacman -F firefox                   # find package containing a file (from file db)
```

### Cleanup

```bash
sudo pacman -Sc                     # remove old package cache
sudo pacman -Scc                    # remove ALL package cache
sudo pacman -Qtdq | sudo pacman -Rs -   # remove orphaned packages
```

### Database maintenance

```bash
sudo pacman-key --refresh-keys              # refresh signing keys
sudo pacman-key --populate archlinux        # repopulate keyring
sudo rm /var/lib/pacman/db.lck              # remove stale lock (if pacman crashed)
```

## pacman.conf

Velox's `/etc/pacman.conf` is pre-configured with:

| Repo | Contents |
|---|---|
| `[core]` `[extra]` `[multilib]` | Standard Arch repos |
| `[chaotic-aur]` | Pre-built AUR packages |
| `[velox_repo]` | Velox kernel, calamares, wallpapers, welcome |
| `[nemesis_repo]` | Erik Dubois tools |

`multilib` is enabled by default for 32-bit compatibility (Steam, Wine).
