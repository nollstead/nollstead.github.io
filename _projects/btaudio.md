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

| Function | Default value |
|---------|-------------|
| Output Volume | 20 (0-50 scale) |
| Loudness Compensation | on |
| Subwoofer Cutoff Frequency | 120Hz |
| Subwoofer Phase	| 0 degrees |
| Subwoofer Output	| LPF |
| Subwoofer Input	| Loudness |
| Fader Front Left	| +0dB |
| Fader Front Right	| +0dB |
| Fader Rear Left	| +0dB |
| Fader Rear Right	| +0dB |
| Fader Sub1	| +0dB |
| Fader Sub2	| +0dB |
| Bass Gain	| +0dB |
| Middle Gain	| +0dB |
| Treble Gain	| +0dB |

## Menu

Connect via Bluetooth SPP (serial) to send commands. Type `help` for a summary or `help all` for the full list.

### General

| Command | Description |
|---------|-------------|
| `help` | Show general help |
| `help all` | Show all commands |
| `help vol` | Show volume commands |
| `help sub` | Show subwoofer commands |
| `help balance` | Show balance commands |
| `help fader` | Show fader commands |
| `version` | Show firmware version |
| `status` | Show current settings |
| `reset` | Reset all settings to factory defaults |

### Volume

Volume is controlled on a 0-50 display scale. Level 0 is mute, 44 is 0 dB, and 50 is +6 dB max. 

Loudness compensation is enabled by default. Our ears naturally lose sensitivity to bass and treble at lower volumes, which can make music sound thin. Loudness compensation corrects for this by boosting bass and treble at low volumes so music sounds full. The boost gradually reduces as volume increases since it's no longer needed. Disable it for a neutral, uncolored output at all volume levels.

| Command | Description | Example |
|---------|-------------|---------|
| `vol up x` | Increase volume by x steps | `vol up 3` |
| `vol down x` | Decrease volume by x steps | `vol down 6` |
| `set vol x` | Set volume to level x (0-50) | `set vol 30` |
| `loud on` | Enable loudness compensation | |
| `loud off` | Disable loudness compensation | |

### Subwoofer
These commands only affect the sub output (SUB1/SUB2). Front and rear speakers always get full-range audio. No subwoofer? Just use `sub off`.

| Command | Description |
|---------|-------------|
| `sub off` | Disable sub output |
| `sub 55` | Set LPF cutoff to 55 Hz |
| `sub 85` | Set LPF cutoff to 85 Hz |
| `sub 120` | Set LPF cutoff to 120 Hz |
| `sub 160` | Set LPF cutoff to 160 Hz |
| `sub pass` | Bypass LPF, send full range to sub |
| `sub phase 0` | Normal phase (0 degrees) |
| `sub phase 180` | Inverted phase (180 degrees) |
| `sub out lpf` | Sub gets bass only (filtered by cutoff above) |
| `sub out front` | Sub mirrors front channels (full range) |
| `sub out rear` | Sub mirrors rear channels (full range) |
| `sub input loudness` | Sub input from loudness block |
| `sub input selector` | Sub input from input selector |

### Balance / Fader

Not yet implemented.


## Useful Links

- [BTAudio Testbed Configuration](/btaudio/testbed)
- [BTAudio Firmware Update Page](/btaudio/update)
- [Github Repository](https://github.com/nollstead/btaudio)

