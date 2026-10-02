<div align="center">

# 🎛️ Neon Entities Card

**An entities card for Home Assistant with neon controls for switches, covers, climates, numbers and sensors, and rows that light up when something needs attention.**

[![HACS Custom][hacs-badge]][hacs-url]
[![Release][release-badge]][release-url]
[![Validate][validate-badge]][validate-url]
[![License: MIT][license-badge]][license-url]

[![Open your Home Assistant instance and open this repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=cerealkiller57540&repository=neon-entities-card&category=plugin)

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-entities-card/main/images/main.gif" alt="Neon Entities Card with a gate, a garage door, a shutter, a lamp, an air conditioner, a boiler setpoint and three sensors; the smoke detector battery row pulses red" width="440">

</div>

A drop-in replacement for the built-in entities card. Each row picks its control from the entity's domain: a neon toggle for switches, open / stop / close buttons and a position bar for covers, − / + steppers for climates and numbers, a state badge for binary sensors, a glowing value for sensors. The header uses the same neon title engine as [neon-markdown-card](https://github.com/cerealkiller57540/neon-markdown-card), so both cards sit side by side with matching titles.

*Screenshot taken with made-up entities and the Neo Tokyo theme. State badges are in French for now (see FAQ).*

## ✨ Features

- **One control per domain**: `switch`, `light`, `input_boolean` (toggle), `cover` (buttons and position bar), `climate`, `number`, `input_number` (steppers), `binary_sensor` (state badge by device class: door, window, garage door, moisture…), `sensor` (value with unit).
- **Alerts**: a row turns red and pulses when a smoke, leak, gas, CO, problem, heat or cold sensor is `on`, or when a battery sensor drops below `battery_threshold`. Add your own rule per row with `alert_state`, `alert_below` or `alert_above`, or switch it off with `alert: false`.
- **`status_entity`**: show a second entity on the same row, for example the contact sensor next to the gate switch that opens it.
- **`tap_action: impulse`**: for momentary relays (gates, garage openers), the toggle turns on and back off after `impulse_duration` ms.
- **Neon header and footer**: icon, glow, two-colour gradient, flicker; optional footer line.
- **Entry cascade**: rows slide in one after the other when the card appears.
- Inherits your theme colours by default. Looping decorations are switched off on phones and tablets to save battery.
- **Visual editor**.

## 📦 Installation

### HACS (recommended)

1. Click the **Open in HACS** button above, or add this repository as a custom repository in HACS (category **Dashboard**): `https://github.com/cerealkiller57540/neon-entities-card`.
2. Download **Neon Entities Card**.
3. Reload your browser.

### Manual

1. Copy [`dist/neon-entities-card.js`](dist/neon-entities-card.js) to `config/www/neon-entities-card/`.
2. Add a dashboard resource: URL `/local/neon-entities-card/neon-entities-card.js`, type **JavaScript module**.

## 🚀 Usage

This is the configuration of the screenshot above.

```yaml
type: custom:neon-entities-card
header:
  title: HOME CONTROL
  icon: mdi:home-lightning-bolt
  glow: true
  gradient: true
footer:
  enabled: true
  text: HOME · NEON ENTITIES CARD
entities:
  - entity: switch.gate
    name: Gate
    icon: mdi:gate
    status_entity: binary_sensor.gate_contact
    tap_action: impulse
  - entity: binary_sensor.garage_door
    name: Garage door
  - entity: cover.living_room_shutter
    name: Shutter
    secondary_info: state
  - type: divider
  - entity: switch.desk_lamp
    name: Desk lamp
  - entity: climate.bedroom
    name: Bedroom AC
  - entity: number.boiler_target
    name: Boiler target
  - type: divider
  - entity: sensor.outdoor_temperature
    name: Outdoor
  - entity: sensor.smoke_detector_battery
    name: Smoke detector battery
  - entity: binary_sensor.kitchen_leak
    name: Kitchen leak
```

## ⚙️ Options

**Card**

| Option | Default | Description |
|---|---|---|
| `entities` | **required** | List of rows (see below) |
| `header` / `footer` | — | See below |
| `color_primary` / `color_accent` | theme | Main and accent colours |
| `name_color` / `value_color` / `icon_color` | theme | Text and icon colours |
| `card_bg` / `bg_blur` | theme | Card background and backdrop blur |
| `use_theme_card` | `false` | Let the theme's own card style win |
| `pulse_active` | `true` | Active icons breathe |
| `value_glow` | `true` | Glowing values |
| `flash_on_change` | `false` | Flash a row when its state changes |
| `show_label` | `false` | Show the domain label under each name |
| `ctrl_width` / `ctrl_align` | `12` / `center` | Width and alignment of the control column |
| `alerts` | `true` | Enable alert rows |
| `battery_threshold` | `20` | Battery level (%) that raises an alert |
| `alert_color` / `alert_period` | `#FF2E4A` / `2.8` | Alert colour and pulse period (s) |
| `alert_glow` / `alert_bg` / `alert_name_white` | `1` / `0.08` / `false` | Alert glow strength, background opacity, keep the name white |
| `enter_anim` | `true` | Entry cascade |
| `enter_spread` / `enter_min` / `enter_dur` / `enter_dx` | `360` / `50` / `0.38` / `10` | Total spread (ms), minimum step (ms), row duration (s), slide distance (px) |

**`header:`**

| Option | Default | Description |
|---|---|---|
| `enabled` | `true` | Show the header |
| `title` / `icon` | — | Title text and `mdi:` icon |
| `title_size` / `font` / `font_weight` / `letter_spacing` | theme | Title font |
| `uppercase` / `italic` | — | Text style |
| `color` / `icon_color` / `icon_size` | theme | Colours and icon size |
| `glow` / `glow_color` / `glow_size` | — / primary / `12` | Neon glow |
| `title_shadow` | — | Raw `text-shadow`, replaces `glow` |
| `gradient` / `gradient_from` / `gradient_to` | — | Two-colour gradient text |
| `flicker` | `false` | Neon flicker |

**`footer:`** `enabled`, `text`.

**Per row** (`entities:`)

| Option | Description |
|---|---|
| `entity` / `name` / `icon` | Entity, display name, `mdi:` icon |
| `secondary_info: state` | Show the state under the name |
| `decimal_places` | Rounding for numeric values |
| `status_entity` / `status_decimal_places` | Second entity shown on the same row |
| `tap_action: impulse` / `impulse_duration` | Momentary toggle, back off after N ms (default `500`) |
| `alert` | `false` disables the alert for this row |
| `alert_state` | State (or list of states) that raises an alert |
| `alert_below` / `alert_above` | Numeric thresholds that raise an alert |
| `type: divider` | A separator line instead of an entity |

## ❓ FAQ

**Why is my door sensor not red when it is open?** Only danger device classes (smoke, moisture, gas, carbon monoxide, safety, problem, battery, heat, cold) alert on their own. For anything else, add `alert_state: 'on'` to the row.

**The state badges and the editor are in French.** Translation is on the way. Every option can also be set in YAML.

**Which theme is in the screenshots?** Neo Tokyo, the author's own dark theme (not published). The card works with any theme.

## 🌃 More neon cards

This card is part of a family. See the full collection at [**Home-Assistant-Neon-Cards**](https://github.com/cerealkiller57540/Home-Assistant-Neon-Cards).

---

## 🐾 Support this project

If you enjoy these cards, please consider donating to **Quatre Pattes**, an animal rescue organization.

[![Sauver des animaux](https://img.shields.io/badge/🐾%20Sauver%20des%20animaux-Faire%20un%20don-ff69b4?style=for-the-badge)](https://don.quatre-pattes.org/s/?_jtsuid=70083177244599792679303)

> 💛 No need to support me — just help the animals. Thank you!

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-card`
3. Commit and push
4. Open a Pull Request

---

## 📄 License

[MIT License][license-url]

[hacs-badge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[hacs-url]: https://hacs.xyz
[release-badge]: https://img.shields.io/github/v/release/cerealkiller57540/neon-entities-card?style=for-the-badge
[release-url]: https://github.com/cerealkiller57540/neon-entities-card/releases
[validate-badge]: https://img.shields.io/github/actions/workflow/status/cerealkiller57540/neon-entities-card/validate.yml?branch=main&label=HACS&style=for-the-badge
[validate-url]: https://github.com/cerealkiller57540/neon-entities-card/actions/workflows/validate.yml
[license-badge]: https://img.shields.io/github/license/cerealkiller57540/neon-entities-card?style=for-the-badge
[license-url]: LICENSE
