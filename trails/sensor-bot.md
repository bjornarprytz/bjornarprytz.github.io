---
layout: post
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

I tried using the included examples in the Arduino extension, but they use the [WiFi.h](https://www.arduino.cc/en/Reference/WiFi) library, did not seem to fit my model. Instead, I'm going to use [WiFiNINA.h](https://www.arduino.cc/en/Reference/WiFiNINA). It was very easy adding the library through the [Arduino Library Manager](https://www.arduino.cc/en/guide/libraries).

## Updating Firmware

I updated the WiFiNINA Firmware using [this guide](https://support.arduino.cc/hc/en-us/articles/360013896579-How-to-update-the-WiFi-Nina-and-WiFi101-firmware).

> Even after updating to 1.3.0, I'm getting reminders that there's a newer version (1.4.1). [This forum post](https://forum.arduino.cc/index.php?topic=709150.0) states that this will become available with the next Arduino IDE version.

It looks like there's an option to add certificates to the board as well (though I did not do so at this time).

## Bot

[Previous post](../_posts/2021-03-17-Discord-Bot.md)

Back in the discord bot side of things, I'm continuing to follow the [guide](https://discordjs.guide/creating-your-bot/commands-with-user-input.html#mentions), where I left off in [this post](../_posts/2021-03-17-Discord-Bot.md). I'm tackling commands, and JS (it's a blast!).

Found out there's a difference between the [in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) and [of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) keyworlds in javascript:

```js
const collection = ['a', 'b', 'c']

for (const item in collection) {
    // item is an enumerable (0, 1, 2)
}

for (const item of collection) {
    // item is an object (a, b, c)
}
```

It turns out it's not necessary to *join* a channel with the bot. I just had to *add* the bot through my regular Discord client.

### Events

Finished the [event handler guide](https://discordjs.guide/event-handling/#event-handling). Now, adding events is easy as pie!

### Commands

Added some placeholder commands, that I can implement later.

- `!graph <ph|ec|hum|all> default:all`
- `!read <ph|ec|hum|all> default:all`
- `!configure <event_name> <params>`

## Todo

- Bot
  - Upgrage to [slash commands](https://gist.github.com/advaith1/287e69c3347ef5165c0dbde00aa305d2)
    - Can users type `/` to see available commands?
    - [Good place to start?](https://stackoverflow.com/questions/65402187/new-discord-slash-commands)
  - Read from database.
  - Add interesting commands
    - `!graph <ph|ec|hum|all> default:all` (can I use [grafana](https://grafana.com/) for this?)
    - `!read <ph|ec|hum|all> default:all`
    - `!configure <event_name> <params>`
  - Add interesting events
    - alert (on sensor levels)
- Arduino
  - Hook up sensors
  - Post to database
