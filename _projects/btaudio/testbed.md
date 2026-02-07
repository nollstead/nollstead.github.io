---
title: "BTAudio Testbed Setup"
layout: InnerLayout            
permalink: /btaudio/testbed/
---

Below is the current setup for the BTAudio breadboard testbed.  There are a lot of wires and a handful of passives so I recommend doing this in stages and testing each stage before proceeding to the next.

# Component Placement

I recommend spacing the components out as described below to support moving wires around as we test various things.  You can see an example of how mine is setup [here](/assets/btaudio/testbed.jpg).

- Place the esp32 breadboard adapter and the codec on one row with a few spaces between them.  The breadboard adapter should have the USB port facing left and the codec should have the headphone jack facing right.  Position the codec so that there are two holes available on the bottom pins.
- On the second breadboard (below top one) place the two ADS115 breakouts on the left and the BD37033 breakout board on the right.
  - Position ADS115's with pins facing up, toward the ESP32 above it
  - Position the BD37033 with the lable on the left, so MIN -> A1 are facing up towards the codec
- On the third breadboard place the RCA breakout board on the right.  You can add any pots or other things on the left or below that (if you have a 4-breadboard setup)
- We'll get power from the USB and run everything (except the BD37033, more on that later) off 3.3v.  So, connect a F/M jumper from 3V3 and GND on the ESP32 to a a PWR/GND rail then connect all of the PWR/GND rails together.  

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

## ADS1115 (x2)

For our initial testing we'll use 3.3v.  We'll need to figure out how to handle 5v signals but we can do that once everything else is setup and working.

- Connect VDD/GND pins on both boards to shared PWR/GND
- Connect SCL and SDA on both board to the shared I2C columns you made earlier.  Alternatively you can connect one set then connect the second board to those pins on the first, either will work
- On the left ADS1115, connect the ADDR pin to shared GND
- On the right ADS1115, connect the ADDR pin to shared PWR

## RCA Jacks

The current code is set to output the same signal on both ROUT1/LOUT1 and ROUT2/LOUT2.  Currently the headphones are connected to ROUT2/LOUT2 so for this initial RCA test setup (that bypasses the BD37033 for now) we'll use ROUT1 and LOUT1.  Once the steps below are done you should have the same audio out the right RCA jacks as the headphone jacks.  

- Connect GND on the RCA breakout to shared GND (there is no PWR pin)
- Connect LOUT1 on the codec to the RL pin with a 10uF series capacitor and a 10Ω series resistor (in that order)
  - Wiring should be:  LOUT1 -> 10uF -> 10Ω -> RL
  - To do this you'll obviously need some open spaces
  - Positive side of capacitor should face LOUT1, negative faces RL
- Do the same for ROUT1 to RR


## BD37033

Here we'll wire the BD37033 into the left RCA pairs (FR, FL) and sub.  This will allow us to test audio separately.  This one is a work in progress, so more to come.

- Connect GND to shared GND
- Connect VREF to GND through a 10uF capacitor
- Connect SCA and SCL to the shared I2C columns you made earlier
- DO NOT connect VCC to shared PWR.  The BD37033 requires a minimum of 7v so we'll have to use a separate bench power that's connected to shared GND
- Connect a 1uF capacitor from the LOUT pin on the codec to any open breadboard column with the negative side facing the codec (LOUT2).  Then connect ...
- - Connect a 1uF capacitor from the LOUT pin on the codec to any open breadboard column with the negative side facing the codec (LOUT2).  Then connect ... 
