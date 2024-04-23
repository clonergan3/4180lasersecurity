# Mbed IoT Laser Tripwire Security System
ECE 4180 Final Project  <br />
Cullen Lonergan, Dario Tscholl

The mbed IoT Laser Tripwire Security System is a device capable of detecting intruders through
the use of an invisible laser tripwire. This system is internet of things (IoT) enabled and incorporates text notification
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
- P-Channel MOSFET (optional)
- RGB LED
- 330Ω, 10kΩ Resistors
- 2x 5V Power Supplies
- 10µF Capacitor
- Red LED
- 4DGL uLCD (uLCD-144-G2 GFX)

## Schematic

![4180schematic drawio](https://github.com/clonergan3/4180lasersecurity/assets/163477135/8360bc07-e628-44cb-a16d-cb9b48b07c2b)

Not pictured: The class-D amplifier, Pi, Bluefruit, and laser use 5V external power. A P-channel MOSFET can also be used to switch the SD card power using p30, improving SD reliability.

## Development
The mbed was programmed using ARM's Keil Studio Cloud IDE and a number of libraries, including SDFileSystem (https://os.mbed.com/handbook/SDFileSystem), 4DGL-uLCD-SE (https://os.mbed.com/users/4180_1/code/4DGL-uLCD-SE/docs/tip/classuLCD__4DGL.html), and wave_player (https://os.mbed.com/users/sravet/code/wave_player/). The code is built on 
the mbed RTOS, allowing for concurrent programming and functionality. The Adafruit Bluefruit Connect app is used to send UART messages to the mbed.
On the Pi side, Node-RED (built on Node.JS) and PushBullet were used to facilitate the IoT capability.

A structure to mount the Raspberry Pi camera and uLCD was designed in Autodesk Fusion and 3D printed on a Prusa i3 Mk3 using PLA. The STL files are available here: <br />
[click to download](https://github.com/clonergan3/4180lasersecurity/raw/main/RPI_Parts.zip) <br />
<img width="575" alt="camera-cad" src="https://github.com/clonergan3/4180lasersecurity/assets/167137160/b50879b7-c049-4c26-8da8-95ec3461d3dc">

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

## IoT Implementation
As already mentioned, this project uses Node-RED and Pushbullet for its IoT integration. Node-RED is an open-source visual programming tool that is used for connecting hardware devices, APIs, and online services through a browser-based flow editor. Node-RED allows to drag and drop nodes onto a canvas and wire them together to perform various tasks.
For this project, Node-RED was used to read an input signal sent from the Mbed to trigger the Raspberry Pi camera. After that, Node-RED sends the picture taken to any mobile device via Pushbullet. The implementation in Node-RED looks like the following:

<img width="400" alt="Node-Red" src="https://github.com/clonergan3/4180lasersecurity/assets/167137160/7731e47a-cf5c-4513-9027-d36b6a31b03a">

Note that a delay function node was added so that the user will receive 1 image every 2 seconds. This prevents an overflow of push notifications should the light sensor be triggered multiple times in a row. To access the camera and Pushbullet nodes, the following two packages have to be installed first under _Settings/Manage palette_:

**contrib-camerapi:** <br />
<img width="400" alt="nr_cam" src="https://github.com/clonergan3/4180lasersecurity/assets/167137160/421ad3b5-1a24-472f-a8cc-b23aeb9fd851">
<br />

**node-pushbullet:** <br />
<img width="400" alt="nr_pushbullet" src="https://github.com/clonergan3/4180lasersecurity/assets/167137160/1fabcbdf-210e-43a0-a4ff-53911b036ef9">

While the camera node can be used as is, the Pushbullet node first needs to be configured. Double-clicking on the node opens the editor that looks as follows:

<img width="686" alt="Pushbullet_config" src="https://github.com/clonergan3/4180lasersecurity/assets/167137160/4ed155c4-ab3e-46fa-adb4-025540ea0c9b">

The most important step is to add the API-key from your Pushbullet account to the node setting under _Config/Edit pushbullet-config node_. _Device ID_ allows the user to select the devices that should receive the push notification. Once the _Type_ has been set to file, because an image should be pushed, a _Title_ for the push notification can be chosen. Pushbullet works on PC and Mac and is available for Android. Unfortunately, Pushbullet is not available in the App Store anymore. More information about Pushbullet can be found on the website (https://www.pushbullet.com/).

## Usage
To use the security system, the user must enter the code (default: 1324), a space, and the
corresponding state number using the Bluefruit Connect app. Entering the user's Pushbullet API key
in Node-RED on the Pi allows pictures to be received.


<img src="https://github.com/clonergan3/4180lasersecurity/assets/163477135/b61522cf-8f3b-43c8-add3-fc08eb48394f" width="300">


## Demonstration
[![Watch the video](https://img.youtube.com/vi/5XX1oY2-kMY/maxresdefault.jpg)](https://youtu.be/5XX1oY2-kMY)


