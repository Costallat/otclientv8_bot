```
storage.mwalls = {}

local channel = "uniquenameofchannel"
BotServer.init(name(), channel)
BotServer.listen("mwall", function(name, message)
  if not storage.mwalls[message["pos"]] then
    storage.mwalls[message["pos"]] = now + message["duration"] - 150 -- 150 is latency correction
  end
end)

onAddThing(function(tile, thing)
  if thing:isItem() and thing:getId() == 2129 then
    local pos = tile:getPosition().x .. "," .. tile:getPosition().y .. "," .. tile:getPosition().z
    if not storage.mwalls[pos] or storage.mwalls[pos] < now then 
      storage.mwalls[pos] = now + 20000
      BotServer.send("mwall", {pos=pos, duration=20000})
    end
    tile:setTimer(storage.mwalls[pos] - now)
  end
end)
```