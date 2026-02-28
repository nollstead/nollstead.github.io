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
| Loudness Center Frequency (f0) | 800 Hz |
| Loudness Hi-Cut | 1 |
| Input Gain | 0 dB |
| Bass Gain | 0 dB |
| Bass f0 | 60 Hz |
| Bass Q | 0.5 |
| Middle Gain | 0 dB |
| Middle f0 | 0.5 kHz |
| Middle Q | 0.75 |
| Treble Gain | 0 dB |
| Treble f0 | 7.5 kHz |
| Treble Q | 0.75 |
| Subwoofer Cutoff Frequency | 120 Hz |
| Subwoofer Phase | 0 degrees |
| Subwoofer Output | LPF |
| Subwoofer Input | Loudness |
| Fader Front Left | +0 dB |
| Fader Front Right | +0 dB |
| Fader Rear Left | +0 dB |
| Fader Rear Right | +0 dB |
| Fader Sub1 | +0 dB |
| Fader Sub2 | +0 dB |

## Menu

Connect via Bluetooth SPP (serial) to send commands. Type `help` for a summary or `help all` for the full list.

### General

| Command | Description |
|---------|-------------|
| `help` | Show general help |
| `help all` | Show all commands |
| `help vol` | Show volume commands |
| `help eq` | Show EQ/tone commands |
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
| `vol x` | Set volume to level x (0-50) | `vol 30` |
| `loud on` | Enable loudness compensation (default) | |
| `loud off` | Disable loudness compensation | |
| `loud f0 400` | Set loudness center frequency to 400 Hz | |
| `loud f0 800` | Set loudness center frequency to 800 Hz (default) | |
| `loud f0 2400` | Set loudness center frequency to 2400 Hz | |
| `loud hicut {n}` | Set loudness hi-cut (1, 2, 3, 4) | `loud hicut 2` |
| `input gain {n}` | Set input gain (0-20 dB) | `input gain 5` |

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
These commands only affect the sub output (SUB1/SUB2). Front and rear speakers always get full-range audio. No subwoofer? Just use `sub off`.

| Command | Description |
|---------|-------------|
| `sub off` | Disable subwoofer output |
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
| `sub input loudness` | Subwoofer signal includes loudness compensation (default) |
| `sub input selector` | Subwoofer signal bypasses loudness compensation |

### Balance / Fader

Not yet implemented.


## Useful Links

- [BTAudio Testbed Configuration](/btaudio/testbed)
- [BTAudio Firmware Update Page](/btaudio/update)
- [Github Repository](https://github.com/nollstead/btaudio)
