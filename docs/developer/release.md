# Release Workflow

## Full release checklist

### 1. Make and commit all changes

```bash
cd ~/velox-iso
git add -A
git commit -m "feat/fix: description"
git push
```

### 2. Clean build

```bash
sudo umount -l ~/velox-build/x86_64/airootfs/proc 2>/dev/null
sudo umount -l ~/velox-build/x86_64/airootfs/sys  2>/dev/null
sudo umount -l ~/velox-build/x86_64/airootfs/dev  2>/dev/null
sudo rm -rf ~/velox-build
cd ~/velox-iso/build-scripts && bash build-the-iso.sh
```

### 3. Test in VM

```bash
# Destroy old VM
virsh --connect qemu:///session destroy velox-test 2>/dev/null
virsh --connect qemu:///session undefine velox-test 2>/dev/null
rm -f ~/.local/share/libvirt/images/velox-test.qcow2

# Boot new VM
virt-install \
  --connect qemu:///session \
  --name velox-test --ram 4096 --vcpus 4 \
  --disk path=~/.local/share/libvirt/images/velox-test.qcow2,size=40,bus=sata \
  --cdrom ~/velox-Out/velox-v1.0-x86_64.iso \
  --os-variant archlinux --video virtio --graphics spice \
  --boot cdrom,hd --noautoconsole
```

**Test checklist:**
- [ ] KDE loads, liveuser desktop appears
- [ ] Resolution wizard pops up (~5 seconds after KDE loads)
- [ ] velox-welcome opens
- [ ] Calamares opens after resolution wizard
- [ ] Calamares dark theme looks correct
- [ ] Install completes without errors
- [ ] Reboot into installed system works
- [ ] Resolution wizard fires on first login
- [ ] velox-welcome opens on first login
- [ ] NVIDIA drivers installed (if NVIDIA GPU test)

### 4. Upload to SourceForge

```bash
rsync -avP -e "ssh -i ~/.ssh/id_ed25519" \
  ~/velox-Out/velox-v1.0-x86_64.iso \
  ~/velox-Out/velox-v1.0-x86_64.iso.md5 \
  ~/velox-Out/velox-v1.0-x86_64.iso.sha1 \
  ~/velox-Out/velox-v1.0-x86_64.iso.sha256 \
  ~/velox-Out/velox-v1.0-x86_64.iso.pkglist.txt \
  alextor98405@frs.sourceforge.net:/home/frs/project/velox-linux/7.9/
```

## Commit conventions

```
feat: add <feature>
fix: <what was broken>
chore: version bump / mirrorlist refresh
refactor: <what changed and why>
docs: update docs
```

## Git push policy

Always confirm before pushing to remote. Push only when the build has been tested.

## SourceForge

- **Account:** `alextor98405`
- **SSH key:** `~/.ssh/id_ed25519`
- **Project:** `sourceforge.net/projects/velox-linux`
- **Files path:** `/home/frs/project/velox-linux/7.9/`
