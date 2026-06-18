# NVIDIA Drivers

## Automatic detection at install time

The Velox installer (`install.sh`) runs `lspci` during installation. If an NVIDIA GPU is detected, it automatically installs `nvidia-open-dkms` and the associated utilities. NVIDIA drivers are **not** included in the live ISO — they are installed to the target system only.

After installation, verify:

```bash
lspci | grep -i nvidia
nvidia-smi
```

## Manual installation

If you need to install or reinstall NVIDIA drivers after setup:

```bash
sudo pacman -S nvidia-open-dkms nvidia-utils nvidia-settings
```

For older GPUs not supported by `nvidia-open-dkms`:

```bash
sudo pacman -S nvidia-dkms nvidia-utils nvidia-settings
```

## DKMS rebuilds automatically

The `linux-velox` kernel uses DKMS, so NVIDIA modules rebuild automatically when the kernel updates. After any kernel update, reboot to load the new module.

## GRUB entries

The installer adds dedicated NVIDIA GRUB entries only when an NVIDIA GPU is detected. These entries include the correct kernel parameters for NVIDIA + Wayland.

## Wayland vs X11 with NVIDIA

Modern NVIDIA drivers (550+) support KDE Plasma Wayland. If you experience issues, you can switch to an X11 session from the SDDM login screen by selecting **Plasma (X11)**.

## Checking driver status

```bash
# Check loaded kernel module
lsmod | grep nvidia

# Check driver version
cat /proc/driver/nvidia/version

# Monitor GPU usage
nvidia-smi dmon
```
