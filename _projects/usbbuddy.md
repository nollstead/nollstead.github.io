---
layout: project
title: "USB Buddy"
description: "Description of USB Buddy"
featured: true
tags: [Arduino, USB-C]
image: "https://raw.githubusercontent.com/nollstead/USBBuddy/main/images/U2SFront.png"
weight: 10
---

## Product Overview

Looking to breadboard your next MCU-based idea and need a USB or Serial interface to load firmware, voltage regulation and/or logic level conversion but annoyed with the hassle of using multiple components?  Want your prototype to be as close to your production version as possible, without having to add in a lot of connectors to support breadboarding?  Well look no further - this USB-to-Serial Adapter is an easy to use, multi-function, breadboard friendly and feature rich USB-C breakout board, 5v-to-3.3v voltage regulator and Serlal/TTL adapter (with logic level conversion) all in one package.

This adapter fits perfectly on a breadboard and connects to your computer via a USB-C connector - exposing USB data pins, voltages and allowing you to convert USB signals to Serial for programming various MCU's.  You can choose a target voltage (VCC) of either 3.3v or 5v via an included jumper pin - so no soldering required.

For USB to serial conversion, standard, push-pull and open source driven DTR are supported, so this can be used a wide selection of MCUs.  Simply connect a 4.7kΩ resistor across CTS# and DTR# for push-pull driven DTR, a 4.7kΩ resistor from DTR# to GND for MCU's that require open source driven DTR or leave as default for standard DTR.  Alternatively, there are spaces on the [back](/images/U2SBack.png) of the adapter to solder on a resistor should that be preferred (we made that a 1206 to make soldering easier).

## Features & Specs

- Exposes both raw (VBUS) and filtered 5V/3.3v (via an internal LDO voltage regulator)
- Exposes USB D+ and D- differential pairs with onboard ESD protection.
- Supports both push-pull and open source driven DTR modes
- Power, TX and RX LED's
- Automatic level shifting of serial signals to either 5v or 3.3v logic based on jumper selection
- ESD protection on USB connector
- Up to 500mA output current
- 2mm holes for adding to an enclosure
- RoHC Compliant

## USB Breakout

The raw USB VBUS signal and D+/D- differential pairs are exposed.  The adapter contains the necessary 5.1kΩ resistors on the USB CC1 and CC2 pins to allow it to source power, so no need to expose them or add those resistors to your project.

Note that VBUS is connected directly to the VBUS pin on the USB connector, with no filtering or reverse voltage/current protection.  Exercise caution when using this pin and utilize as an output voltage only when a USB cable is connected.

## Target Voltage Selection

While VBUS is exposed for convenience and may be used if desired, the VCC pin is the primary output voltage for target devices.  VCC voltage is controlled by applying a jumper to select either 5v or 3.3v.  So even without using any of the other features it can serve as a standalone USB5v-to-3.3v LDO voltage regulator.  

The VCC and GND pins are exposed on both sides of the adapter for convenience, though they are internally connected - so only one of each is required to power a target.  Additionally, the VCC pin is protected via a reverse current Schottky diode, ensuring that any current on the target device does not propagate to the adapter.

## USB-to-Serial Conversion

USB-to-Serial conversion is accomplished by an onboard [CH340X](/ch340.pdf) and supports both 3.3v and 5v targets.  Select the desired output (VCC) voltage via the jumper pin and the serial signals are automatically level shifted accordingly.  

### DTR# Modes
To support as many target MCUs as possible, the DTR# pin can operate in one of three modes:  Standard, push-pull driven DTR# and open source driven DTR#.  

- Standard:  By default, the DTR# exposes a typical DTR# signal with a weak pullup for MCUs that require it for auto-reset functionality.    
- Push-Pull driven DTR:  Push-pull mode is activated by placing a 4.7KΩ resistor across the DTR# and CTS# (or solding a 1206 SMD 4.7kΩ on the back).  This mode ensures a strong signal transition, which can be useful for reset circuits in some MCUs such as an ATMega328P, driving LEDs or relays or high-speed signal switching where fast transitions are needed without relying on external pull-ups.
- Open Source Driven DTR:  Open source mode is activated by placing a 4.7kΩ pull-down  resistor on the DTR# pin.  This mode is used when interfacing with microcontrollers that require a strong signal to control boot mode selection (such as ESP8266/ESP32), when multiple devices share a communication line (allowing them to pull the line low without intering when idle), or in some low-power designs to minimize power consumption by allowing external components to control the signal state.

## Pinout

| Pin Name          | Pin Type  | Pin Description                |
| :----------- | :----------- | :------------------------- |
| VBUS      | POWER | The raw unfiltered 5v output from the USB connector.   |
| VCC      | POWER | Either 3.3v or 5v, depending on jumper pin selection.  Intended as primary voltage output to match target MCU when using as a USB to Serial converter |
| GND      | POWER | Common ground |
| CTS#    | IN | Clear to Send, active low(high) |
| DTR#    | IN | Data terminal ready that can opeate in one of two modes.  Connect a 4.7kΩ pull-down resistor for open source DTR enhanced mode or a 4.7kΩ resistor between DTR# and CTS# for push-pull DTR enhanced mode.  Note that the resistor can be either external or utilize one of the 1206 pads on the back of the adapter.  |
| RTS#      | OUT | Request to send |
| D-      | USB Signal | Direct USB D- signal with no series resistor |
| D+      | USB Signal | Direct USB D+ signal with no series resistor |
| TXO      | OUT | Serial transmit.  Logic level matches jumper selection and VCC   |
| RXI      | IN | Serial Receive. Logic level matches jumper selection and VCC |


## Documentation & Hookup Guides

- [Loopback Test](/usbbuddy/loopback/) - .c.Here we'll cover the basics of connecting the adapter to your computer and performing a simple loopback test using a terminal emulator.
- [Loopback Test]({{ "/usbbuddy/loopback/" | relative_url }}) - Here we'll cover the basics of connecting the adapter to your computer and performing a simple loopback test using a terminal emulator.
- [Hello World with ATMega328p](/HelloATMega.md) - With the basics under our belt, let's do something more interesting.  Here we'll demonstrate how to load a sketch and send/receive serial text using the Arduino IDE.
