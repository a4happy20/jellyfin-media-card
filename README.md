# Jellyfin Media Card

A Home Assistant Lovelace card that shows a rotating spotlight of sensor data for
Jellyfin media, with tap-to-play. Supports poster or episode artwork, multiple
transition effects, per-library art overrides, and synchronized rotation across
multiple card instances. Fully integrated with layout tab. Different styles depending
on card size on mobile. Supports two modes, switching between detailed and image only.

![hacs](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)
![license](https://img.shields.io/badge/License-GPLv3-blue.svg)

## Config
<p align="center">
  <img src="images/config.png" width="500" alt="Jellyfin Media Card">
</p>

## Desktop
<table align="center">
  <tr>
    <td valign="top"><img src="images/desktop_03.png" width="300" alt="Jellyfin Media Card"></td>
    <td valign="top"><img src="images/desktop_01.png" width="300" alt="Jellyfin Media Card"></td>
    <td valign="top"><img src="images/desktop_02.png" width="300" alt="Jellyfin Media Card"></td>
  </tr>
</table>

## Mobile
<table align="center">
  <tr>
    <td valign="top"><img src="images/mobile_01.png" width="300" alt="Jellyfin Media Card"></td>
    <td valign="top"><img src="images/mobile_02.png" width="300" alt="Jellyfin Media Card"></td>
    <td valign="top"><img src="images/mobile_03.png" width="300" alt="Jellyfin Media Card"></td>
  </tr>
</table>

## Features

- Rotating spotlight of recently added items from a template sensor
- Tap an item to trigger a play script
- Poster or episode artwork, with per-library overrides
- `slide`, `coverflow`, and `fade` page transitions
- `full` and `half` layouts for the Sections dashboard grid
- Visual editor (UI config) included
- Sync rotation across multiple cards via a shared `sync_group`

## Prerequisites

This card renders a **template sensor** you provide. The sensor's configured
attribute (default `episodes`) must be a list of items shaped like:

```
{ id, series, season, episode, title, overview, library, episode_art, series_art, added }
```

`added` is used for sorting. Setting up that sensor (and the play script) is
outside the scope of this card.

## Installation

### HACS (custom repository)

[![Open in HACS](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=a4happy20&repository=jellyfin-media-card&category=plugin)

1. In HACS, open the three-dot menu → **Custom repositories**.
2. Add this repository's URL and choose category **Dashboard** (plugin).
3. Search for **Jellyfin Media Card** and install it.
4. HACS registers the resource automatically (storage-mode dashboards).

### Manual

1. Download `jellyfin-media-card.js` from the latest
   [release](../../releases/latest).
2. Copy it to `config/www/jellyfin-media-card/jellyfin-media-card.js`.
3. Register the resource (Settings → Dashboards → three-dot menu → Resources):
   - URL: `/local/jellyfin-media-card/jellyfin-media-card.js`
   - Type: **JavaScript Module**

For HACS installs the resource URL is
`/hacsfiles/jellyfin-media-card/jellyfin-media-card.js` with type `module`.

### YAML-mode dashboards

If your dashboard uses `mode: yaml`, HACS can't register the resource
automatically. Add it yourself:

```yaml
resources:
  - url: /hacsfiles/jellyfin-media-card/jellyfin-media-card.js
    type: module
```

## Usage

```yaml
type: custom:jellyfin-media-card
entity: sensor.jellyfin_recent_card_data
attribute: episodes
play_script: script.jellyfin_play_episode_custom_card
id_field: episode_id
title: Recently Added
rotate_seconds: 8
art_mode: poster
transition: slide
sort_mode: interleaved
layout: full
```

## Configuration options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | string | — | `custom:jellyfin-media-card` (required) |
| `entity` | string | — | Template sensor holding the media list (required) |
| `attribute` | string | `episodes` | Attribute on the sensor containing the list |
| `play_script` | string | `script.jellyfin_play_episode_ready` | Script called on tap |
| `id_field` | string | `episode_id` | Field passed to the play script as the item ID |
| `title` | string | `""` | Card header title |
| `api_key` | string | — | Appended to art URLs that need auth |
| `rotate_seconds` | number | `8` | Seconds per item; `0` disables auto-rotation |
| `height` | number | `375` | Card height seed (px); real size set on the Layout tab |
| `art_mode` | string | `poster` | Default artwork: `poster` or `episode` |
| `art_overrides` | object | `{}` | Per-library art mode, e.g. `{ youtube: episode }` |
| `sort_mode` | string | `interleaved` | `interleaved` (newest across libraries) or `grouped` (by library) |
| `transition` | string | `slide` | Page effect: `slide`, `coverflow`, or `fade` |
| `poster_ratio` | string | `183/274` | Frame ratio when showing poster art |
| `episode_ratio` | string | `16/9` | Frame ratio when showing episode art |
| `layout` | string | `full` | `full` (full width) or `half` (poster tile, 6/12 grid columns) |
| `sync_group` | string | `""` | Cards sharing a value rotate together off one clock |
| `font_scale` | number | `1.0` | Scales card text (0.5–2.0) |

## License

Licensed under the [GNU General Public License v3.0](LICENSE). You're free to use,
modify, and distribute this software, including commercially, provided that
derivative works are also released under the GPLv3 and their source is made
available.
