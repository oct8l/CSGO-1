# What is Counter-Strike: Global Offensive?
Counter-Strike: Global Offensive (CS: GO) expands upon the team-based action gameplay that it pioneered when it was launched 19 years ago. CS: GO features new maps, characters, weapons, and game modes, and delivers updated versions of the classic CS content (de_dust2, etc.).
This Docker image contains the dedicated server of the game.

>  [CS:GO](https://store.steampowered.com/app/730/CounterStrike_Global_Offensive/)

<img src="https://upload.wikimedia.org/wikipedia/en/thumb/1/1b/CS-GO_Logo.svg/1920px-CS-GO_Logo.svg.png" alt="logo" width="300"/></img>

# How to use this image
## Mapping a volume
This container is able to make use of an external volume mapped at `csgo-install` so multiple servers can use the same files and not need a ~20GB download for each. Make sure to include the `-v` flag when starting the servers, and specify where the game files should be installed (in case of the first container being run) or updated (subsequent containers or restarts of the original container).

To create a volume at /opt/csgo-install for example, run the following commands as your user you will use Docker as:

```console
sudo mkdir /opt/csgo-install
sudo chown $USER:$USER /opt/csgo-install/
```

## Hosting a simple game server

Running on the *host* interface (recommended):<br/>
```console
$ docker run -d --net=host --name=csgo-dedicated -v /opt/csgo-install:/csgo-install -e SRCDS_TOKEN={YOURTOKEN} oct8l/csgo-pug
```

Running multiple instances (increment SRCDS_PORT and SRCDS_TV_PORT):
```console
$ docker run -d --net=host -e SRCDS_PORT=27016 -v /opt/csgo-install:/csgo-install -e SRCDS_TV_PORT=27021 -e SRCDS_TOKEN={YOURTOKEN} --name=csgo-dedicated2 oct8l/csgo-pug
```

`SRCDS_TOKEN` **is required to be listed & reachable;** [https://steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers)<br/><br/>
**It's also recommended to use "--cpuset-cpus=" to limit the game server to a specific core & thread.**<br/>
**The container will automatically update the game on startup, so if there is a game update just restart the container.**

# Configuration
## Environment Variables
Feel free to overwrite these environment variables, using -e (--env):
```dockerfile
SRCDS_RCONPW="changeme" (value can be overwritten by csgo/cfg/server.cfg)
SRCDS_PW="changeme" (value can be overwritten by csgo/cfg/server.cfg)
SRCDS_PORT=27015
SRCDS_TV_PORT=27020
SRCDS_FPSMAX=999
SRCDS_TICKRATE=128
SRCDS_MAXPLAYERS=14
SRCDS_STARTMAP="de_dust2"
SRCDS_REGION=255
SRCDS_MAPGROUP="mg_active"
```
## Config
A server configuration should be placed at <server installation location>/csgo/cfg/server.cfg

If you want to learn more about configuring a CS:GO server check this [documentation](https://developer.valvesoftware.com/wiki/Counter-Strike:_Global_Offensive_Dedicated_Servers#Advanced_Configuration).