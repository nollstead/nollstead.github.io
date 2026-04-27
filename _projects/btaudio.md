---
layout: default
title: "Bluetooth Audio"
description: "Bluetooth to Vehicle Audio Adapter"
featured: true
tags: [ESP32, USB-C, Bluetooth, A2DP]
image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR_rHf5L_cGI94wxfgj2iyts3SH3tJt8wQmwg&s"
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

## Factory Defaults

| Function | Default | Notes |
|---------|---------|-------|
| Input Source | Bluetooth | |
| Input Gain | 0 dB | 0-20 dB |
| Mic Sensitivity | 0 | 0-8 |
| Speaker Volume | 20 | 0-50 scale; 0=mute, 44=0 dB, 50=+6 dB |
| Headphone Volume | 26 | 0-33 scale; 26≈-6 dB, 30=0 dB, 33=+4.5 dB |
| Loudness Compensation | on | |
| Loudness Center Frequency (f0) | 800 Hz | 400, 800, 2400 Hz |
| Loudness Hi-Cut | 1 | 1-4 |
| Bass Gain | 0 dB | -14 to +14 dB |
| Bass f0 | 60 Hz | 60, 80, 100, 120 Hz |
| Bass Q | 0.5 | 0.5, 1.0, 1.5, 2.0 |
| Middle Gain | 0 dB | -14 to +14 dB |
| Middle f0 | 0.5 kHz | 0.5, 1, 1.5, 2.5 kHz |
| Middle Q | 0.75 | 0.75, 1.0, 1.25, 1.5 |
| Treble Gain | 0 dB | -14 to +14 dB |
| Treble f0 | 7.5 kHz | 7.5, 10, 12.5, 15 kHz |
| Treble Q | 0.75 | 0.75, 1.25 |
| Subwoofer Cutoff Frequency | 120 Hz | 55, 85, 120, 160 Hz, or pass-through |
| Subwoofer Phase | 0 degrees | 0, 180 degrees |
| Subwoofer Output | LPF | LPF, front, rear |
| Subwoofer Input | Variable | Variable (tracks volume), Fixed |
| Fader Front Left | +0 dB | |
| Fader Front Right | +0 dB | |
| Fader Rear Left | +0 dB | |
| Fader Rear Right | +0 dB | |
| Fader Sub1 | +0 dB | |
| Fader Sub2 | +0 dB | |

## Console

BTAudio is configured via a USB serial console. Connect a USB-C cable between the device and your computer, then open a serial terminal with these settings:

- **Baud rate:** 115200
- **Data bits:** 8
- **Parity:** None
- **Stop bits:** 1

The prompt `BTAudio>` will appear. Commands are case-insensitive. Type `help` or `help all` for a full command list.

### General

| Command | Description |
|---------|-------------|
| `show version` | Show firmware version |
| `show status` | Show Bluetooth connection status |
| `show settings` | Show all current settings |
| `show vol` | Show volume and loudness settings |
| `show eq` | Show EQ settings |
| `show sub` | Show subwoofer settings |
| `save` | Save current settings to flash |
| `reset` | Reset all settings to factory defaults |
| `reboot` | Reboot the device |
| `help` | Show top-level commands |
| `help all` | Show all commands |

### Speaker Volume

Speaker Volume is a static level set for the amplifier — set it once for your install. Day-to-day volume is controlled from your phone. The scale is 0-50 where level 0 is mute, 44 is 0 dB, and 50 is +6 dB max.

Loudness compensation is enabled by default. Our ears naturally lose sensitivity to bass and treble at lower volumes, which can make music sound thin. Loudness compensation corrects for this by boosting bass and treble at low volumes so music sounds full. The boost gradually reduces as volume increases since it's no longer needed. Disable it for a neutral, uncolored output at all volume levels.

| Command | Description | Example |
|---------|-------------|---------|
| `vol {n}` | Set speaker volume to level n (0-50) | `vol 30` |
| `vol up {n}` | Increase speaker volume by n steps | `vol up 3` |
| `vol down {n}` | Decrease speaker volume by n steps | `vol down 6` |
| `loud on` | Enable loudness compensation (default) | |
| `loud off` | Disable loudness compensation | |
| `loud f0 400` | Set loudness center frequency to 400 Hz | |
| `loud f0 800` | Set loudness center frequency to 800 Hz (default) | |
| `loud f0 2400` | Set loudness center frequency to 2400 Hz | |
| `loud hicut {n}` | Set loudness hi-cut (1, 2, 3, 4) | `loud hicut 2` |
| `hpvol {n}` | Set headphone volume (0-33, 26≈-6 dB default) | `hpvol 30` |

### Input / Mic

| Command | Description | Example |
|---------|-------------|---------|
| `input bt` | Switch audio input to Bluetooth | |
| `input aux` | Switch audio input to AUX jack | |
| `input gain {n}` | Set input gain (0-20 dB) | `input gain 5` |
| `mic {n}` | Set microphone sensitivity (0-8) | `mic 4` |

### EQ / Tone

| Command | Description | Example |
|---------|-------------|---------|
| `bass gain {n}` | Set bass gain (-14 to +14 dB) | `bass gain 6` |
| `bass q {val}` | Set bass Q (0.5, 1.0, 1.5, 2.0) | `bass q 1.0` |
| `bass f0 {val}` | Set bass f0 (60, 80, 100, 120 Hz) | `bass f0 80` |
| `mid gain {n}` | Set middle gain (-14 to +14 dB) | `mid gain -3` |
| `mid q {val}` | Set middle Q (0.75, 1.0, 1.25, 1.5) | `mid q 1.0` |
| `mid f0 {val}` | Set middle f0 (0.5k, 1k, 1.5k, 2.5k Hz) | `mid f0 1k` |
| `treble gain {n}` | Set treble gain (-14 to +14 dB) | `treble gain 4` |
| `treble q {val}` | Set treble Q (0.75, 1.25) | `treble q 1.25` |
| `treble f0 {val}` | Set treble f0 (7.5k, 10k, 12.5k, 15k Hz) | `treble f0 10k` |

### Subwoofer

These commands only affect the sub output (SUB1/SUB2). Front and rear speakers always get full-range audio.

| Command | Description |
|---------|-------------|
| `sub 55` | Set subwoofer cutoff frequency to 55 Hz |
| `sub 85` | Set subwoofer cutoff frequency to 85 Hz |
| `sub 120` | Set subwoofer cutoff frequency to 120 Hz (default) |
| `sub 160` | Set subwoofer cutoff frequency to 160 Hz |
| `sub pass` | Subwoofer plays full range audio |
| `sub phase 0` | Normal subwoofer phase (default) |
| `sub phase 180` | Inverted subwoofer phase |
| `sub out lpf` | Subwoofer gets bass only (default) |
| `sub out front` | Subwoofer plays same audio as front speakers |
| `sub out rear` | Subwoofer plays same audio as rear speakers |
| `sub input variable` | Sub level tracks with volume (default) |
| `sub input fixed` | Sub level stays constant regardless of volume |

### Balance / Fader

Not yet implemented.


## Useful Links

- [BTAudio Firmware Update Page](/btaudio/update)
- [Nextion HMI](/assets/projects/btaudio/BTAudio7.HMI)
- [Github Repository](https://github.com/nollstead/btaudio)
