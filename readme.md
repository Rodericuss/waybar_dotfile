```
░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░░▒▓██████▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓███████▓▒░ ░▒▓██████▓▒░░▒▓███████▓▒░  
░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓████████▓▒░░▒▓██████▓▒░░▒▓███████▓▒░░▒▓████████▓▒░▒▓███████▓▒░  
░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░   ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░   ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░ 
 ░▒▓█████████████▓▒░░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░   ░▒▓███████▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░ 
```

Waybar config for Hyprland. Single-bar layout with arrow-decorated segments.

## Bar layout

| Section | Modules |
|---------|---------|
| Left    | Arch logo (drun launcher), workspaces |
| Center  | Date, music player, active window title, clock |
| Right   | Network, keyboard layout, audio, RAM, system tray |

> [!TIP]
> Several extra modules are defined in `modules.jsonc` but not active in the bar — including `custom/brightness` (DDC/CI monitor brightness via scroll), battery, bluetooth, CPU/GPU usage & temperature, notifications, and updates. You can add any of them to `modules-left`, `modules-center`, or `modules-right` in `config.jsonc`.

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/bright.sh` | Adjusts monitor brightness via DDC/CI (up/down/absolute). Requires `ddcutil` DBus service. |
| `scripts/bright-status.sh` | Reads current brightness for display in the bar. Requires `ddcutil`. |
| `scripts/mediaplayer.py` | Outputs current media player info (used by `custom/music`). Requires `playerctl`. |
| `output-switcher.sh` | Cycles through PipeWire audio output devices via `wpctl`. |

## Prerequisites

- `waybar`
- `hyprland`
- Geist Mono Nerd Font (or any Nerd Font)
- `playerctl` — for music module
- `ddcutil` with DBus service enabled — for brightness scripts (optional)

Install on Arch:
```sh
sudo pacman -S waybar playerctl
yay -S geist-mono-nerd ddcutil
```

## Installation

```sh
git clone https://github.com/Rodericuss/waybar.git ~/.config/waybar
```

Make scripts executable:
```sh
chmod +x ~/.config/waybar/scripts/*
chmod +x ~/.config/waybar/output-switcher.sh
```

<details>
<summary>Expected file structure</summary>

```
~/.config/waybar/
├── config.jsonc            # bar layout and settings
├── modules.jsonc           # module definitions
├── style.css               # visual styling (references svg/ files)
├── output-switcher.sh      # audio output cycler
├── scripts/
│   ├── bright.sh           # DDC/CI brightness control (scroll up/down)
│   ├── bright-status.sh    # brightness value reader
│   └── mediaplayer.py      # media player info for custom/music
└── svg/                    # arrow/separator graphics used in style.css
    ├── gr0-left.svg
    ├── gr0-right.svg
    ├── no1-left.svg
    ├── no1-right.svg
    ├── re0-left.svg
    ├── re0-right.svg
    ├── re1-left.svg
    └── re1-right.svg
```
</details>

> [!IMPORTANT]
> Keep the file structure intact — `config.jsonc` uses relative paths to `modules.jsonc` and `svg/` files.

## Multi-monitor setup

Check your monitor names:
```sh
hyprctl monitors
```

Edit `config.jsonc`, duplicate the bar block, and set `"output"` in each:
```jsonc
/* BAR 1 — primary */
{
    "output": "DP-2",
    ...
}
/* BAR 2 — secondary */
,{
    "output": "HDMI-A-1",
    ...
}
```

## Restart waybar

```sh
killall waybar && waybar
```
