Welcome to the very work in progress ServerGridEditor wiki!

## Overview 
To get an Atlas grid running, you have to run Redis and game server(s). To link these all together you need to configure the ServerGrid JSON files. This tool helps you generate those.

## Grid Setup
Servers must be flagged as a homeserver via the ServerGrid tool to allow spawning in them, and use one of the freeport islands that has spawnpoints (otherwise players will spawn at the origin, though we'll be improving that to automatically spawn along the cost soon if no spawnpoints are found)
Alternatively, you can launch a server with ``-ForceAllHomeServer`` to make all server/grid act as a homeserver and thus allow spawning on it.

### Exporting JSON config
Use your preferred tool to export(More details later).

## Starting Redis
You can use the included windows binary with the server files `AtlasTools/RedisDatabase/` or run your copy. Newer versions work just fine on Linux! No special parameters are required, but we highly recommend that you do set a custom password.

## Starting an Atlas Server

Once you have your configuration exported you will end up with files like the following:
* ServerGrid.json
* ServerGrid.ServerOnly.json
* ServerGrid/*.jpg (Map images for display)

These will need to go into the ShooterGame/ folder. By default it looks for "ServerGrid" if you wish to use an alternative one you may run with the optional cmd line ``-GridConfig="MyServerConfig.json"``

Once those files are in place, you have to load up the ocean map with its coordinates on the grid.

Example: ``ShooterGameServer.exe Ocean?ServerX=1?ServerY=0?AltSaveDirectoryName=B1?MaxPlayers=100?ReservedPlayerSlots=30?QueryPort=57561?Port=5761?SeamlessIP=1.2.3.4 -log -server``

The important bits being which grid location to use `?ServerX=1?ServerY=0` and your own external IP `?SeamlessIP=1.2.3.4` This is your public facing IP, not an internal network one.

### ServerGrid.ServerOnly.json Values
* ``BaseServerArgs`` and ``MachineIdTag`` Don't actually do anything by default. We use them for our internal tools.
* ``gridSize`` is the size in unreal units of each grid. We do not recommend going over 1400000 due to floating point precision issues.
* ``WorldAtlasId`` Set a unique id to make your servers line up

More to come soon!

### ServerGrid.json Values
* ``DatabaseConnections`` This is your connection to Redis! Yes, you can keep them all the same, and they will share a connection. We will likely cut these down in the future.

## Some Atlas Server ini values
## Game.ini
[/Script/ShooterGame.ShooterGameMode]
* bDontUseClaimFlags -- set this to disable claimflags entirely, on any grid
* NoClaimFlagDecayPeriodMultiplier -- set this value to scale-up the auto decay of any structures that are not witin a claimflag
* PlayerDefaultNoDiscoveriesMaxLevelUps -- how many level ups to allow players to achieve before discovery zone points are required (all remaining levelups up to the xp ramp limit are subdivided by the discoverzone points)
* bClampHomeServerXP -- limit levelling to a certain amount on a homeserver (requires that the server is also a homeserver)
* ClampHomeServerXPLevel -- what level to limit XP to on this homeserver.

More to come soon!