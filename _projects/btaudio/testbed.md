---
title: "BTAudio Testbed Setup"
layout: InnerLayout            
permalink: /btaudio/testbed/
---

## Components


## ESP32 to Codec

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


## Codec to RCA Breakout
