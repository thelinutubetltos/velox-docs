# Package Management

Velox gives you access to the full Arch package ecosystem: the official repos, Chaotic-AUR, the AUR via `paru`, and Flatpak.

## Package sources

| Source | What's in it | How to use |
|---|---|---|
| **Official repos** (core, extra, multilib) | Arch Linux packages | `sudo pacman -S <pkg>` |
| **Chaotic-AUR** | Pre-built AUR packages | `sudo pacman -S <pkg>` |
| **velox_repo** | Velox packages (kernel, calamares, wallpapers, welcome) | automatic |
| **nemesis_repo** | Erik Dubois packages (flameshot-git, pamac-aur, etc.) | `sudo pacman -S <pkg>` |
| **AUR** | Community source packages | `paru -S <pkg>` |
| **Flatpak** | Sandboxed apps | `flatpak install <app>` |

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

## velox-welcome Package Search

The **Packages** tab in velox-welcome lets you search across pacman, Chaotic-AUR, AUR, and Flatpak simultaneously and install with one click — no terminal needed.
