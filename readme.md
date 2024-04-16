# Mbed IoT Laser Tripwire Security System
ECE 4180 Final Project
Cullen Lonergan, Dario Tscholl

The mbed IoT Laser Tripwire Security System is a device capable of detecting intruders through
the use of an invisible laser tripwire. This system is internet of things and enabled and incorporates text notification
functionality to alert owners of potential intrusions. Bluetooth is used to control the device.

## Features
The system detects breaks in a laser beam using a light sensor and supporting circuitry. When 
an intrusion is detected, an alarm is sounded and a signal is sent to a raspberry pi to take a picture
of the area and send it to the user's phone. The device can be armed, disarmed, and temporarily disarmed 
using a passcode and a bluetooth enabled phone. In addition, the audible alarm may be disabled using bluetooth.
The state of the device and the number of intrusions counted since arming the device is displayed on an LCD. 
In addition to the LCD, visual cues are also given through an RGB LED.

## Components
- MBED LPC1768
- Raspberry Pi Zero W 2
- Raspberry Pi Camera Module
- 980nm Infrared 5mW Laser Diode
- 940nm Infrared Photodiode
- Adafruit Bluefruit UART Module
- SD Card and SD Card Breakout
- Sparkfun Class D Amplifier Breakout (TPA2005D1)
- Speaker
- TL071CP Operational Amplifier (Comparator may also be used)
- P-Channel MOSFET
- RGB LED
- 330Ω, 10kΩ, 100Ω Resistors
- 2x 5V Power Supplies
- 10µF Capacitors
- Red LED

## Schematic



