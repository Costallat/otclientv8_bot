```
macro(25, "auto healing", function()
  if hppercent() < 80 then
    say("exura")
    delay(1000) -- not calling this macro again for next 1000ms
  end
end)
```