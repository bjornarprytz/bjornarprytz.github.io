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
2. Setting up the [repository for the bot](https://github.com/bjornarprytz/sensor-bot), with some good recommendations on Node.js
3. [Authorizing](https://discordjs.guide/preparations/adding-your-bot-to-servers.html) the bot with the Discord server.
4. [Handling commands](https://discordjs.guide/creating-your-bot/commands-with-user-input.html#working-with-multiple-mentions). I need to work out which commands we need for operations here. It would be sweet to see live sensor data, or at least periodic summaries.

Here are the [docs for Discord.js](https://discord.js.org/#/docs/main/stable/general/welcome). NB! Servers are called [Guild](https://discord.js.org/#/docs/main/stable/class/Guild).
