# Bit Collector

## What is it?
Bit Collector is an economy Discord bot, players have their own account, and can increase their balance by buying items that increase their accounts balance
per second. Players can compare their accounts between their own and someone elses, and theres an leaderboard that has the top 10 biggest accounts.

Bit Collector consists of three projects, There's the bot, that talks directly with Discord API and our express backend. Then there is the express backend, that talks with the bot, MongoDB and our local redis cache. Then for the admin of the bot, there is the dashboard that can currently show all the players that we have and edit an single player's data with an player editor. I also have configured nginx to work as an reverse proxy to redirect requests to express backend, and it works as an ratelimiter currently too.

I have implemented the caching with redis, with an memory limit of 200mb.
When the limit exceeds, redis evicts any keys that match this redis key eviction policy:

`allkeys-lru: Keeps most recently used keys; removes least recently used (LRU) keys`

The whole project is currently deployed on an AWS Lightsail Ubuntu instance, and you can use the bot in Discord.
If you want to test the bot, you can join this Discord server: https://discord.gg/p7W9ZEXYup
To use the bots commands, you can write /help on an channel and the bot responds with all the current usable commands.
If you want to check out the source code for these projects, just click the names of the repositories below and they will take you to the project repository page.

Dashboard here: [http://3.73.209.127:8080/](http://3.73.209.127:8080/)

## Hours document
[Link to the hours.md file](../main/hours.md)

## Development environment
If you are planning to develop this project in an development environment, you might need to use yarn for package management, and check that you are using the correct node version

Project has been developed using these:
- `Node version 18.12.1`
- `Yarn version 3.3.1`

And check that vscode is using the correct typescript version like this:

![gif](https://imgur.com/RAVFUoV.gif)

If this doesnt work, you can try installing the yarn sdks yourself with this:
`yarn dlx @yarnpkg/sdks vscode`
try the above step again, and now vscode should use typescript correctly


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
