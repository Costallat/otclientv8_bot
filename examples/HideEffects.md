onAddThing(function(tile, thing)
  if thing:isEffect() then
    thing:hide()
  end
end)