# Mbed IoT Laser Tripwire Security System
ECE 4180 Final Project
Cullen Lonergan, Dario Tscholl

The mbed IoT Laser Tripwire Security System is a device capable of detecting intruders through
the use of a laser tripwire. This system is internet of things and enabled and incorporates text notification
functionality to alert owners of potential intrusions. Bluetooth is used to control the device.

## Features
The system detects breaks in a laser beam using a light sensor and supporting circuitry. When 
an intrusion is detected, an alarm is sounded and a signal is sent to a raspberry pi to take a picture
of the area and send it to the user's phone. The device can be armed, disarmed, and temporarily disarmed 
using a passcode and a bluetooth enabled phone. In addition, the audible alarm may be disabled using bluetooth.
The state of the device and the number of intrusions counted since arming the device is displayed on an LCD. 
In addition to the LCD, visual cues are also given through an RGB LED.

## Key Components
- MBED LPC1768
- Raspberry Pi Zero W 2
- 980nm 5mW Laser Diode
