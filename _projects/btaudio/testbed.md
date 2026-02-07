---
title: "BTAudio Testbed Setup"
layout: InnerLayout            
permalink: /btaudio/testbed/
---

Below is the current setup for the BTAudio breadboard testbed.  There are a lot of wires and a handful of passives so I recommend doing this in stages and testing each stage before proceeding to the next.

# Component Placement

I recommend spacing the components out as described below to support moving wires around as we test various things.  You can see an example of how mine is setup [here].

- Place the esp32 breadboard adapter and the codec on one row with a few spaces between them.  The breadboard adapter should have the USB port facing left and the codec should have the headphone jack facing right.  Position the codec so that there are two holes available on the bottom pins.

# Wiring

## ESP32 to Codec

First connect the ESP32 and the codec.  Once this is complete you should be able to load firmware and test bluetooth output to the headphone jack on the codec breakout.

### I2S

I2S is used for streaming audio from the esp32 and is only used by the codec, therefore it can be connected directly

- Connect a F/M jumper from GPIO 0 on the ESP32 to MCLK on the codec
- Connect a F/M jumper from GPIO 5 on the ESP32 to BCLK on the codec
- Connect a F/M jumper from GPIO 25 on the ESP32 to LRCLK on the codec
- Connect a F/M jumper from GPIO 26 on the ESP32 to SDOUT on the codec
- Connect a F/M jumper from GPIO 19 on the ESP32 to SDIN on the codec

### I2C

I2C is used by several components so we don't want to connect directly to the codec.  Instead we'll utilize a few of space breadboard spaces between the esp32 and the codec and jumper from there

- Connect a F/M jumper from GPIO 23 on the ESP32 to a middle column, then a M/M jumper from there to SCL on the codec
- Connect a F/M jumper from GPIO 18 on the ESP32 to a middle column, then a M/M jumper from there to SDA on the codec

### Others

- Connect 3V3 on the codec to a shared 3.3v power rail
- Connect GND on the codec to a shared GND power rail
- Connect CE on the codec to a shared GND power rail
- Place a 3.3kΩ resistor from 3V3 on the codec to a shared GND power rail.  Note: This just provides a poweroff discharge path and isn't strictly required, but I recommend adding it so that you mimic my setup.  

## Codec to RCA Breakout
