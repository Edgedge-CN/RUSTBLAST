[core]
tags:HorizontalUnitChild
#showInEditor:false
@memory listId:number



[hiddenAction_HorizontalUnitChildTeleportToHorizontalListChild]
autoTriggerOnEvent:newMessage(withTag="HorizontalUnitChildTeleportToHorizontalListChild")
setUnitMemory:listId=eventData("listId",type="number")
teleportTo:eventData("HorizontalListChild",type="unit")