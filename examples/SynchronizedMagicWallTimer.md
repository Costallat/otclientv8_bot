```
-- sync mwall
-- tested on 1099
local secondPlayer = "Player1"
if name() == "Player1" then
  secondPlayer = "Player2"
end
info("Syncing mwalls with player: " .. secondPlayer)

onAddThing(function(tile, thing)
  if thing:isItem() and thing:getId() == 2129 then
    local pos = tile:getPosition().x .. "," .. tile:getPosition().y .. "," .. tile:getPosition().z
    if not storage[pos] or storage[pos] < now then 
      storage[pos] = now + 20000
      talkPrivate(5, secondPlayer, encode({type = "mwall", pos = pos, ping = ping()}))
    end
    tile:setTimer(storage[pos] - now)
  end
end)

listen(secondPlayer, function(text) 
  local data = decode(text)
  if data and data.type == "mwall" then
    storage[data.pos] = now + 20000 - data.ping - ping()
  end
end)
```