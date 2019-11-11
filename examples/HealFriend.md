```
local friendName = "asd"
macro(100, "heal friend", function()
    local friend = getPlayerByName(friendName)
    if friend and friend:getHealthPercent() < 90 then
        say("exura sio \"" .. friendName)
        delay(1000)
    end
end)
```