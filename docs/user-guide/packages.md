# Package Management

Velox gives you access to the full Arch package ecosystem: the official repos, Chaotic-AUR, the AUR via `paru`, and Flatpak — all in one place through the Velox Control Center.

## Velox Control Center

The easiest way to install software. Search across all sources simultaneously and install with one click — no terminal needed.

![Velox Control Center Home](../assets/Control center1.png)

- **pacman / Chaotic-AUR** — official Arch repos and pre-built AUR packages
- **AUR** — community packages, scanned by `velox-pkgcheck` for supply-chain attacks before installation
- **Flatpak** — sandboxed apps from Flathub

Open it from the taskbar or run `velox-control-center`.

## Package sources

| Source | What's in it | How to use |
|---|---|---|
| **Official repos** (core, extra, multilib) | Arch Linux packages | `sudo pacman -S <pkg>` or Control Center |
| **Chaotic-AUR** | Pre-built AUR packages | `sudo pacman -S <pkg>` or Control Center |
| **velox_repo** | Velox packages (kernel, control center, wallpapers) | automatic |
| **AUR** | Community source packages | `paru -S <pkg>` or Control Center |
| **Flatpak** | Sandboxed apps | `flatpak install <app>` or Control Center |

## pacman — quick reference

```bash
sudo pacman -Syu              # full system upgrade
sudo pacman -S <package>      # install a package
sudo pacman -Rs <package>     # remove package + unused deps
sudo pacman -Ss <keyword>     # search repos
sudo pacman -Qi <package>     # info on installed package
sudo pacman -Ql <package>     # list files owned by package
```

## paru — AUR helper

```bash
paru -S <package>             # install from AUR (or repos)
paru -Syu                     # upgrade everything including AUR
paru -Ss <keyword>            # search AUR + repos
paru -c                       # clean unneeded dependencies
```

## Flatpak

```bash
flatpak install flathub <app.id>    # install from Flathub
flatpak update                       # update all Flatpaks
flatpak list                         # list installed
flatpak uninstall <app.id>          # remove
```

## AUR Security

Every AUR package installed through the Control Center is scanned by `velox-pkgcheck` before installation. It checks for supply-chain attacks, malicious scripts, and known bad maintainers. You'll see a severity rating and can cancel or proceed.
