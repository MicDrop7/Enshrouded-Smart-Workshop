SMART WORKSHOP - automated production stations for Enshrouded
==============================================================
Smart Workshop by MicDrop7
Version 1.1.0 (tested on game build 26430625 / dedicated server build 26430253)

What's new in 1.1.0
  * "Automation" and "Dry run" are now one setting, Automation: Off / Log only / On (ini: automation = off/log/on).
    Your existing enabled / dryRun lines keep working; the menu replaces them when you change Automation there.
  * Fixed: the F6 menu no longer says "This server does not run Smart Workshop" in a solo world.

Smart Workshop keeps the production stations of a base (kilns, smelters, looms, grinders, seedbeds ...) supplied
and emptied, using the base's storage chests:

  * Refill    - tops up a station's ingredients from the chests of the same base (the game's own
                "fill from chests"), up to a number of crafts you choose.
  * Collect   - moves finished items from the station straight into a chest of the same base:
                the chest you assigned -> a chest that already holds that item -> your fallback chest.
                If no chest has room, the items stay in the station.
  * Targets   - stops a station once the base holds enough of its product, and starts it again when the
                stock falls below the target. Stations you stopped yourself are left alone.
  * Reserves  - never takes an ingredient below an amount you choose.

Everything goes through the game's own systems (the same code as the "fill from chests" button, the stop button
and moving items in the inventory), so other players see the changes normally. Recipes, ingredient categories and
stack sizes are read from the running game, so no data files are needed.

Settings can be changed in game (press F6) or in smart-workshop.ini.

IMPORTANT
  * It is OFF after installing (Automation: Off). Set Automation to "Log only" first: it then only writes what
    it WOULD do to the log. Set it to "On" (in the F6 menu or the ini) when you are happy with the plan.
  * It only works on stations while a player is inside that station's base (like the game's own
    "fill from chests"). Stations only produce while their area is loaded anyway.
  * It never touches another base's chests or stations.
  * Back up your world before you try it for the first time.


INSTALL - single player, or hosting a game for friends from your own PC
-----------------------------------------------------------------------
Requires Ultimate ASI Loader (dinput8.dll in the Enshrouded folder; many mods already install it):
  https://github.com/ThirteenAG/Ultimate-ASI-Loader/releases

  1. Copy the "Scripts" folder from SmartWorkshop-1.1.0.zip into your Enshrouded folder
     (e.g. ...\steamapps\common\Enshrouded\). You get:
        Enshrouded\Scripts\SmartWorkshop.asi
        Enshrouded\Scripts\SmartWorkshop\smart-workshop.ini
        Enshrouded\Scripts\SmartWorkshop\README.txt
  2. Start the game, load your world, walk into your base and wait about 30 seconds.
  3. Press F6 to open the settings menu (see THE IN-GAME MENU), or open
     Enshrouded\Scripts\SmartWorkshop\smart-workshop.log to see your stations and what it would do.


INSTALL - dedicated server
--------------------------
The server needs the plugin (it does the work). Players only need it to use the F6 menu on that server.
Use the server download (SmartWorkshop-server-1.1.0.zip). It contains one folder:
     SmartWorkshop\SmartWorkshop.dll
     SmartWorkshop\smart-workshop.ini
     SmartWorkshop\README.txt
     SmartWorkshop\mod.json   (keep it: the Enshrouded Mod Loader (EML) will not start the server if a
                               folder in "mods" has no mod.json. It contains no mod code.)

  1. Upload the whole SmartWorkshop folder into the server's "mods" folder:
        <server>\mods\SmartWorkshop\
  2. The DLL then has to be loaded by the server:
       * BisectHosting: see INSTALL - BisectHosting below.
       * Your own Windows server: install Ultimate ASI Loader (dinput8.dll) next to enshrouded_server.exe and
         copy SmartWorkshop.dll into <server>\Scripts\ renamed to SmartWorkshop.asi. Leave the ini in
         mods\SmartWorkshop\ - Smart Workshop finds it there.
       * Other hosts: use the host's DLL loader as described by the host.
  3. Restart the server. smart-workshop.log appears in mods\SmartWorkshop\, next to smart-workshop.ini.
  4. Players who want to change the server's settings in game install the normal download (see above) on
     their own PC. Players without it are not affected.


INSTALL - BisectHosting
-----------------------
  1. Upload the SmartWorkshop folder into the server's "mods" folder (as above).
  2. MOVE SmartWorkshop.dll from mods\SmartWorkshop\ into the server's DLLMods folder.
     smart-workshop.ini and mod.json stay in mods\SmartWorkshop\.
  3. Open chainloader.ini in the DLLMods folder and add this line to the end of the [Load] section:
        SmartWorkshop.dll
  4. Restart the server.
  To switch Smart Workshop off again: remove that line from chainloader.ini (or delete DLLMods\SmartWorkshop.dll)
  and restart.


