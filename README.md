# Smart Workshop

**Version 1.1.0** · Ultimate ASI Loader plugin (players) / DLL (dedicated servers) ·
[Releases](https://github.com/MicDrop7/Enshrouded-Smart-Workshop/releases/tag/v1.1.0)

| Download | For |
|---|---|
| [SmartWorkshop-1.1.0.zip](SmartWorkshop-1.1.0.zip) | **Players**: single player, hosting from your PC, and using the F6 menu on a server |
| [SmartWorkshop-server-1.1.0.zip](SmartWorkshop-server-1.1.0.zip) | **Dedicated servers** |

Older: [1.0.0](SmartWorkshop-1.0.0.zip) / [1.0.0 server](SmartWorkshop-server-1.0.0.zip)

**Pre-release 1.2.0 (testing):** optional support for [Kiln](https://github.com/frostdev-ops/kiln) by Frostdev on
dedicated servers. [Release notes](https://github.com/MicDrop7/Enshrouded-Smart-Workshop/releases/tag/v1.2.0) ·
[players](SmartWorkshop-1.2.0.zip) / [server](SmartWorkshop-server-1.2.0.zip) /
[server with Kiln](SmartWorkshop-server-kiln-1.2.0.zip). 1.1.0 above stays the stable version until 1.2.0 is tested.

**New in 1.1.0:** Automation and Dry run are one setting, **Automation: Off / Log only / On** (ini:
`automation = off | log | on`; old `enabled` / `dryRun` lines keep working). Fixed: in a solo world the F6 menu said
"This server does not run Smart Workshop".

## What it does

Smart Workshop keeps the production stations of your bases running: kilns, smelters, blast furnaces, looms, grinders,
seedbeds, drying racks, butter churns and the rest. No more walking from station to station to top up ingredients and
empty out what they made.

- **Refill**: tops up each station's ingredients from the chests of the same base, using the game's own "fill from
  chests", up to the number of crafts you choose.
- **Collect**: moves finished items out of the station into a chest of the same base: the chest you picked for that
  station, else a chest that already holds that item, else your fallback chest.
- **Targets**: stops a station once the base holds enough of its product, and starts it again when the stock drops.
  Stations you stopped yourself are left alone.
- **Reserves**: never takes an ingredient below an amount you choose.

Everything goes through the game's own systems, so other players see the changes as normal. Recipes are read from the
running game, so no data files are needed.

**Settings in game.** Press **F6** for a settings menu in the game's own style: a tab for all bases, then one tab per
base (named after its flame altar), with every station listed by its game name and what it makes. Give a whole base or
a single station its own settings, or leave them on *Inherit*.

**On dedicated servers too.** The server runs Smart Workshop and does the work. Players with the mod installed can
open F6 on the server and change the server's settings, if the server allows them.

**Safe by default.** After installing, **Automation** is **Off**. Set it to **Log only** first: it then only writes to
its log what it *would* do, without moving anything. Set it to **On** when you are happy with the plan.

## Requirements

- **Ultimate ASI Loader** (x64) by ThirteenAG: <https://github.com/ThirteenAG/Ultimate-ASI-Loader/releases>.
  Its dll (dinput8.dll or winmm.dll) goes next to enshrouded.exe. If you already have a `Scripts` folder with other
  .asi mods, you have it.
- **Dedicated servers**: a way to load SmartWorkshop.dll: the Ultimate ASI Loader next to enshrouded_server.exe,
  BisectHosting's DLLMods chain loader, or your host's DLL loader.

## Installation

### Single player, or hosting from your own PC

1. Install the Ultimate ASI Loader if you do not have it yet.
2. Copy the `Scripts` folder from SmartWorkshop-1.1.0.zip into your Enshrouded folder (the one with enshrouded.exe):
   ```
   ...\Enshrouded\Scripts\SmartWorkshop.asi
   ...\Enshrouded\Scripts\SmartWorkshop\smart-workshop.ini
   ...\Enshrouded\Scripts\SmartWorkshop\README.txt
   ```
3. Start the game, load your world, walk into a base and wait about 30 seconds.
4. Press **F6** and set Automation to **Log only**. Check the plan in `Scripts\SmartWorkshop\smart-workshop.log`, then
   set it to **On** for the bases or stations you want.

### Dedicated server (your own Windows server)

1. Stop the server.
2. Upload the `SmartWorkshop` folder from SmartWorkshop-server-1.1.0.zip into the server's `mods` folder.
3. Install the Ultimate ASI Loader (dinput8.dll) next to enshrouded_server.exe, and copy SmartWorkshop.dll into
   `<server>\Scripts\` renamed to **SmartWorkshop.asi**. Leave smart-workshop.ini in `mods\SmartWorkshop`.
4. Start the server. `smart-workshop.log` appears in `mods\SmartWorkshop`.

### BisectHosting

1. Stop the server and upload the `SmartWorkshop` folder into the server's `mods` folder.
2. **Move** SmartWorkshop.dll from `mods\SmartWorkshop` into the server's `DLLMods` folder. smart-workshop.ini and
   mod.json stay in `mods\SmartWorkshop`.
3. Open `DLLMods\chainloader.ini` and add the line `SmartWorkshop.dll` at the end of the `[Load]` section.
4. Start the server.

**Players on a server**: install the normal download on your own PC to use the F6 menu there. It is optional: the
server does the work either way.

### Uninstalling

- Players: delete `Scripts\SmartWorkshop.asi` and the `Scripts\SmartWorkshop` folder.
- Servers: delete `mods\SmartWorkshop` and the DLL (on BisectHosting `DLLMods\SmartWorkshop.dll` and its
  chainloader.ini line).
- Stations Smart Workshop stopped at a stock target stay stopped until you start them.

## Controls (F6 menu)

| Key / mouse | Action |
|---|---|
| Up / Down | select |
| Left / Right | change the value |
| Enter or Space | open a station |
| Backspace, Esc or right-click | back to the base |
| Q / E | switch tab |
| Esc on a base tab / F6 | close |
| Mouse | wheel or scroll bar to scroll, click a station to open it, click the arrows or drag the slider |

## Settings (`smart-workshop.ini`)

Three levels, the most specific wins: `[global]` → `[base <id or altar name>]` → `[station <key>]`.

| Setting | Meaning |
|---|---|
| `automation` | `off`, `log` (only write what it would do to the log) or `on` (default off) |
| `maxCraftsLoaded` | ingredients for up to this many crafts, 1 to 1000 (default 10) |
| `collectOutputs` | move finished items into chests (default true) |
| `outputChest` | (station) the chest for this station's output |
| `useFallbackChest` / `fallbackChest` | where items go when nothing else fits |
| `target.<item id>` / `reserve.<item id>` | stock target and ingredient reserve |
| `menuKey` | the menu key (default F6) |
| `menuEditors` | who may change a server's settings in game: `admins` or `everyone` |

The full list, with examples, is in [README.txt](README.txt).

## Good to know

- **Back up your world** before you set Automation to On for the first time.
- Smart Workshop only works on a base while a player is inside it, like the game's own "fill from chests".
- It never uses another base's chests.
- Bases appear in the menu about 30 seconds after a player first walks into them.
- **Who can change a server's settings in game** depends on the password a player joined with:
  - **Admin password**: may change everything, including "Who can change settings".
  - **Other passwords**: may change settings only while "Who can change settings" is set to Everyone, and never that
    option itself. Settings they can't change are greyed out.
  - "Admin" means a user group in enshrouded_server.json with `"canKickBan": true`.
- On a server without Smart Workshop, the F6 menu only shows a notice and never sends anything.
- Not supported yet: recipes with "use all ingredient options" switched on, and ingredient categories where some items
  count more than once (the log says when a station is skipped).
- Esc on a station page goes back to the base, but the menu briefly closes and opens again.
- On a dedicated server, the menu's data reaches players as hidden name texts that are never shown in the world. The
  game may keep them in the world save (a few KB, replaced each time).
- Works alongside Survivor Crafting Anywhere, Quick Stack Area and Creative Mode.

## Credits

- ThirteenAG for the Ultimate ASI Loader.
- JudeCarpathian for the [Cliffside Base World Save](https://www.nexusmods.com/enshrouded/mods/290), which was used to
  test Smart Workshop.

---

More Enshrouded mods by MicDrop7: [Enshrouded-Game-Mods](https://github.com/MicDrop7/Enshrouded-Game-Mods).
