# Minecraft Serve with Canarymod Installed - Taugh Cousin Javascript Through This #
## Installing Java to run the Minecraft Server (Instructions from Here): ##
In order to actually run Minecraft, you will need a Java Runtime Environment. Although openjdk (open source java) is easily installable and apparently Minecraft will run under it, I have had great success with Sun’s (now Oracle’s) JRE and decided to go there straight away. You can download it from Java.com. I grabbed the Linux x64 version which is a gzipped tar archive. The archive was called jre-8u51-linux-x64.tar.gz.
 I put it in opt because opt is for optional common packages that aren't handled by the package manager. It required sudo before all the commands below though:

```bash
mv jre-7u5-linux-x64.tar.gz /opt/
cd /opt
tar zxvf jre-7u5-linux-x64.tar.gz
```

This ended up with the following in /opt. You can delete the *.tar.gz:
```bash
drwxr-xr-x  3 root root     4096 Jun 13 20:11 .
drwxr-xr-x 23 root root     4096 Jun 13 20:06 ..
drwxr-xr-x  6 uucp  143     4096 May 15 17:38 jre1.7.0_05
-rw-r--r--  1 root root 32876408 Jun 13 20:09 jre-7u5-linux-x64.tar.gz
```

I added a symbolic link to make things easier in the future. I then also added a symbolic link for the java binary itself. The below makes it so that java actually calls jreBlahBlah.
```bash
cd /opt
ln -s jre1.7.0_05 java
ln -s /opt/java/bin/java /usr/bin/java
```

## Installing CanaryMod Minecraft Server: ##
Downloaded the canarymod server from here because it was recommended by the scriptcraft book: http://scriptcraftjs.org/download/latest/. I put it into a folder called mcserver. Rename the canarymod to be called "canarymod.jar".

Create a bash script, in the same mcserver dir, that contains the below (the 1024 is telling java to use ONLY 1 GB of memory for the process). Mine was called canarymod.sh:
```bash
#~/bin/sh
BINDIR=$(dirname "$readlink -fn "$0")")
cd "$BINDIR"
java -Xmx1024 -jar canarymod.jar -o true
```

