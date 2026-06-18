# Kernel (linux-velox)

Velox ships a custom kernel built on the CachyOS kernel patchset.

## Overview

| Property | Value |
|---|---|
| Package name | `linux-velox` |
| Current version | `7.0.12-1-velox` |
| Base | CachyOS kernel (performance-optimized patchset) |
| CPU governor | Locked to `performance` at boot via systemd service |
| PKGBUILD | `~/velox_repo/linux-velox/PKGBUILD` |

## Performance governor

A systemd service (`cpu-performance-governor.service`) locks the CPU frequency scaling governor to `performance` on every boot:

```ini
[Service]
Type=oneshot
ExecStart=/bin/sh -c 'echo performance | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor'
```

This is why Velox benchmarks faster than stock Arch on the same hardware.

## Benchmark results (vs Arch kernel 7.0.11-arch1-1)

| Test | Arch | Velox + performance governor | Delta |
|---|---|---|---|
| CPU events/sec | 1119.83 | 1126.48 | +0.6% |
| CPU 95th pct latency | 4.65 ms | 4.49 ms | better |
| Memory throughput | 11,932 MiB/s | 11,987 MiB/s | +0.5% |

## DKMS support

`linux-velox` is built with DKMS support. Out-of-tree kernel modules (NVIDIA, VirtualBox, etc.) rebuild automatically when the kernel updates.

## Building the kernel

```bash
cd ~/velox_repo/linux-velox
makepkg -si
```

Build time: 30–60 minutes depending on hardware.

After building, copy to the local and remote package repos — see [Package Repositories](repos.md).

## GRUB entries

The installer generates separate GRUB entries for the velox kernel:

- `Velox Linux` — standard boot
- `Velox Linux (NVIDIA)` — with NVIDIA-specific kernel params (added only if NVIDIA GPU detected)
- `Velox Linux (fallback)` — fallback initramfs

## Why not the standard Arch kernel?

The linux-velox kernel includes:
- CachyOS scheduler patches (BORE, EEVDF)
- Latency improvements for desktop workloads
- Performance governor locked via systemd (biggest real-world impact)
- Compatible with all standard Arch tools (DKMS, headers, etc.)
