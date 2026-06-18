# systemd Essentials

Velox (like all modern Arch systems) uses systemd for service management, logging, boot, networking, and more.

## Managing services

```bash
# Start / stop / restart
sudo systemctl start   <service>
sudo systemctl stop    <service>
sudo systemctl restart <service>

# Enable / disable at boot
sudo systemctl enable  <service>
sudo systemctl disable <service>

# Enable and start in one command
sudo systemctl enable --now <service>

# Check status
systemctl status <service>

# List all failed units
systemctl --failed
```

## User services

Some services (PipeWire, KDE wallet, etc.) run as your user, not root:

```bash
systemctl --user status pipewire
systemctl --user restart wireplumber
systemctl --user enable --now syncthing
```

## Logs with journalctl

```bash
journalctl -xe                  # recent log with errors highlighted
journalctl -b                   # logs from current boot
journalctl -b -1                # logs from previous boot
journalctl -u <service>         # logs for a specific service
journalctl -f                   # follow log in real time
journalctl --since "1 hour ago" # recent entries only
```

## Boot time analysis

```bash
systemd-analyze                         # total boot time
systemd-analyze blame                   # time per unit
systemd-analyze critical-chain          # critical path
```

## Timers (cron replacement)

systemd timers replace cron jobs:

```bash
systemctl list-timers                   # list all active timers
systemctl enable --now snapper-timeline.timer
```

## Key services in Velox

| Service | Purpose |
|---|---|
| `sddm` | Display manager (login screen) |
| `NetworkManager` | Network management |
| `cpu-performance-governor` | Locks CPU to performance governor |
| `pipewire` | Audio (user service) |
| `bluetooth` | Bluetooth |
| `cups` | Printing |
