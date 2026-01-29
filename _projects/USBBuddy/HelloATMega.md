---
title: "Hello ATMega"
layout: InnerLayout            
permalink: /usbbuddy/HelloATMega/   
---

Here we'll demonstrate how to send and receive serial text using the Arduino IDE.  For this example, we'll use a ATMega328P/Pro Mini module as our microcontroller to keep it simple though the concepts can be applied to your custom modules as well.

For this example you'll need the following components
- A [USB-C cable](https://www.sparkfun.com/usb-3-1-cable-a-to-c-3-foot.html)
- [Pin Headers](https://www.sparkfun.com/header-pins-14x1.html)
- A [breadboard](https://www.sparkfun.com/breadboard-full-size-bare.html) and a [male-to-male jumper wires](https://www.sparkfun.com/jumper-wires-premium-6-m-m-pack-of-10.html)
- a [4.7kΩ resistor](https://www.amazon.com/dp/B09PLNPX3P?ref_=ppx_hzsearch_conn_dt_b_fed_asin_title_2&th=1)
- a [100nF (0.1uF) capacitor](https://www.amazon.com/dp/B085RDTCCV?ref_=ppx_hzsearch_conn_dt_b_fed_asin_title_1&th=1)
- A [3.3v/8MHz Atmega328P Module](https://www.amazon.com/HiLetgo-ATmega328P-Oscillator-Compatible-ATmega128/dp/B07RS911JD/ref=sr_1_1?crid=2XX5O92W51KY5&dib=eyJ2IjoiMSJ9.3cETzf54YDlehGHQvoVDiQ2OYwxbPZVTnwWWRwbrBDXdQB8PqRsPPvXYszRcHeellDz9JqJS13-nzzhL22qzFj45zdAtakwfXc_mrf8PptTV1Wtgn5GVUBPSOSVEt8a3beSxQHMcjPakd812wA2waqJ-0kME4R8LqAhN2D2qAgXl0kd_s94av2q8DeCoLvmgeCJGe5xpGkVClefxt3b8ozh_jDuSVOM_z43peC0DFOs.g6b-XhXh12nQCdZQ9DKUea2MDF29PvzY0qVvodVY_qs&dib_tag=se&keywords=arduino+pro+mini+3.3v&qid=1748048550&sprefix=arduino+pro+mini+3.3v%2Caps%2C110&sr=8-1)

## 1. Wiring it all up

1. Add the adapter and the ATMega328P module to the breadboard
2. Select the 3V3 option on the adapter by jumping the middle and bottom pins together.  Note, it's important to complete this step prior to proceeding to ensure the correct voltage is sent to the target once connected.
3. Connect the VCC pin on the adapter to the VCC pin on the target module.  Note that many Arduino Pro Mini clones have a misprint on the silkscreen where VCC is mistakenly labeled as ACC.  If that's the case then go ahead and connect to that pin on the target module.
4. Connect the GND pin on the adapter to any GND pin on the target module
5. Connect the TXO pin on the adapter to the RXI pin on the target module
6. Connect the RXI pin on the adapter to the TXO pin on the target module
7. Connect the DTR# pin on the adapter to one end of the 100nF/0.1uF capacitor. Connect the other end of the capacitor to the RST pin on the target module.  This capacitor is required when using the RST pin to set the reset timing when loading a sketch
8. Connect one end of the 4.7kΩ resistor to the CTS# on the adapter and the other end to the DTR# pin on the adapter.  This configures the adapter for push-pull mode, which is needed for the ATMega328P.  Alternatively you can solder a [1206 SMD 4.7kΩ resistor](https://www.amazon.com/dp/B08QRT5ZLW?ref_=ppx_hzsearch_conn_dt_b_fed_asin_title_1&th=1) on the back of the adapter.
9. Connect the USB-C cable to your computer and the adapter

   Note:  In our example we wired the output of the adapter directly into the VCC pin of the target module since the adapter is set to 3.3v.  The VCC pin on this module expects 3.3v so be sure that the jumper is set to 3V3 prior to applying power via the USB cable.  Alternatively you can connect the VBUS pin on the adapter (which supplies 5v) to the RAW pin on the adapter.  In either case, be sure to set the adapter's jumper to 3V3 to ensure the correct logic levels for the RX, TX and DTR signals.

## 2. Configuring the Arduino IDE

Now that everything is wired up you'll need to configuure your Arduino IDE for the target.  

1. Open the Arduino IDE and create a new blank sketch
2. Under Tools/Board, select "Arduino Pro or Pro Mini"
3. Under Tools/Port, select the port that was assigned to the adapter.
4. Under Tools/Processor, select "ATmega328p (3.3v, 8MHz)"

## 3. Loading a 'Hello World' Sketch

Now that everything is wired up it's time to test it with a simple sketch.  Replace the default code in the IDE with the code below and press Upload (right arrow button).  

``` cpp
void setup() {
    pinMode(LED_BUILTIN, OUTPUT); // Set LED pin as output
    Serial.begin(9600); // Start serial communication at 9600 baud
}

void loop() {
    digitalWrite(LED_BUILTIN, HIGH); // Turn LED on
    delay(500); // Wait 500ms
    digitalWrite(LED_BUILTIN, LOW); // Turn LED off
    
    Serial.println("Hello, world!"); // Print message to serial port
    delay(2500); // Wait for the remaining time (3s total delay)
}

```
If everything is connected correctly, you should get an upload success message and the LED on the target board should blink every 1/2 second.  Now open the serial monitor and you should see "Hello, world!" printed every 3 seconds.  