THE IN-GAME MENU (F6)
---------------------
F6 opens Smart Workshop's settings in the game's own menu style. The key can be changed with menuKey in the
[global] section of smart-workshop.ini.

  * Tabs along the top: Global (all bases), then one tab per base, named after its flame altar.
    Bases appear about 30 seconds after a player first walks into them.
  * Each base tab lists the base's settings and its stations, named as in the game with what they make
    (e.g. "Kiln - Fired Bricks"). Open a station to give it its own settings and an output chest.
  * Inherit: a base or station setting left on "Inherit" uses the setting one level up, shown in brackets.
  * Changes are saved to smart-workshop.ini straight away.

  Keys:   Up / Down          select            Left / Right       change the value
          Enter or Space     open a station    Backspace or Esc   back to the base
          Q / E              switch tab        Esc (on a tab)     close        F6   open / close
  Mouse:  wheel or scroll bar to scroll, click a station to open it, right-click to go back,
          click the arrows or drag the slider to change a value.

ON A DEDICATED SERVER
  When you join a server that runs Smart Workshop, the menu shows the SERVER's bases and settings, and your
  changes are sent to the server. Who may change them depends on the server password the player joined with:
    * Admin password      - may change everything, including "Who can change settings".
    * Other passwords     - may change settings only while "Who can change settings" is set to Everyone,
                            and never that option itself. Settings they may not change are greyed out.
  "Admin" means a user group in enshrouded_server.json with "canKickBan": true (the game's normal admin group).
  On a server without Smart Workshop the menu only shows a notice; it never sends anything to such a server.


SETTINGS (smart-workshop.ini: Scripts\SmartWorkshop\ for players, mods\SmartWorkshop\ on servers)
---------------------------------------------------------------------------------------------------
Everything in the menu can also be set in the ini. Three levels, the most specific wins:
  [global]                    all stations
  [base <base id>]            one base - the base id is shown in the log ("base 0006ffff3e017e73: 38 station(s) ...")
  [base <altar name>]         the same, using the name you gave the base's flame altar, e.g. [base Main Base]
                              (if both exist for one base, the id section wins; renaming the altar breaks the link)
  [station <station key>]     one station - the key is its position, shown in the log ("st@5417.1,1124.0,2452.2")

  automation = off/log/on        off: nothing; log: only write to the log what it would do, nothing is
                                 moved; on: refill and collect                    (default off)
                                 (the older keys enabled / dryRun from 1.0.x still work)
  maxCraftsLoaded = 1..1000      ingredients for up to this many crafts           (default 10)
  collectOutputs = true/false    move finished items into chests                  (default true)
  outputChest = <chest key>      (station) preferred chest for its output
  useFallbackChest = true/false  use the fallback chest when nothing else fits    (default false)
  fallbackChest = <chest key>    the fallback chest
  target.<item id> = <number>    stop at this base stock, resume below it
  reserve.<item id> = <number>   keep at least this many of an ingredient in the chests
  ([global] only)
  menuKey = F6                   key that opens the in-game menu
  menuEditors = admins/everyone  on a dedicated server: who may change settings in game (default admins)

  <item id> = the game's numeric item id, as shown in the plan in the log ("item=372029138 ...").
  Chest keys look like ch@5424.5,1127.0,2479.0 (shown in the log). Targets, reserves and the fallback chest
  are set in the ini only.

Example:
  [global]
  automation = on
  # charcoal: stop at 500
  target.372029138 = 500
  # wood logs: always keep 200 in the chests
  reserve.722268629 = 200

  [station st@5417.1,1124.0,2452.2]
  maxCraftsLoaded = 20

Station and chest keys are their positions: if you pick a station up and place it again, it gets a new key.
The ini is re-read within 30 seconds when you edit it by hand.

YOUR BASES ARE ADDED TO THE INI AUTOMATICALLY
  A minute or two after a player first walks into a base, Smart Workshop adds a block for that base to the end of
  smart-workshop.ini: its id, the name of its flame altar, roughly where it is, and a list of its stations with
  what each one makes. Everything in it is commented out, so it changes nothing until you remove a # in front of
  a line (or change the base in the F6 menu). Each base is added only once - if you delete its block, leave the
  "[base <id>]" line (with a # in front) so it is not added again. If you rename a base's flame altar, the name in
  the block's first line is updated automatically.


THE LOG (smart-workshop.log next to smart-workshop.ini; the last 5 sessions are kept as .1.log ... .5.log)
------------------------------------------------------------------------------------------------------------
  cycle: ...           every 30 s when something changes: stations found, bases, planned actions
  --- plan ---         the stations that take part and what Smart Workshop plans for them
  load / collected / stopped / resumed     what it actually did
  menu: / remote:      the in-game menu and, on servers, settings changed by players (and refused requests)


COMPATIBILITY
-------------
  Works alongside Survivor Crafting Anywhere, Quick Stack Area and Creative Mode, and with mods loaded through other
  loaders that hook the same game functions.


KNOWN LIMITS
------------
  * Recipes with the station's "use all ingredient options" switched on, and ingredient categories where
    some items count more than once, are skipped (the plan says so).
  * Water-based recipes are planned like any other; water itself is not supplied.
  * Esc on a station page goes back to the base, but the menu briefly closes and opens again.


UNINSTALL
---------
Players: delete Scripts\SmartWorkshop.asi and the Scripts\SmartWorkshop folder (settings, logs, README).
Servers: delete mods\SmartWorkshop (and on BisectHosting DLLMods\SmartWorkshop.dll plus its chainloader.ini line).
Stations Smart Workshop stopped at a target stay stopped until you start them (Scripts\SmartWorkshop\state.txt
only remembers which ones). On a dedicated server the menu's data is sent to players as hidden name texts that
are never shown in the world; the game may keep them in the world save (a few KB, replaced, not added to).
