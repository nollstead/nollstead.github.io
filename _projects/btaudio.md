---
layout: default
title: "Bluetooth Audio"
description: "Bluetooth to Vehicle Audio Adapter"
featured: true
tags: [ESP32, USB-C, Bluetooth, A2DP]
image: "/assets/projects/btaudio/btaudio.png"
weight: 10
breadcrumb:
  - { title: "Home", url: "/" }
  - { title: "Projects", url: "/projects/" }
  - { title: "Bluetooth Audio" }
---

# Bluetooth Audio

## Project Overview

BTAudio adds Bluetooth audio streaming to classic cars and older vehicles that do not have a modern Bluetooth-enabled radio. It wires into your existing amplifier and speakers, letting you pair your phone and stream music with built-in sound processing including volume control, tone adjustment, loudness compensation, and subwoofer management.

## Features

- Bluetooth (A2DP) music streaming, plus optional hands-free phone calls
- Volume, loudness compensation, 3-band EQ, balance and fader
- Subwoofer output with adjustable crossover, phase and level
- AUX input with adjustable input gain
- Settings via the USB serial console, the Android app (Bluetooth LE) or a Nextion display
- Optional sensor board expansion

## Quick Start

1. Power the unit and pair **BTAudio** from your phone's Bluetooth settings.
2. Set Speaker Volume once for your amplifier (for example `vol 30`), then `save`.
3. Use your phone's volume for day-to-day listening.

Everything else is optional — see the [Settings Reference](/btaudio/settings/).

## Documentation

| Page | What's in it |
|------|--------------|
| [Settings Reference](/btaudio/settings/) | Every console command, range and factory default |
| [Firmware Update](/btaudio/update/) | Flash the latest firmware from your browser |

## Downloads

- [BTAudio Firmware Update Page](/btaudio/update)
- [Android App v0.9 Build 14](https://drive.google.com/file/d/176ZrEpMOBysuRFBq1B8vk31tpK_s8N80/view?usp=sharing)
- [Nextion HMI](/assets/projects/btaudio/BTAudio7.HMI)
- [BTAudio case (Bambu Studio File)](/assets/projects/btaudio/BTAudio2.3mf)

## Developer Resources

- [BLE UUID's](https://github.com/nollstead/btaudio/blob/main/docs/ble-uuids.md)
- [Github Repository - Firmware](https://github.com/nollstead/btaudio)
- [Github Repository - app](https://github.com/nollstead/btaudio_app)

## Legal

- [Privacy Policy](https://nollsteadstudio.com/btaudio/privacy/)
