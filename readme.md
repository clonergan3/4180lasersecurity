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
- 4DGL uLCD (uLCD-144-G2 GFX)

## Schematic

## Development
The mbed was programmed using ARM's Keil Studio Cloud IDE and a number of libraries, including SDFileSystem (https://os.mbed.com/handbook/SDFileSystem), 4DGL-uLCD-SE (https://os.mbed.com/users/4180_1/code/4DGL-uLCD-SE/docs/tip/classuLCD__4DGL.html), and wave_player (https://os.mbed.com/users/sravet/code/wave_player/). The code is built on 
the mbed RTOS, allowing for concurrent programming and functionality. The Adafruit Bluefruit Connect app is used to send UART messages to the mbed.
On the Pi side, Node-RED (built on Node.JS) and PushBullet were used to facilitate the IoT capability.

## Structure
A simplified state diagram describing the basic operation is given below:

![projectstates drawio](https://github.com/clonergan3/4180lasersecurity/assets/163477135/add15835-ad0c-4533-a381-9ceafc31bac8)

Although the most common transitions are shown, any state can be reached using a combination of the passkey and the corresponding number
entered using bluetooth.

Alarm States:
1 - Disarmed
2 - Armed
3 - Arming in 10 seconds
4 - Triggered

Sound States:
5 - Sound Enabled
6 - Sound Disabled

Upon each trigger generated by the light sensor,
the intrusion count is incremented and a signal is sent to the Pi
to capture a picture. Disarming the device
resets the intrusion count. If the audio alarm is enabled, the alarm continues
to sound until the device is disarmed.

## Usage
To use the security system, the user must enter the code (default: 1324), a space, and the
corresponding state number using the Bluefruit Connect app. Entering the user's Pushbullet API key
in Node-RED on the Pi allows pictures to be received.

![Screenshot_20240416_062059_Bluefruit Connect](https://github.com/clonergan3/4180lasersecurity/assets/163477135/b61522cf-8f3b-43c8-add3-fc08eb48394f)


## Demonstration



