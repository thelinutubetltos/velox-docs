# First Steps

After Calamares finishes and you reboot into your installed system, here's what happens and what to do first.

## First login

On your first login, two things happen automatically:

1. **Resolution wizard** — a small dialog asks if you want to apply the best detected resolution for your display. Click **Apply** to set it, or **Keep current** to skip. This only appears once.
2. **Velox Welcome** — the main first-run app opens. Use it to install software, set up snapshots, configure gaming tools, and more.

## velox-welcome tabs

| Tab | What it does |
|---|---|
| **First Steps** | Toggle autostart, update system, set appearance, install display drivers, tools |
| **Gaming** | Install Steam, Lutris, Heroic, Bottles, performance tools, Discord |
| **Creative** | Kdenlive, OBS, DaVinci Resolve, GIMP, Inkscape, Krita, Blender |
| **Internet** | Browsers, messaging apps, media players, cloud storage |
| **Kernels** | Switch between standard and performance kernels, update GRUB |
| **Snapshots** | Set up Snapper + grub-btrfs for Btrfs snapshot management |
| **Packages** | Search and install from pacman, Chaotic-AUR, AUR, and Flatpak |
| **About** | Project info and links |

## First things to do

### 1. Update your system

```bash
sudo pacman -Syu
```

Or use the **System Update** button in velox-welcome → First Steps.

### 2. Check your display drivers

- **AMD / Intel** — drivers are included, nothing to do.
- **NVIDIA** — go to velox-welcome → First Steps → Display Drivers and click **Install NVIDIA**. The installer (`install.sh`) already detected your GPU and installed `nvidia-open-dkms` during setup, but verify with:

```bash
lspci | grep -i nvidia
nvidia-smi
```

### 3. Enable AUR helper

`paru` and `yay` are both pre-installed. `paru` is the default:

```bash
paru -S <package-name>
```

### 4. Toggle velox-welcome autostart

If you don't want velox-welcome to open on every login, uncheck **Launch at startup** in velox-welcome → First Steps, or delete the autostart file:

```bash
rm ~/.config/autostart/velox-welcome.desktop
```
