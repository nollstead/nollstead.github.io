---
title: "BTAudio Testbed Setup"
layout: default
permalink: /btaudio/testbed/
breadcrumb:
  - { title: "Home", url: "/" }
  - { title: "Projects", url: "/projects/" }
  - { title: "Bluetooth Audio", url: "/btaudio/" }
  - { title: "Testbed" }
---

# BTAudio Testbed Setup v2

Version 2 of the BTAudio PCB focuses on the audio portions (RCA jacks, ES8388, BD37033, etc.), digital and power are still supplied externally via pin headers.

## RCA Board

<p align="center">
  <a href="/assets/projects/btaudio/testbed2.png" target="_blank" rel="noopener noreferrer">
    <img src="/assets/projects/btaudio/testbed2.png" alt="testbed" width="35%">
  </a>
</p>

- The Triple-V pcb is intended to supply all power needs from the amplifier (8v to bd37033, 3v3 to esp32 and ES8388 and 5v to ADC pins) and is wired in as follows:
  - Connect power/gnd to amplifier via the +/- connectors on the green connector
  - (Optional) Connect a jumper wire from the REM port on the amplifier to the + pin header next to the green connector.  Note that for the initial setup this just supplies 12v continuously, in the **Amplifier REM Signal** section below we'll wire this into the REM circuitry
  - Connect the 3V3 (+) pin header on the Triple-V to the 3V3 pin on the pcb
  - Connect the 5v (+) pin header on the Triple-V to the 5V pin on the pcb
  - Connect the 8v (+) pin header on the Triple-V to the 8v pin on the pcb
  - Connect any GND pin header on the Triple-V to any GND pin on the pcb (only 1 needs to be connected)

- Connect the I2C and I2S signals from the ESP32 to the pcb as follows:
  - Connect a F/F jumper from GPIO 5 on the ESP32 to the BCLK pin on the pcb
  - Connect a F/F jumper from GPIO 19 on the ESP32 to the DIN pin on the pcb
  - Connect a F/F jumper from GPIO 0 on the ESP32 to the MCLK pin on the pcb
  - Connect a F/F jumper from GPIO 26 on the ESP32 to the DOUT pin on the pcb
  - Connect a F/F jumper from GPIO 25 on the ESP32 to the LRCK pin on the pcb
  - Connect a F/F jumper from GND on the ESP32 to any GND pin on the pcb
  - Connect a F/F jumper from GPIO 23 on the ESP32 to the SCL pin on the pcb
  - Connect a F/F jumper from GPIO 18 on the ESP32 to the SDA pin on the pcb

  
- Connect the ESP32 to your computer via USB.  Note for this setup we're powering the esp32 from the 5v USB but bypassing it's LDO - so do not connect the 3V3 pin on the 

## (Optional) Amplifier REM Signal

<p align="center">
  <a href="/assets/projects/btaudio/testbed2withREM.png" target="_blank" rel="noopener noreferrer">
    <img src="/assets/projects/btaudio/testbed2withREM.png" alt="testbed" width="35%">
  </a>
</p>


This version of the pcb does not include the circuitry for controlling the REM signal on the amplifier, however the circuitry has been worked out and and can be added via a separate BD846BPN breakout board on a breadboard and wired in as follows:.  This can be added to a breadboard for this test if desired.  

- Add the BC846BPN breakout pcb to a breadboard
- Connect a F/M jumper from GPIO 27 on the esp32 to an open breadboard space, then connect a 10kΩ resistor there to PIN 2
- Connect a 100kΩ resistor from pin 2 to the GNd rail
- Connect a 10kΩ resistor from pin 6 to an open breadboard column, then connect a 1kΩ resistor from there to pin 5
- Connect a 100kΩ resistor from pin 3 to the GND rail
- Disconnect the 12v wire that connects to the REM port (from the Triple-V) and connect to the breadboard power rail
- Connect a 47kΩ resistor from pin 5 to the power rail
- Connect pin 4 to the power rail
- Connect pin 1 to any GND rail
- Connect the GND pin to the GND rail
- Connect a F/M jumper from any GND port on the breakout board to the breadboard GND rail
- Connect pin 3 to the REM port on the amplifier.




