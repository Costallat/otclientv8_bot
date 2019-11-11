```
onAddThing(function(tile, thing)
  if thing:isItem() and not thing:isGround() then
    thing:hide()
  end
end)
```