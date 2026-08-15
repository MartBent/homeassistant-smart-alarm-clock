# Smart Alarm Clock for Home Assistant

Home Assistant integration for a DIY ESP32-S3 bedside alarm clock. It talks to the
clock over its local HTTP API and picks up changes the moment they happen using
Server-Sent Events, so there's no polling lag and nothing to run in between (no MQTT
broker). The device firmware and hardware live in the main repo,
[MartBent/smart-alarm-clock][device].

[![GitHub Release][release-shield]][release]
[![HACS Custom][hacs-shield]][hacs]
[![Validate][validate-shield]][validate]
[![License][license-shield]](LICENSE)

## Requirements

- Home Assistant 2024.x or newer.
- The clock reachable on your network. Give it a DHCP reservation, or rely on mDNS,
  so its address stays put.

## Installation

This is a HACS custom repository (it isn't in the default HACS store). Click the
button to add it, or add it by hand:

[![Open your Home Assistant instance and open this repository inside HACS.][hacs-repo-shield]][hacs-repo]

By hand: HACS, then the three-dot menu, then Custom repositories. Paste
`https://github.com/MartBent/homeassistant-smart-alarm-clock`, choose the
Integration category, and download it from the HACS list. Restart Home Assistant
when it's done.

Without HACS: copy `custom_components/smart_alarm_clock/` into your
`config/custom_components/` folder and restart.

## Adding the clock

Once the clock is on the network it advertises itself over mDNS, so Home Assistant
usually offers it under Settings → Devices & Services. If it doesn't turn up, add it
by hand with Add Integration → Smart Alarm Clock and type in its IP address.

[![Add the Smart Alarm Clock integration to your Home Assistant instance.][config-shield]][config]

## Entities

The clock shows up as a single device with these entities:

- **Phase** (sensor) — `syncing`, `idle`, `armed`, `ringing`, or `snoozed`.
- **Time** (sensor) — the clock's own wall time.
- **Display** (switch) — turns the matrix and status LED off so the clock goes dark.
  A ringing alarm lights up regardless.
- **Alarm 1–8** (switch) — enable a slot. The clock is "armed" whenever at least one
  slot is on.
- **Alarm 1–8 time** (time) — set each slot's alarm time.
- **Snooze** / **Dismiss** (button) — only do anything while an alarm is ringing.
  Dismiss also switches off the slot that fired, so alarms are one-shot.

There's deliberately no separate "armed" control: enabling any alarm arms the clock,
and dismissing a ringing one turns that slot back off.

## License

MIT. See [LICENSE](LICENSE).

[device]: https://github.com/MartBent/smart-alarm-clock
[release-shield]: https://img.shields.io/github/release/MartBent/homeassistant-smart-alarm-clock.svg
[release]: https://github.com/MartBent/homeassistant-smart-alarm-clock/releases
[hacs-shield]: https://img.shields.io/badge/HACS-Custom-41BDF5.svg
[hacs]: https://hacs.xyz
[validate-shield]: https://github.com/MartBent/homeassistant-smart-alarm-clock/actions/workflows/validate.yml/badge.svg
[validate]: https://github.com/MartBent/homeassistant-smart-alarm-clock/actions/workflows/validate.yml
[license-shield]: https://img.shields.io/github/license/MartBent/homeassistant-smart-alarm-clock.svg
[hacs-repo-shield]: https://my.home-assistant.io/badges/hacs_repository.svg
[hacs-repo]: https://my.home-assistant.io/redirect/hacs_repository/?owner=MartBent&repository=homeassistant-smart-alarm-clock&category=integration
[config-shield]: https://my.home-assistant.io/badges/config_flow_start.svg
[config]: https://my.home-assistant.io/redirect/config_flow_start/?domain=smart_alarm_clock
