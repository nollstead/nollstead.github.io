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
| Loudness Center Frequency | 800 Hz | 400, 800, 2400 Hz |
| Loudness Hi-Cut | 1 | 1-4 |
| Bass Gain | 0 dB | -14 to +14 dB |
| Bass Frequency | 60 Hz | 60, 80, 100, 120 Hz |
| Bass Q | 0.5 | 0.5, 1.0, 1.5, 2.0 |
| Middle Gain | 0 dB | -14 to +14 dB |
| Middle Frequency | 0.5 kHz | 0.5, 1, 1.5, 2.5 kHz |
| Middle Q | 0.75 | 0.75, 1.0, 1.25, 1.5 |
| Treble Gain | 0 dB | -14 to +14 dB |
| Treble Frequency | 7.5 kHz | 7.5, 10, 12.5, 15 kHz |
| Treble Q | 0.75 | 0.75, 1.25 |
| Subwoofer Cutoff Frequency | 120 Hz | 55, 85, 120, 160 Hz, or pass-through |
| Subwoofer Phase | 0 degrees | 0, 180 degrees |
| Subwoofer Output | LPF | LPF, front, rear |
| Subwoofer Input | Variable | Variable (tracks volume), Fixed |
| Balance | 0 dB | -15 to +15 dB; positive = right heavier |
| Fader | 0 dB | -15 to +15 dB; positive = front heavier |

## Console

BTAudio is configured via a USB serial console. Connect a USB-C cable between the device and your computer, then open a serial terminal with these settings:

- **Baud rate:** 115200
- **Data bits:** 8
- **Parity:** None
- **Stop bits:** 1

The prompt `BTAudio>` will appear. Commands are case-insensitive. Type `help` or `?` for a full command list.

### General

| Command | Description |
|---------|-------------|
| `show version` | Show firmware version |
| `show status` | Show Bluetooth connection status |
| `show settings` | Show all current settings |
| `show vol` | Show volume and loudness settings |
| `show eq` | Show EQ settings |
| `show sub` | Show subwoofer settings |
| `show spatial` | Show balance and fader settings |
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
| `vol <n>` | Set speaker volume to level n (0-50) | `vol 30` |
| `vol up <n>` | Increase speaker volume by n steps | `vol up 3` |
| `vol down <n>` | Decrease speaker volume by n steps | `vol down 6` |
| `loudness on` | Enable Fletcher-Munson loudness compensation (default) | |
| `loudness off` | Disable Fletcher-Munson loudness compensation | |
| `loudness frequency {400\|800\|2400}` | Set loudness center frequency in Hz | `loudness frequency 400` |
| `loudness hicut {1\|2\|3\|4}` | Set loudness hi-cut level | `loudness hicut 2` |
| `headphone volume <n>` | Set headphone volume (0-33, 26≈-6 dB default) | `headphone volume 30` |

### Input / Mic

| Command | Description | Example |
|---------|-------------|---------|
| `input bt` | Switch to Bluetooth input | |
| `input aux` | Switch to AUX input | |
| `input gain <n>` | Set input gain (0-20 dB) | `input gain 5` |
| `mic sensitivity <n>` | Set microphone sensitivity (0-8) | `mic sensitivity 4` |

### EQ / Tone

| Command | Description | Example |
|---------|-------------|---------|
| `bass gain <n>` | Set bass gain (-14 to +14 dB) | `bass gain 6` |
| `bass q {0.5\|1.0\|1.5\|2.0}` | Set bass Q factor | `bass q 1.0` |
| `bass frequency {60\|80\|100\|120}` | Set bass center frequency in Hz | `bass frequency 80` |
| `middle gain <n>` | Set middle gain (-14 to +14 dB) | `middle gain -3` |
| `middle q {0.75\|1.0\|1.25\|1.5}` | Set middle Q factor | `middle q 1.0` |
| `middle frequency {0.5k\|1k\|1.5k\|2.5k}` | Set middle center frequency | `middle frequency 1k` |
| `treble gain <n>` | Set treble gain (-14 to +14 dB) | `treble gain 4` |
| `treble q {0.75\|1.25}` | Set treble Q factor | `treble q 1.25` |
| `treble frequency {7.5k\|10k\|12.5k\|15k}` | Set treble center frequency | `treble frequency 10k` |

### Subwoofer

These commands only affect the sub output (SUB1/SUB2). Front and rear speakers always get full-range audio.

| Command | Description |
|---------|-------------|
| `sub cutoff {55\|85\|120\|160}` | Set subwoofer LPF cutoff frequency in Hz |
| `sub pass` | Bypass LPF, send full range to sub |
| `sub phase {0\|180}` | Set subwoofer phase (0 or 180 degrees) |
| `sub output {lpf\|front\|rear\|sub}` | Set subwoofer output source |
| `sub input {variable\|fixed}` | Sub level tracks volume or stays constant |

### Balance / Fader

Balance shifts the left/right mix between channels. Fader shifts the front/rear mix. Both are applied as per-channel attenuation on top of the current volume level. Sub channels are not affected.

| Command | Description | Example |
|---------|-------------|---------|
| `balance [n]` | Set left/right balance (-15 to +15 dB; positive = right heavier). No argument shows current value. | `balance -3` |
| `fader [n]` | Set front/rear fader (-15 to +15 dB; positive = front heavier). No argument shows current value. | `fader 5` |


## Useful Links

- [BTAudio Firmware Update Page](/btaudio/update)
- [Nextion HMI](/assets/projects/btaudio/BTAudio7.HMI)
- [Android App v1.0 build 2](https://drive.google.com/file/d/151ij22wGW8b2wEcCeCgi5xwKL8b_3Guo/view?usp=sharing)
- [BLE UUID's](https://github.com/nollstead/btaudio/blob/main/docs/ble-uuids.md)
- [Github Repository - Firmware](https://github.com/nollstead/btaudio)
- [Github Repository - app](https://github.com/nollstead/btaudio_app)

