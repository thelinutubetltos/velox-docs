# Display & Resolution

## First-run resolution wizard

On first login (and in the live session before installing), Velox runs a resolution wizard that detects the best available resolution for your display and offers to apply it. It runs once — after you respond, it marks itself done and won't appear again.

If you need to re-run it:

```bash
rm ~/.config/velox-resolution-configured
/usr/bin/velox-set-resolution
```

## Changing resolution manually

### KDE Display Settings (recommended)

Open **System Settings → Display and Monitor** or run:

```bash
systemsettings6 --module kcm_kscreen
```

### kscreen-doctor (command line)

```bash
# List outputs and modes
kscreen-doctor -o

# Apply a specific mode (e.g. output Virtual-1, mode index 1)
kscreen-doctor output.Virtual-1.mode.1
```

### xrandr (X11 sessions)

```bash
xrandr --query                              # list outputs and modes
xrandr --output HDMI-1 --mode 1920x1080    # apply resolution
```

## Multiple monitors

Use **System Settings → Display and Monitor** to arrange, enable, or disable monitors. KDE Plasma remembers your configuration per display combination.

## HiDPI / scaling

In **System Settings → Display and Monitor**, set the **Global Scale** (e.g. 150% for 4K displays). For per-app scaling on X11:

```bash
export QT_SCALE_FACTOR=1.5
```
