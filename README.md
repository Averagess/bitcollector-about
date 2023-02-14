# Bit Collector

Bit Collector consists of two projects, there is the bot project that talks with the Discord API directly and provides the players information by
talking with the express backend. I also have configured nginx to work as an reverse proxy to redirect requests to express backend, and it works as an ratelimiter currently too.

I have also implemented some caching with redis, with an memory limit of 200mb.
When the limit exceeds, redis evicts any keys that have according to this redis key eviction policy that i use:
`allkeys-lru: Keeps most recently used keys; removes least recently used (LRU) keys`

Both the bot and express server are currently deployed on an AWS Lightsail Ubuntu instance, and you can use the bot in Discord.
If you want to test the bot, you can join this Discord server: https://discord.gg/p7W9ZEXYup
To use the bots commands, you can write /help on an channel and the bot responds with all the current usable commands.
If you want to check out the source code for these projects, just click the names of the repositories and they will take you to the project repository page.

## [bitcollector-server](https://github.com/Averagess/bitcollector-server)
This project contains the express backend, that talks with the mongo database and redis cache, and relays the information to the bot
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
- First clone both the bitcollector-server and bitcollector-bot in an empty directory, like "bitcollector"
- Then clone the docker-compose.yaml and nginx.conf files from this repository to that empty directory that has both the server and bot in it
- Edit the docker-compose.yaml file and replace all values surrounded with <> with your own properties
- Run docker compose up
- Bot, express, redis, nginx should now be running correctly in Docker compose

By default, the express server doesnt serve any static content. To serve the dashboard you need to do this:
- Pull the bitcollector-dashboard project to the root directory where the bot and server are.
- CD into this directory, and install dependencies
- Run the build script inside package.json `npm run build`
- Copy the contents of dist/ directory into the bitcollector-server/public/ directory
- run `docker compose down && docker compose up --build` in the root directory of all the 3 projects
- express should now be serving the dashboard
