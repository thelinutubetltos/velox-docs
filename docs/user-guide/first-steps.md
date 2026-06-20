# First Steps

After Calamares finishes and you reboot into your installed system, here's what happens and what to do first.

## First login

On your first login, two things happen automatically:

1. **Resolution wizard** — a small dialog asks if you want to apply the best detected resolution for your display. Click **Apply** to set it, or **Keep current** to skip. This only appears once.
2. **Velox Control Center** — opens automatically and asks if you'd like to launch it. Click **Open Control Center** to get started, or **Maybe Later** to dismiss.

## Velox Control Center

The Control Center is your one-stop shop for managing your Velox system. It replaces the need for separate package managers, driver installers, and update tools.

![Velox Control Center Home](../assets/Control center1.png)

| Section | What it does |
|---|---|
| **Home** | Browse and install popular apps by category |
| **Installed** | View and remove all installed packages |
| **Updates** | One-click system update — shows the Arch Linux news feed first so you never miss a required manual step |
| **Desktops** | Install additional desktop environments |
| **Kernels** | Switch between kernels (linux-velox, linux, linux-lts, linux-zen, linux-hardened, linux-rt) |
| **GPU Drivers** | Install NVIDIA, AMD, and Intel drivers — auto-detects your GPU |
| **Snapshots** | Manage BTRFS snapshots via Snapper |
| **Fix Keyrings** | One-click repair for broken pacman GPG keys |

## First things to do

### 1. Update your system

Open the Control Center → click **Updates** in the sidebar → click **Update All**.

The updates page shows the latest Arch Linux news feed. If there are any items flagged as **RECENT** (last 14 days), read them before updating — some require a manual step first.

Or from the terminal:

```bash
sudo pacman -Syu
```

### 2. Check your display drivers

- **AMD / Intel** — drivers are included, nothing to do.
- **NVIDIA** — the Calamares installer already detected your GPU and installed `nvidia-open-dkms` during setup. Verify with:

```bash
lspci | grep -i nvidia
nvidia-smi
```

Or use Control Center → **GPU Drivers** to install additional NVIDIA packages.

### 3. Install apps

Use Control Center → **Home** to browse popular apps by category (Browsers, Gaming, Creative, Development, etc.) and install with one click — searches pacman, Chaotic-AUR, AUR, and Flatpak simultaneously.

### 4. Enable AUR helper

`paru` and `yay` are both pre-installed. `paru` is the default:

```bash
paru -S <package-name>
```
