csgo-pug-setup
===========================

This is a useful plugin for managing pug games or 10 mans. It allows a player to type .setup into chat and select (from a menu):
- how to choose the teams (players do it manually, random teams, captains select teams)
- how to choose the map (use the current map, do a map vote using maps from addons/sourcemod/configs/pugselect/maps.txt)

The goal is to allow a lightweight, easy-to-use setup system that automates as much as possible with as few dependencies as possible. However,
the goal isn't fully automated - it assumes the players know each other or there is an admin. There is no mechanism for kicking players or anything similar.

### Download
You should be able to get the most recent **pugsetup.zip** file from https://github.com/splewis/csgo-pug-setup/releases.

### Installation
Download pugsetup.zip and extract the files to the game server. You should have installed:
- csgo/addons/sourcemod/configs/maps.txt **(you might want to edit this)**
- csgo/addons/sourcemod/plugins/pugsetup.smx
- csgo/cfg/sourcemod/pugsetup/warmup.cfg **(you might want to edit this)**
- csgo/cfg/sourcemod/pugsetup/standard.cfg **(you might want to edit this)**

Note that the maps.txt file will be created automatically if it doesn't exist and the names of the cfg files executed can be changed by convars. For example, you could set sm_pugsetup_live_cfg to cevo.cfg to run the CEVO config instead of the one I provide. See the ConVars section for details.

### Usage
There is a notion of the the pug/game "leader". This is the player that writes .setup first and goes through the setup menu. The leader has elevated permissions and can use some extra commands (e.g. pause). To prevent some abuse there is also an admin command sm_leader to manually change the leader.

Generally, here is what happens:
- A player joins and types .setup and goes through the menu to select how the teams and map will be chosen
- Once 10 (this number if configurable) players join and all type .ready, the next stage starts
- If the leader setup for a map vote, the map vote will occur and the map will change, then all players will type .ready on the new map
- If the leader setup for a captain-style team selection, the game will wait for when 2 captains are selected, then the captains will be given menus to chose players
- Then, either by the leader typing .start or the game auto-living (which is also configurable), the game will initiate a live-on-3 restart and go

### Commands

Some commands that are important are:
- **.setup**, begins the setup phase and sets the pug leader
- **.start**, begins the game (note that the cvar sm_teamselect_autolo3 controls if this is needed
- **.ready**
- **.unready**
- **.pause**
- **.unpause**
- **.capt** gives the pug leader a menu to select captains
- .rand selects random captains
- .leader gives a menu to change the game leader
- .endgame, force ends the game safely (only the leader can do this, note that this **resets the leader** to nobody)

The chat commands are mostly aliases for sourcemod admin commands, so an admin can override things if needed. The bold commands are only available through these admin commands and have no chat aliases (other than the default sourcemod ones, e.g. !leader or /leader go with sm_leader)

These use admin flag "g" for map change abilities:
- sm_setup
- sm_leader
- sm_start
- sm_rand
- sm_capt
- sm_endgame (note this resets the leader to none)

These use the generic admin flag "b":
- sm_pause
- sm_unpause

Generally you don't need the admin (sm_) commands, but they may come in helpful if a captain/leader doesn't know about the .pause feature or
you need to take leardership of the pug.

### Convars
These are put in an autogenerated file at **cfg/sourcemod/pugsetup.cfg**, once you start the plugin go edit that file if you wish.
- **sm_pugsetup_warmup_cfg** should store where the warmup config goes, defaults to the included file **sourcemod/pugsetup/warmup.cfg**)
- **sm_pugsetup_live_cfg** should store where the warmup config goes, defaults to the included file **sourcemod/pugsetup/standard.cfg**
- **sm_pugsetup_autolo3** controls if a .start (or !start) is needed for going live in a 10man/random team game
- **sm_pugsetup_numplayers** controls the minimum number of players to go live
- **sm_pugsetup_autorecord** controls if the plugin should autorecord a gotv demo (you may need to add some extra cvars to your cfgs, such as tv_enable 1)
- **sm_pugsetup_savemoney** controls if the plugin should save player money on disconnects to restore it if they rejoin
- **sm_pugsetup_requireadmin** controls if an admin flag ("g" for change map permissions) is needed to use .setup
