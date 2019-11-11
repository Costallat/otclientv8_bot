# OTClientV8 BOT

### Made and tested on [OTClientV8](https://github.com/OTCv8/otclientv8), however it is possible to port it to classic OTClient.
### Discord channel: https://discord.gg/feySup6

## Main bot variables
```
now - time in milliseconds since otclient start
time - same as now
player - just LocalPlayer, for eg. player:getHealth()
storage - table which keeps it values even after otclient restart
panel - main panel of bot widget
tabs - ui element, MoveableTabBar, bot tabs, is active when you add tab
```

## Main bot functions

```
Description will be added later, by now if you want to know how they work check game_bot/executor.lua

info(text)
warn(text)
error(text)

saveConfig()
addTab(name) -- returns tab panel, can be used as parent parameter for bellow functions

-- functions/ui.lua
setupUI(text[, parent])
addLabel(id, text[, parent])
addSwitch(id, text, onClickCallback[, parent])
addButton(id, text, onClickCallback[, parent])
addTextEdit(id, text, onTextChangeCallback[, parent])
addSeparator(id[, parent])

-- functions/main.lua
macro(timeout, callback)
macro(timeout, name, callback)
macro(timeout, name, hotkey, callback[, parent]) -- hotkey is used to turn on/off macro

hotkey(keys, callback)
hotkey(keys, name, callback[, parent])

singlehotkey(keys, callback) -- works like hotkey, but will be executed only once after key press
singlehotkey(keys, name, callback[, parent])

schedule(timeout, callback)

delay(duration) -- can be only used inside callback function, blocks execution of current macro/hotkey/callback for x milliseconds

-- functions/callbacks.lua
onKeyDown(callback) -- callback = function(keys)
onKeyPress(callback) -- callback = function(keys)
onKeyUp(callback) -- callback = function(keys)
onTalk(callback) -- callback = function(name, level, mode, text, channelId, pos)
onAddThing(callback) -- callback = function(tile, thing)
onRemoveThing(callback) -- callback = function(tile, thing)
onCreatureAppear(callback) -- callback = function(creature)
onCreatureDisappear(callback) -- callback = function(creature)
onCreaturePositionChange(callback) -- callback = function(creature, newPos, oldPos)
onCreatureHealthPercentChange(callback) -- callback = function(creature, healthPercent)
onUse(callback) -- callback = function(pos, itemId, stackPos, subType)
onUseWith(callback) -- callback = function(pos, itemId, target, subType)
onContainerOpen(callback) -- callback = function(container, previousContainer)
onContainerUpdateItem(callback) -- callback = function(container)
onContainerUpdateItem(callback) -- callback = function(container, slot, item)
onMissle(callback) -- callback = function(missle)
onChannelList(callback) -- callback = function(channels)
onOpenChannel(callback) -- callback = function(channelId, name)
onCloseChannel(callback) -- callback = function(channelId)
onChannelEvent(callback) -- callback = function(channelId, name, event)

listen(name, callback) -- callback = function(text, channelId, pos)
onPlayerPositionChange(callback) -- callback = function(newPos, oldPos)
onPlayerHealthChange(callback) -- callback = function(healthPercent)

```

## For other functions check folder `game_bot/functions/`. Second list of functions you can find in `lua` directory.

## This bot comes with plenty of ready to use UI elements called Panels. Check `game_bot/panels/` and first example script for list of them 

## Example script, check `examples` for more
```
--Default

--#main

local healTab = addTab("HP")
local attackTab = addTab("Atck")
local warTab = addTab("War")
local caveTab = addTab("Cave")

Panels.TradeMessage()

Panels.Haste(healTab)
Panels.ManaShield(healTab)
Panels.AntiParalyze(healTab)
Panels.Health(healTab)
Panels.Health(healTab)
Panels.HealthItem(healTab)
Panels.HealthItem(healTab)
Panels.ManaItem(healTab)
Panels.ManaItem(healTab)
Panels.Equip(healTab)
Panels.Equip(healTab)
Panels.Equip(healTab)
Panels.Eating(healTab)

Panels.AttackSpell(attackTab)
Panels.AttackItem(attackTab)

Panels.AttackLeaderTarget(warTab)
Panels.LimitFloor(warTab)
Panels.AntiPush(warTab)

local waypoints = Panels.Waypoints(caveTab)
local attacking = Panels.Attacking(caveTab)
local looting = Panels.Looting(caveTab) 
addButton("tutorial", "Help & Tutorials", function()
  g_platform.openUrl("https://github.com/OTCv8/otclientv8_bot")
end, caveTab)

--#macros

addSeparator("sep1")
local helloLabel = addLabel("helloLabel", "")

macro(1000, "example macro (time)", nil, function()
  helloLabel:setText("Time from start: " .. now)
end)

macro(1000, "this macro does nothing", "f7", function()

end)

macro(100, "debug pathfinding", nil, function()
  for i, tile in ipairs(g_map.getTiles(posz())) do
    tile:setText("")
  end
  local path = findEveryPath(pos(), 20, {
    ignoreNonPathable = false
  })
  local total = 0
  for i, p in pairs(path) do
    local s = i:split(",")
    local pos = {x=tonumber(s[1]), y=tonumber(s[2]), z=tonumber(s[3])}
    local tile = g_map.getTile(pos)
    if tile then
      tile:setText(p[2])
    end
     total = total + 1
  end
end)

macro(1000, "speed hack", nil, function()
  player:setSpeed(1000)
end)


--#hotkeys

hotkey("f5", "example hotkey", function()
  info("Wow, you clicked f5 hotkey")
end)

singlehotkey("ctrl+f6", "singlehotkey", function()
  info("Wow, you clicked f6 singlehotkey")
  usewith(268, player)
end)

--#callbacks

local positionLabel = addLabel("positionLabel", "")
onPlayerPositionChange(function()
  positionLabel:setText("Pos: " .. posx() .. "," .. posy() .. "," .. posz())
end)

--#other

macro(100, "hide useless tiles", "", function()
  for i, tile in ipairs(g_map.getTiles(posz())) do
    if not tile:isWalkable(true) then
      tile:setFill('black')
    end
  end
end)

addLabel("mapinfo", "You can use ctrl + plus and ctrl + minus to zoom in / zoom out map")

```