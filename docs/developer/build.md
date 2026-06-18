# Build System

## Prerequisites

On the build machine:

- Arch Linux (or Velox Linux)
- `archiso` package installed
- `chaotic-keyring` and `chaotic-mirrorlist` installed (for Chaotic-AUR in the build)
- SSH key added to SourceForge (`~/.ssh/id_ed25519`) for uploads
- `gh` CLI authenticated for GitHub Releases uploads

```bash
sudo pacman -S archiso
# Chaotic-AUR setup is handled by build-scripts/get-pacman-repos-keys-and-mirrors.sh
```

## Build command

**Always run as a normal user** — the script calls `sudo` internally where needed.

```bash
# 1. Clear stale mounts and old build dir
sudo umount -l ~/velox-build/x86_64/airootfs/proc 2>/dev/null
sudo umount -l ~/velox-build/x86_64/airootfs/sys  2>/dev/null
sudo umount -l ~/velox-build/x86_64/airootfs/dev  2>/dev/null
sudo rm -rf ~/velox-build

# 2. Build
cd ~/velox-iso/build-scripts && bash build-the-iso.sh
```

!!! warning "Always unmount before cleaning"
    Use `-l` (lazy unmount), not `-R`. `-R` silently fails on live kernel virtual filesystems. Skipping the unmount step causes leftover mounts to be included in the squashfs, massively increasing ISO size and build time.

Output lands in `~/velox-Out/`:
- `velox-v1.0-x86_64.iso`
- `velox-v1.0-x86_64.iso.sha256`
- `velox-v1.0-x86_64.iso.sha1`
- `velox-v1.0-x86_64.iso.md5`
- `velox-v1.0-x86_64.iso.pkglist.txt`

## What the build script does

1. Checks for required tools
2. Copies `archiso/` to `~/velox-build/archiso/`
3. Fetches latest `.bashrc` from `erikdubois/edu-shells`
4. Pre-populates the pacman GPG keyring (Arch + Chaotic)
5. Calls `mkarchiso` which: pacstraps the base system, overlays `airootfs/`, squashes to zstd
6. Generates checksums and pkglist

## GRUB boot parameters (live session)

```
cow_spacesize=4G copytoram=n driver=free
module_blacklist=nvidia,nvidia_modeset,nvidia_uvm,nvidia_drm,pcspkr
nouveau.modeset=1 radeon.modeset=1 i915.modeset=1 nvme_load=yes
video=1920x1080
```

NVIDIA is blacklisted in the live session — drivers are installed post-install only.

## File permissions

Files in `archiso/airootfs/` that need specific ownership or permissions must be listed in `archiso/profiledef.sh` under `file_permissions`. Without an entry, executable scripts may not be executable in the live system.

!!! important
    `/usr/local/bin/` does **not** exist in the live squashfs. All custom scripts must go in `/usr/bin/`.

## VM testing

```bash
# Destroy old test VM
virsh --connect qemu:///session destroy velox-test 2>/dev/null
virsh --connect qemu:///session undefine velox-test 2>/dev/null
rm -f ~/.local/share/libvirt/images/velox-test.qcow2

# Create new test VM
virt-install \
  --connect qemu:///session \
  --name velox-test \
  --ram 4096 --vcpus 4 \
  --disk path=~/.local/share/libvirt/images/velox-test.qcow2,size=40,bus=sata \
  --cdrom ~/velox-Out/velox-v1.0-x86_64.iso \
  --os-variant archlinux \
  --video virtio --graphics spice \
  --boot cdrom,hd --noautoconsole
```

Use `sata` disk bus — the linux-velox kernel does not detect virtio block devices.

## Uploading to SourceForge

```bash
rsync -avP -e "ssh -i ~/.ssh/id_ed25519" \
  ~/velox-Out/velox-v1.0-x86_64.iso \
  ~/velox-Out/velox-v1.0-x86_64.iso.md5 \
  ~/velox-Out/velox-v1.0-x86_64.iso.sha1 \
  ~/velox-Out/velox-v1.0-x86_64.iso.sha256 \
  ~/velox-Out/velox-v1.0-x86_64.iso.pkglist.txt \
  alextor98405@frs.sourceforge.net:/home/frs/project/velox-linux/7.9/
```
