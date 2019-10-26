-- Tested on version 1099
onAddThing(function(tile, thing)
  if thing:isItem() and thing:getId() == 2129 then
    local pos = tile:getPosition().x .. "," .. tile:getPosition().y .. "," .. tile:getPosition().z
    if not storage[pos] or storage[pos] < now then 
      storage[pos] = now + 20000
    end
    tile:setTimer(storage[pos] - now)
  end
end)