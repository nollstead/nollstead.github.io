---
layout: default
title: "Nextion Switch"
description: "Nextion HMI Serial Switch"
featured: true
tags: [HMI, Arduino, ESP32, Nextion]
image: "/assets/projects/nextionswitch/front.png"
weight: 15
breadcrumb:
  - { title: "Home", url: "/" }
  - { title: "Projects", url: "/projects/" }
  - { title: "Nextion Switch" }
---

# Nextion Switch

A drawback of the Nextion displays is that they only include a single communication port, which is used for both communicating with your microcontroller and uploading new Nextion code using the Nextion Editor. This means repeatedly swapping the wires between your microcontroller and the Nextion, uploading, swapping back to test - which can be frustrating when in development mode. Additionally it requires a separate USB-to-Serial adapter - which just adds to the mess of connections to manage.

This handy device solves both of those problems. Wire this in-line with your Nextion, your PC, your microcontroller and separate power source and it'll automatically switch those wires for you - no need to constantly remove wires.

## Features

- USB-C Connector with Electrostatic Discharge (ESD) protection
- Embedded FTDI USB-to-Serial converter, so no external converter required
- Reverse voltage protection for power pins protects board against accidential reverse wiring of power pins
- 4-wire JST-XH connector for connectng to your Nextion display. This connector will provide power as well as Tx/Rx signals for both Nextion editor and MCU communication
- RoHS Compliant parts and manufacturing processes

## Setup

### PC USB

### Power

### MCU Connection

### Nextion Connection

### Operation