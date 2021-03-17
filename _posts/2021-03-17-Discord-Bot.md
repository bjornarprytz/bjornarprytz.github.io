---
tags: discord-bot, rest-api
---

# Making a Discord Bot

I want to set up a Discord Bot that can receive sensor data from an [Arduino with WiFi](https://store.arduino.cc/arduino-uno-wifi-rev2). The Arduino is in the mail, so I'm going to focus on the Discord Bot this week.

Ideally:

- The Arduino can use an API to transmit sensor data.
- The Bot can show graphs on a discord channel, following user prompts
  - Can [Grafana](https://www.google.com/search?client=firefox-b-d&q=grafana+board) be used here?
- I can host the bot on a local machine.

I'm going to start [here](https://discord.js.org/#/).

The [Getting Started guide](https://discordjs.guide/) is fantastic. It has taken me through

1. Making an [Application](https://discord.com/developers/applications) on discord
2. Setting up the repository for the bot, with some good recommendations on Node.js
3. Authorizing the bot with the Discord server.
