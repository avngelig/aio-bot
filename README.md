# aio-bot
Placeholder 
<!-- coderabbit-ai-generated:v1 -->

<!-- coderabbit-ai-generated-section:v1:36a5d6296b10bc0c86d2578ea0dd41259e5302d3c5e4147f9f7dd1f25f1f663b -->

# AIO Bot

A Discord slash-command bot for moderation, server management, community activities, and utilities. It runs as a single Node.js process and stores settings, warnings, and reminders in a local JSON file.

## Features

- **Moderation:** `/ban`, `/kick`, `/timeout`, `/warn`, `/warnings`, and `/clear`. Moderation commands require the corresponding Discord permission; members can only moderate people below their own highest role. Bulk deletion skips messages over 14 days old.
- **Server settings:** `/config welcome` and `/config logs` select channels; omit the channel to disable either feature. `/config show` displays the current settings. Warnings and moderation actions are logged when a log channel is configured.
- **Community:** `/poll` creates a yes/no reaction poll; `/roll` and `/choose` help with decisions.
- **Utilities:** `/ping`, `/avatar`, `/server`, `/help`, and `/remind set`, `/remind list`, `/remind cancel`. Reminders persist across restarts and are sent to the channel where they were created. Durations use a single unit, such as `30s`, `10m`, `2h`, or `7d`.

## Setup

1. Install Node.js 20 or newer and run `npm ci`.
2. Create an application and bot in the [Discord Developer Portal](https://discord.com/developers/applications). Under **Bot**, enable the **Server Members Intent** for welcome messages and member lookups. The bot requests this privileged intent at startup.
3. Copy `.env.example` to `.env`. Set `DISCORD_TOKEN` to the bot token and `CLIENT_ID` to the application ID. Never commit or share the token. Optionally set `GUILD_ID` to a server ID for fast, server-specific command registration during development.
4. Invite the bot using the application's installation page with the `bot` and `applications.commands` scopes. Grant the permissions needed for the features you use: view and send messages, add reactions, manage messages, kick members, ban members, and moderate members. Place the bot's role above the members it needs to moderate.
5. Run `npm run deploy` to register slash commands, then `npm start` to run the bot. Commands registered to a server normally appear quickly; global commands can take longer to propagate.

`npm run deploy` replaces this application's commands in the chosen scope. If switching from guild to global registration, remove the old guild commands separately to avoid duplicates. The bot must remain running to respond to commands, welcome members, and deliver reminders.

## Data and operation

Data is saved to `./data/bot.json` by default; set `DATA_FILE` to choose a different path. Back up this file if warnings, reminders, and settings matter to you. The file is local to the process and is not designed for multiple bot instances sharing it. Failed reminder deliveries are retried while the bot is running; the bot needs access to the original channel. Warnings are stored locally and are not automatically sent to the warned member.

Use `/config welcome` and `/config logs` after inviting the bot. Make sure it can view and send messages in the chosen channels. Run `npm test` for offline tests.
