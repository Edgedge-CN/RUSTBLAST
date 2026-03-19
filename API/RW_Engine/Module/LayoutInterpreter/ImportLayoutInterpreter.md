[core]
@memory layout_marginTop:number
@memory layout_marginLeft:number

[hiddenAction_addToLayoutInterpreter]
autoTriggerOnEvent:created
resetCustomTimer:true
setCustomTarget2:self.customTarget1.readUnitMemory("LayoutInterpreter",type="unit")
sendMessageTo:self.customTarget2
sendMessageWithTags:addToLayoutInterpreter
sendMessageWithData:ViewId=self.resource.ViewId

[hiddenAction_setLayout]
autoTriggerOnEvent:newMessage(withTag="setLayout")

setUnitMemory:layout_marginTop=select(eventData("ViewInitType",type="string") == "marginTop",eventData("value",type="number"),memory.layout_marginTop),layout_marginLeft=select(eventData("ViewInitType",type="string") == "marginLeft",eventData("value",type="number"),memory.layout_marginLeft)
teleportTo:self.customTarget2.getOffsetRelative(y=memory.layout_marginTop*(-1),x=memory.layout_marginLeft)
alsoTriggerAction:refreshLocation

[hiddenAction_refreshLocation]
sendMessageTo:self.customTarget1
sendMessageWithTags:refreshLocation

