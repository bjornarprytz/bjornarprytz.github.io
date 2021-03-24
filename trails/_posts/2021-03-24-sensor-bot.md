---
tags: arduino, discord-bot
---

# Goal

Program an [Arduino WiFi r2](https://store.arduino.cc/arduino-uno-wifi-rev2) to post sensor data to a [discord bot](https://discord.js.org/#/).

Repos

- [Arduino](https://github.com/bjornarprytz/chili-sensor)
- [DiscordBot](https://github.com/bjornarprytz/sensor-bot)

## Uploading Code to the Board

The board is outputting a weird warning when I upload a sketch (blink.ino):

> avrdude: WARNING: invalid value for unused bits in fuse "fuse5", should be set to 1 according to datasheet
> This behaviour is deprecated and will result in an error in future version
> You probably want to use 0xcd instead of 0xc9 (double check with your datasheet first).

but according to [these](https://github.com/arduino/Arduino/issues/9443) [posts](https://forum.arduino.cc/index.php?topic=656596.0) it is _normal and expected_.

## Using the Serial Port(s)

I had some issues initially communicating on the USB serial port interface. The [board](https://www.arduino.cc/en/Guide/ArduinoUnoWiFiRev2) uses **Serial** for USB, and **Serial1** for the RX/TX pins on the board. There's a **Serial2** (connected to something called u-blox NINA-W13 module) as well, but I don't know what the use case is for that.

### Baud Rate

The SerialMonitor doesn't work properly until I explicitly set the baud rate (bits per second). I wonder if I can set that as default somewhere.
*Update*: Seems like it's hard coded into this file:

> vscode\extensions\vsciot-vscode.vscode-arduino-0.4.0\out\src\serialmonitor\serialMonitor.js

After some testing, it seems there's a bug, which causes the Serial Monitor to mangle output from the Arduino until I use the [changeBaudRate()](https://github.com/microsoft/vscode-arduino/blob/master/src/serialmonitor/serialMonitor.ts#L177) function. After that, the serial monitor works as desired.

## Programming the Board

I tried using the included examples in the Arduino extension, but they use the [WiFi.h](https://www.arduino.cc/en/Reference/WiFi) library, did not seem to fit my model. Instead, I'm going to use [WiFiNINA.h](https://www.arduino.cc/en/Reference/WiFiNINA). It was very easy adding the library through the Arduino Library Manager.
