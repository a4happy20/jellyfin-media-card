# 📺 Jellyfin Media Card

A Home Assistant Lovelace card that shows a rotating spotlight of your Jellyfin media —
with tap-to-play. Poster or episode artwork, multiple transition effects, per-library art
overrides, synced rotation across cards, responsive mobile layouts, and detailed or
image-only modes.

![Jellyfin Media Card](https://raw.githubusercontent.com/a4happy20/jellyfin-media-card/main/images/header.png)

## Features

- Rotating spotlight of recently added items from a template sensor
- Tap an item to play it (calls a script with the item's `episode_id`)
- Poster or episode artwork, with per-library overrides
- `slide`, `coverflow`, and `fade` page transitions
- `full` and `half` layouts for the Sections dashboard grid
- Swipe on mobile, horizontal scroll on desktop
- Sync rotation across multiple cards via a shared `sync_group`
- Detailed and image-only display modes

## Before you start

This card doesn't fetch from Jellyfin itself — it renders a **template sensor you provide**.
Set that up first, or the card will have nothing to show:

- **Sensor backend (required):** https://github.com/a4happy20/jellyfin-media-card-sensors
- **Play script (optional, for tap-to-play):** https://github.com/a4happy20/jellyfin-media-card-play

## Quick start

After installing, add the card to a dashboard and point it at your sensor:

```yaml
type: custom:jellyfin-media-card
entity: sensor.jellyfin_recent_card_data
```

A fuller example with common options:

```yaml
type: custom:jellyfin-media-card
entity: sensor.jellyfin_recent_card_data
title: Recently Added
play_script: script.jellyfin_play_episode_custom_card
id_field: episode_id
art_mode: poster
transition: fade
rotate_seconds: 8
layout: full
```

## Common options

| Option | Default | Description |
|--------|---------|-------------|
| `entity` | — | Template sensor holding the media list (required) |
| `attribute` | `episodes` | Attribute on the sensor containing the list |
| `play_script` | `script.jellyfin_play_episode` | Script called on tap |
| `id_field` | `episode_id` | Field passed to the play script as the item ID |
| `title` | `""` | Card header title |
| `rotate_seconds` | `8` | Seconds per item; `0` disables auto-rotation |
| `art_mode` | `poster` | Default artwork: `poster` or `episode` |
| `art_overrides` | `{}` | Per-library art mode, e.g. `{ library2: episode }` |
| `transition` | `slide` | Page effect: `slide`, `coverflow`, or `fade` |
| `sort_mode` | `interleaved` | `interleaved` (newest across libraries) or `grouped` |
| `layout` | `full` | `full` (full width) or `half` (poster tile) |
| `sync_group` | `""` | Cards sharing a value rotate together off one clock |

The full option list (including artwork ratios, colors, backgrounds, and Card Mod styling)
is in the repository README:
https://github.com/a4happy20/jellyfin-media-card#readme

## License

Licensed under the GNU General Public License v3.0. See
https://github.com/a4happy20/jellyfin-media-card/blob/main/LICENSE
