# Bit Collector

Bit Collector consists of two projects, there is the bot project that talks with Discord directly and provides the players information by
talking with the express backend. I also have configured an nginx container to route all requests to /webhooks/ to proxy them to the express application,
without exposing internal routes used by the bot to the whole internet.

Both the bot and express server are currently deployed on an AWS Lightsail ubuntu instance, and you can use the bot in Discord.
If you want to test the bot, you can join this Discord server: https://discord.gg/p7W9ZEXYup
To use the bots commands, you can write /help on an channel and the bot responds with all the current usable commands.
If you want to check out the source code for these projects, just click the names of the repositories and they will take you to the project repository page.

## [bitcollector-server](https://github.com/Averagess/bitcollector-server)
This project contains the express backend, that talks with the mongo database, and relays the information to the bot
from different endpoints. Endpoints are tested with jest and supertest, and every PR currently is tested and linted with GitHub actions.
The server has some webhooks too, so we can receive some voting events, done on some websites that list Discord bots, if an player
votes on these websites, they receive an crate that they can unbox for virtual rewards.

## [bitcollector-bot](https://github.com/Averagess/bitcollector-bot)
This project contains the Discord bot, that has commands users can use on servers that the bot is in or in private DM's.
The bot communicates with the Express app, and retrieves players data when it is asked from the bot.

## [bitcollector-dashboard](https://github.com/Averagess/bitcollector-dashboard)
This project is an react app for viewing all the players, and has an player editor that can edit an single player's data.
To use the dashboard, you need to sign in with admin credentials.

## Deploying an own copy of the bot
To deploy an copy of the project, you can use Docker compose.
- First clone both the bitcollector-server and bitcollector-bot in an empty directory
- Then clone the docker-compose.yaml and nginx.conf files from this repository
- Replace all values surrounded with <> with your own properties
- Run docker compose up
- Bot and express application should now be running correctly in docker
