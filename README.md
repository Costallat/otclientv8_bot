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
```

## Main bot functions

```
Description will be added later, by now if you want to know how they work check game_bot/executor.lua

info(text)
warn(text)
error(text)

setupUI(text)
setupUI(text, parent)
addLabel(id, text)
addSwitch(id, text, onClickCallback)
addButton(id, text, onClickCallback)
addSeparator(id)

macro(timeout, callback)
macro(timeout, name, callback)
macro(timeout, name, hotkey, callback) -- hotkey is used to turn on/off macro

hotkey(keys, callback)
hotkey(keys, name, callback)

singlehotkey(keys, callback) -- works like hotkey, but will be executed only once after key press
singlehotkey(keys, name, callback)

schedule(timeout, callback)

onKeyDown(callback) -- callback = function(keys)
onKeyPress(callback) -- callback = function(keys)
onKeyUp(callback) -- callback = function(keys)
onTalk(callback) -- callback = function(name, level, mode, text, channelId, pos)
onAddThing(callback) -- callback = function(tile, thing)
onRemoveThing(callback) -- callback = function(tile, thing)

listen(name, callback) -- callback = function(text, channelId, pos)

delay(duration) -- can be only used inside callback function, blocks execution of current macro/hotkey/callback for x milliseconds
```

## Other functions and shortcuts (from game_bot/functions.lua)
```
-- player releated
name = function() return player:getName() end

hp = function() return player:getHealth() end
mana = function() return player:getMana() end
hppercent = function() return player:getHealthPercent() end
manapercent = function() return player:getManaPercent() end
maxhp = function() return player:getMaxHealth() end
maxmana = function() return player:getMaxMana() end
hpmax = function() return player:getMaxHealth() end
manamax = function() return player:getMaxMana() end

cap = function() return player:getCapacity() end
freecap = function() return player:getFreeCapacity() end
maxcap = function() return player:getTotalCapacity() end
capmax = function() return player:getTotalCapacity() end

exp = function() return player:getExperience() end
lvl = function() return player:getLevel() end
level = function() return player:getLevel() end

mlev = function() return player:getMagicLevel() end
magic = function() return player:getMagicLevel() end
mlevel = function() return player:getMagicLevel() end

soul = function() return player:getSoul() end
stamina = function() return player:getStamina() end
voc = function() return player:getVocation() end
vocation = function() return player:getVocation() end

bless = function() return player:getBlessings() end
blesses = function() return player:getBlessings() end
blessings = function() return player:getBlessings() end


pos = function() return player:getPosition() end
posx = function() return player:getPosition().x end
posy = function() return player:getPosition().y end
posz = function() return player:getPosition().z end

direction = function() return player:getDirection() end
speed = function() return player:getSpeed() end
skull = function() return player:getSkull() end
outfit = function() return player:getOutfit() end


autoWalk = function(destination) return player:autoWalk(destination) end
walk = function(dir) return modules.game_walking.walk(dir) end

-- game releated
say = g_game.talk
talk = g_game.talk
talkPrivate = g_game.talkPrivate
sayPrivate = g_game.talkPrivate
use = g_game.useInventoryItem
usewith = g_game.useInventoryItemWith
useWith = g_game.useInventoryItemWith
findItem = g_game.findItemInContainers

attack = g_game.attack
cancelAttack = g_game.cancelAttack
follow = g_game.follow
cancelFollow = g_game.cancelFollow
cancelAttackAndFollow = g_game.cancelAttackAndFollow

logout = g_game.forceLogout
ping = g_game.getPing

-- map releated
zoomIn = function() modules.game_interface.getMapPanel():zoomIn() end
zoomOut = function() modules.game_interface.getMapPanel():zoomOut() end

-- tools
encode = function(data) return json.encode(data) end
decode = function(text) local status, result = pcall(function() return json.decode(text) end) if status then return result end return {} end
```

## Example script, check `examples` for more
```
--#Example config

--#main
local widget = setupUI([[
Panel
  id: redPanel
  background: red
  margin-top: 10
  margin-bottom: 10
  height: 100
  
  Label
    anchors.fill: parent
    text: custom ui, otml based
    text-align: center
]])

--#macros
macro(5000, "macro send link", "f5", function()
  g_game.talk("macro test - https://github.com/OTCv8/otclient_bot")
  g_game.talk("bot is hiding 50% of effects as example, say exevo gran mas vis")
end)

macro(1000, "flag tiles", function()
  tile:setText("Hello =)", "red")
end)

macro(25, "auto healing", function()
  if hppercent() < 80 then
    say("exura")
    delay(1000) -- not calling this macro for next 1s
  end
end)

addSeparator("spe0")

--#hotkeys
hotkey('y', 'test hotkey', function() g_game.talk('hotkey elo') end)
singlehotkey('x', 'single hotkey', function() g_game.talk('single hotkey') end)

singlehotkey('=', "Zoom in map", function () zoomIn() end)
singlehotkey('-', "Zoom out map", function () zoomOut() end)

--#callbacks
onAddThing(function(tile, thing)
  if thing:isItem() and thing:getId() == 2129 then
    local pos = tile:getPosition().x .. "," .. tile:getPosition().y .. "," .. tile:getPosition().z
    if not storage[pos] or storage[pos] < now then 
      storage[pos] = now + 20000
    end
    tile:setTimer(storage[pos] - now)
  end
end)

-- hide 50% of effects
onAddThing(function(tile, thing)
  if thing:isEffect() and math.random(1, 2) == 1 then
    thing:hide()
  end
end)

listen(player:getName(), function(text)
  info("you said: " .. text)
end)

--#other
addLabel("label1", "Test label 1")
addSeparator("sep1")
addLabel("label2", "Test label 2")

storage.clicks = 0
addButton("button1", "Click me", function()
  storage.clicks = storage.clicks + 1
  ui.button1:setText("Clicks: " .. storage.clicks)
end)

HTTP.getJSON("https://api.ipify.org/?format=json", function(data, err)
    if err then
        warn("Whoops! Error occured: " .. err)
        return
    end
    info("HTTP: My IP is: " .. tostring(data['ip']))
end)
```