---
layout: project
title: "ICSP"
description: "3.3v/5v In-Circuit Serial Programmer"
tags: [Arduino]
featured: false
image: "https://raw.githubusercontent.com/nollstead/ICSP/refs/heads/main/images/ICSP.png"
weight: 11
---
## Product Overview

Looking to easily either load a bootloader or directly program an Arduino Uno but don't like messing with wires?  How about other microcontrollers, such as an ATTiny45 or ATTiny88 that use 3.3v logic?  This handly ICSP programmer allow you to easily program either commercial or custom ISP-capable boards with ease.  

It's powered by an ATMega328P microcontroller and comes preloaded with the ArduinoISP sketch, so out of the box it'll allow you to program any AVR-based target.  Need to program something else - just load your own custom sketch just like any Arduino Uno.

The onboard CH340C automatically converts USB signals to serial and is recognized by Windows, so no drivers or separate FTDI board required.  Check out our [Hookup Guides](#documentation--hookup-guides) to see how easy it is to program your 5v or 3.3v targets.

## Features & Specs

- Program any ISP-capable microcontroller.  Comes preloaded with the ArduinoISP sketch, so it can program AVR-based boards out of the box, but load your own sketch to program any other ISP-capable microcontroller.
- Adjustable output voltage of 5.5v or 3.3v via jumper.  Automatically level shifts data pins based on selected target voltage
- Preloaded with ArduinoISP sketch, no special libraries needed
- Arduino-IDE compatible.  No need for complex AVRDude commands
- USB-C connector
- Transmit (Tx) and Receive (Rx) LEDs
- TVS diodes for ESD protection on USB interface
- 500mA fuse on USB to protect against overcurrent draw from target
- Keyed ICSP connector eliminates accidental mis-wiring or clumbsy jumper wires.
- Includes 6-pin double female cable for connecting to standard Arduino ICSP header.  Compatible with Tag-Connect cables for lower profile custom boards.

## Documentation & Hookup Guides

Looking to program your AVR-based boards without the mess of wires and a breadboard?  In this guide we'll introduct you to the important aspects of the ICSP and how to program different types of boards with it.

- [Programming an Arduino UNO](/Programming-an-Arduino-Uno.md) - A simple example of reloading a bootloader onto an Arduino UNO.
- [Programming a Custom ATMega328P using Tag-Connect](/Programming-a-Custom-ATMega328P.md) - A more interesting example of how to load a bootloader onto a custom ATMega328P based board.  In this example we'll "program the programmer"
- [Programming an ATTiny88-based Custom Board](/Programming-a-Custom-ATTiny88.md) - An even more interesting example of programming something other than an Arduino Uno clone - in this case a custom board based on an Atmega ATTiny88.


### Required Materials

To follow along with this tutorial, you will need an ICSP programmer and a target board to program.  Our tutorials include programming different types of board but we suggest starting with a standard Arduino Uno.  Additionally, you'll need a USB-C cable to plug into your computer and the Arduino IDE installed.

### Drivers

The programmer uses a CH340C USB-to-Serial converter.  For most operating systems the CH340C IC should automatically be recognized but if you find that you need to install a driver check out this article for more information:  https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers

### Optional Accessories (not included)

While the programmer comes with a standard double-female ICSP cable, enabling it to program a board such as an Arduino Uno out of the box, that's just not something that's done very often.  The real power of this programmer is it's compatibility with Tag-Connect cables, enabling you design and program custom boards that have an ICSP interface with a much smaller footprint.  We recommend using either the Tag-Connect TC2030 Legged or, preferably, the smaller TC2030 no-leg version if space is at a premium on your custom board.  See links below for recommended cables:

- [Tag-Connect EC06-Idc 6-pin Castellated Board-Edge Connector](https://www.tag-connect.com/product/ec-06-pcb-edge-connector)
- [Tag-Connect TC2030-IDC-NL (No-Leg version)](https://www.tag-connect.com/product/tc2030-idc-nl)
- [Tag-Connect TC2030-IDC (Leg version)](https://www.tag-connect.com/product/tc2030-idc-6-pin-tag-connect-plug-of-nails-spring-pin-cable-with-legs)
- [Tag-Connect TC2030-CLIP-3PACK](https://www.tag-connect.com/product/tc2030-retaining-clip-board-3-pack)


## License Information

This hardware is released under [Creative Commons Share-alike 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

This device comes preloaded with and unaltered version of the ArduinoISP sketch, which is copyright (c) 2008-2011 by Randall Bohn and released under the simpified BSD license.  More information can be found at https://opensource.org/licenses/bsd-license.php