## Running canarymod Server (use ./canarymod.sh): ##
The first time you run canarymod.sh (using `./canarymod.sh` because it's not in your PATH), it will create a bunch of default directorys, config files, and databases, then abort. Open the end-user license agreement and change the eula from false to true to accept it. Run it again and it will actually create the words, the remaining configuration items, and actually run the server.

## Server and World Configuration - Basic Configuration Steps ##
[Actual Outline of All Server Properties](http://minecraft.gamepedia.com/Server.properties)

The server is configured using the server.cfg file that's within the config dir.

server.cfg contians the settings that affect the game in general.

## Configuration Files ##
### server.cfd ###
- announce Player Achievements - If all players are informed of reached achievements?
- allow-enchantment-Stacking - Allows stacking of enchanted objects to not enchanted items
- Ban-default-message - a message to be shown an exiled players to enter the server while trying
- ban-expiration-date-message - the message displayed when a player tries to enter temporarily rapt the server (the date of repeal is appended)
- chat format - should Specifies how the in-game chat look. All common colors are used. Instead of § character can also be a & used. Placeholders are as follows (and can be extended by plugins):% prefix (player name prefix),% name (player name),% Group (player main group)
- Command Block Enabled - Determines whether the command block may be used
- Command-block-group - Defines the rights group is applied to the command block (works like players)
- Command-block-op - Determines whether the command block to be treated as a Server Operator (if true, the rights of the specified group ignored)
- data-source - The location to use data source. CanaryMod provides the following available: XML, MySQL, SQLite (Can be expanded externally)
- date-format display formatting rule to timestamp -
- death-messages - Specifies whether notifications are displayed on the death of a player
- default-World-name - the name of the world that is to be loaded by default with the Server Start
- Logger-level - Sets the log level (which appears in the server log). Values ​​are: OFF FATAL ERROR WARN INFO DEBUG TRACE ALL
- Max players - maximum number of players on the server (without the reserve list size)
- motd - message of the day is displayed in the Minecraft Server Browser
- online-mode - Specifies whether users should be authenticated (often leads to misbehavior with the rights groups as the UUID of a user can not be detected correctly)
- player-idle-timeout - Time until an absent player automatically thrown in minutes from Server
- playerlist-Enabled - Specifies whether information is to be sent over existing players
- playerlist AutoUpdate - Specifies whether the player list will be renewed automatically or not
- playerlist-usecolors - Determines whether prefixes and colors should also be sent to players list
- playerlist-ticks - The time between solid rosters updates in ticks
- Query Enabled - Determines whether information can be obtained through this server from the outside
- query port - to the port on the run the above requests
- RCON enabled - is allowed Specifies whether remote access
- RCON Port - The port is run on the remote access
- RCON Password - Access password
- reserve list-Enabled - Determines whether the reserve list to be used
- reserve list-message - message displayed to users are not on the reserve list
- Server full-message - message displays the users if the server is full
- Server Port - Specifies to run the port at about the CanaryMod Traffic
- Server locale - Sets the default server language. Available languages ​​check out languages.txt in long / folder
- show-unknown-command - Specifies whether players should be kept informed about unknown commands
- snooper-Enabled - Determines whether Mojang can determine information about the server hardware and software
- Spam Protection - Defines the levels of spam protection firmly. Values: default - applies to all except players with "ignorerestrictions" Permission; off - No protection; all - applies to all
- Strict sign-characters - whether you want to sign on signs checked Sets for validity.
- texture-pack - Recommended Resource packs from the server
- view-distance - Sets the radius around a players around fixed, to be loaded into the chunks (3-15)
- Whitelist Enabled - Determines whether the whitelist will be used
- whitelist message - not be the message to the player on the whitelist
- World-cache-enabled timer - Specifies whether empty worlds to be automatically unloaded from memory
- World-cache-timeout - Specify how long a world must be empty before it is unloaded

### db.cfg ###
If you are using MySQL as a data source, you can make in this file settings. The previously defined default values ​​should be optimal for most server with up to 100 players. The following entries must be adjusted in order CanaryMod can establish a Verbingung to MySQL database.

- Name - The name of the database verwendenen
- host - the name of the MySQL hosts
- username - The MySQL Users
- Password - The MySQL password
- Port - The port to be on the Connected

### Worlds Configurations ###
For each created world there is such a configuration. It contains the following items:

- World-name - The name of the world (composed of the base name and the dimension)
- World-type - The World Type
- Spawn protection - radius in blocks around the Spawn around where nobody is allowed to build something (exceptions are ops and user with the "ignorerestrictions" Permission)
- max-build-height - maximum height
- generate-Structures - Should structures be created
- Generator settings - settings for the FLAT World Type
- World-seed - The worlds-seed (a number)
- startup autoload - Determines if this world is to be loaded automatically when the server starts
- Warp autoLoad - Determines if this world is to be loaded automatically they ,, eg. is requested by a plug-in or a warp command
- allow-nether - Determines whether a Nether dimension must be created
- allow-end - Specifies whether an end dimension may be created
- allow-flight - Allows fly from players by example Client Mods
- pvp - Specifies whether players can harm each other
- difficulty - Sets the difficulty
- Game Mode - Sets the game mode for this world
- forcedefault Game Mode - Specifies whether players regardless of game mode setting can also be in other game modes (enter When Server)
- forceDefaultGameModeDimensional - Specifies whether players regardless of game mode setting can also be in other game modes (when entering another dimension)
- auto-heal - Sets of players to be healed automatically
- enable-experience - Determines whether experience points can be collected
- Enable Health - Specifies whether players can ever become damaged
- Spawn Villagers - Determines whether villagers may spawn
- Golems spawn - may Determines whether Golems spawn in villages
- spawn-animals - Determines whether animals may spawn
- spawn-monsters - Determines whether monsters may spawn
- Natural animals - A comma-separated list of the animals may spawn
- Natural monsters - A comma-separated list of monsters may spawn the
- Natural golems A comma-separated list of the golems may spawn
- Natural water animals - A comma-separated list of Aquatic animals may spawn the
- Natural spawn rate - can spawn The frequency in the new animals and monsters
- Ender blocks - A list of blocks, the final men may not absorb
- disallowed block - A list of blocks that no one may take up

## Installing ScriptCraft Plugin: ##
While in the mcserver/plugins dir, wget the scriptcraft.jar from the book's site. When you run the server the next time it will extract and enable the scriptcraft plugin.

## CanaryMod and Permissions ##
CanaryMod works slightly differently to CraftBukkit in how it handles permissions and groups. By default, there are 4 groups: visitors, players, mods and admins. Each player who joins the game is added to the 'visitors' group. This group has no permissions by default so visitors can't break or place blocks. I ran the command below to get it so that people could modify blocks: ```groupmod permission add visitors canary.world.build```

If you would like all admins to have scripting ability then issue the following command. I haven't ran this yet:
```groupmod permission add admins scriptcraft.evaluate```

This will enable all admins on your server to issue javascript statements. All operators can issue any command (including the /js command to evaluate javascript). 

## Deleting and Remaking Worlds ##
- If you just plan on having one world, you can change the settings under the /config/worlds/ dir. Then delete the other ./worlds dir that's in the server's top level dir. The next time the server is started up, it use the /config/worlds world configuration/settings to create a new world. 
- The other options is to create another world configuration in the /configs/world/ dir with a completely new world-name, then in the server.cfg, you tell the server to load the new world instead. The server.cfg line that has to change is either default-world-name or level-name. I haven't gotten this to work yet, but it would be cool to be able to bounce around.

## Writing Code and Refreshing server: ##
Simply add your *.js files to the /scriptcraft/plugins folder, making sure to `exports.FUNCTIONNAME` the appropriate code, then:
Run `/jsrefresh()` after modifying the js files and all the modules will be reloaded so you can use your code in the game. More importantly, everything in this plugins folder is run at server startup.

/scripcraft/modules is just like the node.js node_modules folder that stores all your dependencies. Its not run automatically or anything by the server, its for modules you write and then require() by code that's actually run in the plugins folder.
