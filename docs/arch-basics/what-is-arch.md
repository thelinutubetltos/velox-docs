# What is Arch Linux?

Velox Linux is built on top of Arch Linux. Understanding what that means helps you get the most out of your system.

## Rolling release

Arch Linux is a **rolling release** distribution. There are no version numbers like "Ubuntu 24.04" — you install once and stay current forever by running `sudo pacman -Syu`. Every package in the repo is always the latest upstream version.

**Pros:** You always have the newest software, no major version migrations.  
**Cons:** Cutting-edge packages occasionally break things. Take snapshots before major updates.

## Minimalist by design

A base Arch install is just the kernel, core utilities, and a package manager. Velox adds everything on top of that — KDE, the linux-velox kernel, apps, config — but under the hood it's still pure Arch.

## The Arch Wiki

The [Arch Wiki](https://wiki.archlinux.org) is the single best Linux documentation resource in existence. Even if you've never used Arch before, the wiki answers almost every question about Linux system administration, and its solutions usually apply to any distro.

**When something breaks: check the wiki first.**

## pacman

Arch uses `pacman` as its package manager. Velox adds Chaotic-AUR (a binary AUR cache) and `paru` (an AUR helper) so you have access to the full software ecosystem. See the [pacman Cheat Sheet](pacman.md).

## The AUR

The Arch User Repository (AUR) is a community-maintained collection of build scripts for software not in the official repos. `paru` lets you install AUR packages like any other:

```bash
paru -S google-chrome
```

See [The AUR](aur.md) for more.

## systemd

Arch uses systemd for init, service management, logging, networking, and more. See [systemd Essentials](systemd.md).

## No hand-holding, full control

Arch gives you complete control over your system. Velox adds convenience on top — the installer, the welcome app, pre-configured themes — but the underlying system is fully accessible. You can always drop to a terminal and do things the Arch way.
