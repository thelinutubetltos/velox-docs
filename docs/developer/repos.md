# Package Repositories

Velox maintains two custom package repositories.

## velox_repo — GitHub Pages (small packages)

**URL:** `https://thelinutubetltos.github.io/velox_repo/$arch`

Contains:
- `velox-wallpapers` — the 7 Velox wallpapers
- `velox-welcome` — the first-run welcome app

To update a package:

1. Update the PKGBUILD in `~/velox_repo/<package>/`
2. Build: `cd ~/velox_repo/<package> && makepkg -si`
3. Add to repo database and push to GitHub Pages

## velox-packages — GitHub Releases (large packages)

**URL:** `https://github.com/thelinutubetltos/velox_repo/releases/download/velox-packages`

Contains:
- `linux-velox` — the custom kernel
- `linux-velox-headers`
- `calamares` — the graphical installer (built from AUR 3.4.2-2)

Local staging: `~/velox-packages-repo/x86_64/`

To update a package:

```bash
# 1. Build the package
cd ~/velox_repo/linux-velox && makepkg -si

# 2. Copy to staging
cp linux-velox-*.pkg.tar.zst ~/velox-packages-repo/x86_64/

# 3. Rebuild database
cd ~/velox-packages-repo/x86_64
repo-add velox-packages.db.tar.gz *.pkg.tar.zst

# 4. Upload to GitHub Releases
gh release upload velox-packages \
  linux-velox-*.pkg.tar.zst \
  velox-packages.db.tar.gz \
  velox-packages.db \
  --clobber --repo thelinutubetltos/velox_repo
```

## velox-local — local repo (build machine only)

**Path:** `~/velox-local-repo/x86_64/`

A local copy of the large packages used during ISO build so mkarchiso doesn't need to download them. Same packages as velox-packages.

## pacman.conf in the ISO

The `archiso/pacman.conf` used during the ISO build includes all four repos:

```ini
[velox_repo]
SigLevel = Never
Server = https://thelinutubetltos.github.io/velox_repo/$arch

[velox-packages]
SigLevel = Never
Server = https://github.com/thelinutubetltos/velox_repo/releases/download/velox-packages

[velox-local]
SigLevel = Never
Server = file:///home/alex/velox-local-repo/$arch

[nemesis_repo]
SigLevel = Never
Server = https://erikdubois.github.io/$repo/$arch

[chaotic-aur]
Include = /etc/pacman.d/chaotic-mirrorlist
```
