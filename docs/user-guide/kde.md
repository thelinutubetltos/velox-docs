# KDE Customization

Velox ships KDE Plasma 6 pre-configured with a dark theme, the Velox green accent palette, Kvantum, and the breeze-velox SDDM login screen.

## Velox color palette

| Role | Color |
|---|---|
| Background | `#0f0f0f` |
| Window | `#141414` |
| Surface | `#1a1a1a` |
| Green primary | `#3a5228` |
| Green highlight | `#5a8160` |
| Accent | `#6dab74` |
| Text | `#c8c8c8` |

## Changing wallpaper

Right-click the desktop → **Configure Desktop and Wallpaper**. Velox ships 7 wallpapers (Velox-1 through Velox-7) pre-installed and visible in the KDE wallpaper picker.

## Kvantum theme engine

Velox uses Kvantum for application styling. To switch or customize:

```bash
kvantummanager
```

## Changing the lock screen wallpaper

The lock screen wallpaper is set via **System Settings → Screen Locking → Appearance**. The default is Velox-4.

## SDDM login screen

The `breeze-velox` SDDM theme is pre-configured. To change it:

```bash
sudo systemsettings6  # or edit /etc/sddm.conf.d/kde_settings.conf
```

## Useful KDE keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Meta` | Open application launcher |
| `Meta + E` | Open file manager |
| `Meta + T` | Open terminal |
| `Meta + Space` | KRunner (search everything) |
| `Alt + F2` | KRunner |
| `Ctrl + F1–F4` | Switch virtual desktops |
| `Meta + Arrow` | Snap window to side |
| `Meta + PgUp/Dn` | Maximize / restore window |

## Installing additional themes

```bash
# From KDE store (in System Settings → Appearance)
# Or via paru for AUR themes:
paru -S kvantum-theme-materia
```
