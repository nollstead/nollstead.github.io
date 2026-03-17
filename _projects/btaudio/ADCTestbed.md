---
title: "BTAudio ADC Testbed Setup"
layout: default
permalink: /btaudio/ADCTestbed/
breadcrumb:
  - { title: "Home", url: "/" }
  - { title: "Projects", url: "/projects/" }
  - { title: "Bluetooth Audio", url: "/btaudio/" }
  - { title: "ADCTestbed" }
---

# BTAudio ADC Testbed

The current (v2) version of the audio pcb does not include the required pullup resistors for the factory gauges.  The purpose of this board is to test just that functionality, without audio (to simplify the test)

## Pinout

| ADS1115 | Pin | Function |
|---------|-------------|-------------|
| 1 (left) | A0 | Fuel Level |
| 1 (left) | A1 | Coolant Temp |
| 1 (left) | A2 | Oil Temp |
| 1 (left) | A3 | Oil Pressure |
| 2 (Right) | A0 | AFRDS |
| 2 (Right) | A1 | Battery Monitor |
| 2 (Right) | A2 | AFRPS |
| 2 (Right) | A3 | Not Used |


## Wiring

<p align="center">
  <a href="/assets/projects/btaudio/adctestbed.png" target="_blank" rel="noopener noreferrer">
    <img src="/assets/projects/btaudio/adctestbed.png" alt="testbed" width="35%">
  </a>
</p>

- Place components on a breadboard in this order (from left to right):  ESP32 -> ADS1115 #1 -> PCA9306 #1 -> ADS1115 #2 -> PCA9306 #2
  - Note that the 'pin side' of the ADS1115's and the "LOW" side of the PCA9306's should face up to match my wiring
- Connect a F/M jumper from the 3v3 pin on the esp32 to the **top** power rail
- Connect a F/M jumper from the 5v pin on the esp32 to the **bottom** power rail
- Connect a F/M jumper from the GND pin on the esp32 to the **top** GND rail
- Connect a F/M jumper from the GPIO23 on the esp32 to the SCL1 pin on PCA9306 #1
- Connect SCL1 on PCA9306 #1 to SCL1 on PCA9506 #2 via a jumper wire
- Connect a F/M jumper from the GPIO18 on the esp32 to the SDA1 pin on PCA9306 #1
- Connect SDA1 on PCA9306 #1 to SDA1 on PCA9506 #2 via a jumper wire
- Connect VREF1 on both PCA9306's to the 3v3 (**Top**) power rail
- Connect GND on both PCA9306's to the GND rail
- Connect VREF2 on both PCA9306's to the 5v (**bottom**) power rail
- Connect EN on both PCA9306's to the 5v (**bottom**) power rail
- Connect SCL on ADS1115 #1 to SCL2 on PCA9306 #1
- Connect SDA on ADS1115 #1 to SDA2 on PCA9306 #1
- Connect SCL on ADS1115 #2 to SCL2 on PCA9306 #2
- Connect SDA on ADS1115 #2 to SDA2 on PCA9306 #2
- Connect the ADDR pin on ADS1115 #1 to GND
- Connect the ADDR pin on ADS1115 #2 to the 5v rail
- Connect the VDD pins of both ADS1115's to the 5v rail
- Connect the GND pins of both ADS1115's to the GND rail
- Connect one end of a 100Ω resitor to A0 on ADS1115 #1 and the other end to the 5v rail
- Connect one end of a 100Ω resitor to A1 on ADS1115 #1 and the other end to the 5v rail
- Connect one end of a 100Ω resitor to A2 on ADS1115 #1 and the other end to the 5v rail
- Connect one end of a 100Ω resitor to A3 on ADS1115 #1 and the other end to the 5v rail
- Connect a 100nF capacitor from a1 on ADS1115 #2 to the GND rail
- Connect a 100Ω resistor from a1 on ADS1115 #2 to an open breadboard column
  - Connect a 33kΩ resistor from that open breadboard column to the GND rail
  - Connect a 100kΩ resistor from that open breadboard column to a second open breadboard column
  - Note.  This second breadboard column will be where we test the battery voltage.
- Connect a 470kΩ resistor from a0 on the ADS1115 #2 to the gnd rail
- Connect a 470kΩ resistor from a2 on the ADS1115 #2 to the gnd rail

## Testing

### Fuel level

Place varying level resistors on a0 to gnd and observe results

| Condition | Voltage | Displayed Result|
|---------|-------------|-------------|
| Jumper wire | 0v | 1.00 |
| 22Ω resistor | 0.69v  | 0.82 |
| 47Ω resistor | 1.43v  | 0.53 |
| 68Ω resistor | 1.82v  | 0.31 |
| 100Ω resistor | 2.24v  | 0.00 |

### Coolant Temperature

Place varying level resistors on a1 to gnd and observe results

| Condition | Voltage | Displayed Result|
|---------|-------------|-------------|
| Jumper wire | 0v | 0 degrees |
| 22Ω resistor | 0.81v  | 55 degrees |
| 47Ω resistor | 1.43v | 118 degrees |
| 68Ω resistor | 1.82v | 172 degrees |
| 100Ω resistor | 2.23v  | 250 degrees |

### Oil Temperature

Place varying level resistors on a2 to gnd and observe results

| Condition | Voltage | Displayed Result|
|---------|-------------|-------------|
| Jumper wire | 0v | 0 degrees |
| 22Ω resistor | 0.81v  | 66 degrees |
| 47Ω resistor | 1.43v | 142 degrees |
| 68Ω resistor | 1.82v | 207 degrees |
| 100Ω resistor | 2.23v  | 300 degrees |

### Oil Pressure

Place varying level resistors on a3 to gnd and observe results

| Condition | Voltage | Displayed Result|
|---------|-------------|-------------|
| Jumper wire | 0v | 0 psi |
| 22Ω resistor | 0.81v  | 22 psi |
| 47Ω resistor | 1.43v | 47 psi |
| 68Ω resistor | 1.81v | 68 psi |
| 100Ω resistor | 2.23v  | 100 psi |

### AFR/O2 sensors

1. With nothing connected the values shoudl read 0
2. Connected a 10kΩ resistor from the 5v rail to an open breadboard column then a jumper from there to either a0 or a2.  You shoudl get a value of about 20
3. Remove the jumper and connect another 10kΩ resistor from that column to gnd (forming a voltage divider) then connect the jumper to the position between the two resistors and a0 or a2.  You should get a value of about 14

Note:  The lower than max/min values are due to the schottky diode reducing the voltage to 4.5v.  This is expected and will be corrected on the final pcb (which won't have a usb/schottky)










