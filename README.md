# OTClientV8 BOT

## In progress, first, preview version

### It's open source so you can check how it works on: https://github.com/OTCv8/otclientv8/tree/master/modules/game_bot

## Main bot variables
```
now - time in milliseconds since otclient start
time - same as now
player - just LocalPlayer, for eg. player:getHealth()
storage - table which keeps it values even after otclient restart
panel - main panel of bot widget
ui - table with references to UI modules
```

## Main bot functions

```
Description will be added later, by now if you want to know how they works check game_bot/executor.lua

info(text)
warn(text)
error(text)

addLabel(id, text)
addSwitch(id, text, onClickCallback)
addButton(id, text, onClickCallback)
addSeparator(id)

macro(timeout, callback)
macro(timeout, name, callback)
macro(timeout, name, hotkey, callback)

hotkey(keys, callback)
hotkey(keys, name, callback)

schedule(timeout, callback)

```

## Other functions and shortcuts
```
-- basic functions
print = print
pairs = pairs
ipairs = ipairs
tostring = tostring
math = math
table = table
string = string
tr = tr
json = json
regexMatch = regexMatch

-- game functions
say = g_game.talk
talk = g_game.talk
talkPrivate = talkPrivate
sayPrivate = talkPrivate
use = g_game.useInventoryItem
usewith = g_game.useInventoryItemWith
useWith = g_game.useInventoryItemWith
findItem = g_game.findItemInContainers

-- classes
g_game = g_game
g_map = g_map
StaticText = StaticText
HTTP = HTTP
```

## Example script
```
--#macros
macro(5000, "macro send link", "f5", function()
  g_game.talk("macro test - https://github.com/OTCv8/otclient_bot")
end)

macro(1000, "flag tiles", function()
  local staticText = StaticText.create()
  staticText:addMessage("t", 9, "xDDD")
  local tile = player:getTile()
  tile:clearTexts()
  tile:addText(staticText)
  for i = 1, 10 do 
    schedule(1000 * i, function()
      staticText:setText(i)
    end)
  end
  schedule(11000, function()
    tile:clearTexts()
  end)
end)


addSeparator("spe0")

--#hotkeys
hotkey('y', 'test hotkey', function() g_game.talk('hotkey elo') end)

--#callbacks

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

info("Bot started")
```