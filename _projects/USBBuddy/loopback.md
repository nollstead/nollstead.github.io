---
title: "USB Buddy — Loopback Test"
layout: InnerLayout            
permalink: /usbbuddy/loopback/   
---

For our first example we'll keep it simple and just do a basic loopback test - essentially verifying the adapter is recognized by your computer and you understand the basic functionality.

For this example you'll need the following components
- A [USB-C cable](https://www.sparkfun.com/usb-3-1-cable-a-to-c-3-foot.html)
- A terminal emulator, such as [Putty](https://www.putty.org/), [Tera Term](https://sourceforge.net/projects/tera-term/) or [SecureCRT](https://www.vandyke.com/cgi-bin/releases.php?product=securecrt).
- [Pin Headers](https://www.sparkfun.com/header-pins-14x1.html)
- A [breadboard](https://www.sparkfun.com/breadboard-full-size-bare.html) and a [male-to-male jumper wire](https://www.sparkfun.com/jumper-wires-premium-6-m-m-pack-of-10.html) or a [female-to-female jumper wire](https://www.sparkfun.com/jumper-wires-premium-6-f-f-pack-of-10.html) (if not using a breadboard)

## 1. Connect the adapter

Connect the adapter to your computer using the USB-C cable (either USB-A to USB-C or USB-C to USB-C, depending on your computer).  Your computer should automatically install the necessary drivers and assign it a COM port.  

## 2. Solder on pin headers & connect TXO and RXI

The adapter comes without the pin headers soldered on, so that you have the option of either soldering on the bottom (for connecting to a breadboard) or on the top (for placing in an enclosure).  For this and subsequent examples we'll assume this is used on a breadboard and solder the pin headers to the bottom.

Using a jumper wire, connect the TXO and RXI pins together.

## 3.  Test

Now that you have everthing seup, open your favorite terminal program and select the COM port assigned and connect.  When you type a character, you should see each charater you type echoed back in the terminal and the RX and TX LEDs will flash as you type.

You can experiment with disconnecting the RXI pin - here you should see the TX LED flash but not the RX LED and no characters will be displayed.


