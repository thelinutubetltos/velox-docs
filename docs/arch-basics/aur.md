# The AUR

The Arch User Repository (AUR) is a community-driven repository of PKGBUILDs — build scripts that compile and package software not in the official Arch repos. It contains tens of thousands of packages.

## How it works

An AUR package is a PKGBUILD script (a bash script) that tells `makepkg` how to download, build, and package software. `paru` automates this process for you.

## paru

Velox ships `paru` as the default AUR helper:

```bash
paru -S google-chrome           # install from AUR (or official repos)
paru -Syu                       # upgrade everything including AUR
paru -Ss <keyword>              # search AUR + official repos
paru -c                         # clean unneeded deps
```

`yay` is also installed if you prefer it.

## Chaotic-AUR

Velox includes Chaotic-AUR — a binary cache of popular AUR packages. These install instantly via `pacman` without building from source:

```bash
sudo pacman -S google-chrome    # comes from Chaotic-AUR, no build needed
```

If a package is in both Chaotic-AUR and the AUR, Chaotic-AUR wins (it's listed first in `pacman.conf`).

## Safety

!!! warning "Read PKGBUILDs before installing"
    Anyone can submit to the AUR. Always check the PKGBUILD before installing an unfamiliar AUR package:
    ```bash
    paru -G <package>      # download PKGBUILD without installing
    cat <package>/PKGBUILD # read it
    ```

    Chaotic-AUR packages are reviewed before inclusion, making them safer than raw AUR builds.

## Manual AUR builds

If you want to build an AUR package manually:

```bash
git clone https://aur.archlinux.org/<package>.git
cd <package>
makepkg -si       # build and install
```
