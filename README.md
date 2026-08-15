# Smart Alarm Clock — Home Assistant integration

Custom integration for the [smart-alarm-clock](https://github.com/MartBent/smart-alarm-clock)
ESP32-S3 bedside clock. It talks to the device's local REST API and gets instant
updates over Server-Sent Events — **no MQTT broker required**.

## Install (HACS)

1. HACS → **⋮** (top right) → **Custom repositories**.
2. Repository: `https://github.com/MartBent/homeassistant-smart-alarm-clock`,
   Type: **Integration** → **Add**.
3. Find **Smart Alarm Clock** in HACS → **Download**.
4. **Restart Home Assistant.**

## Add the device

- **Auto-discovery**: once the device is on your network it advertises over mDNS,
  so HA shows **Settings → Devices & Services → "Smart Alarm Clock discovered"** →
  **Configure**.
- **Manual**: Settings → Devices & Services → **Add Integration** → *Smart Alarm
  Clock* → enter the device's IP.

## Entities

One device, **Smart Alarm Clock**, with:

| Entity | Type | What it does |
| --- | --- | --- |
| Phase | sensor | `syncing` / `idle` / `armed` / `ringing` / `snoozed` |
| Time | sensor | the device's own wall clock |
| Snooze, Dismiss | button | while ringing (Dismiss disables the fired alarm) |
| Display | switch | all light-emitting components on/off (a firing alarm lights up anyway) |
| Alarm *N* | switch | enable/disable each slot — enabling arms the clock |
| Alarm *N* time | time | edit each slot's time |

There's no explicit Armed switch: the clock is armed whenever any alarm is
enabled, and dismissing a ringing alarm turns that slot off (one-shot).

## How it works

- **Commands** → `POST /api/command`, `/api/alarm/enabled`, `/api/alarm/time`.
- **State** → `GET /api/state` (60 s polling fallback).
- **Realtime** → the device streams changes over SSE on `:81/api/events`, so
  entities update instantly (`iot_class: local_push`).

Give the device a DHCP reservation, or rely on mDNS, so its address is stable.

## Firmware & hardware

The device firmware, hardware, and design docs live in the main repo:
[MartBent/smart-alarm-clock](https://github.com/MartBent/smart-alarm-clock).
