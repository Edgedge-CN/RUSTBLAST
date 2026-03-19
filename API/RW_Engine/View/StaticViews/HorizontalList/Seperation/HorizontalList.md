[core]
copyFrom:"""ROOT:RW_Engine/View/View.md,ROOT:RW_Engine/View/CommonCallBack.md"""
name:HorizontalList
tags:HorizontalList

@memory HorizontalListChild:unit[]
@memory HorizontalUnitChildList:unit[]
@memory count:number
@memory maxCount:number

@memory spacedPx:number

@memory needTag:string

#showInEditor:false
@global HorizontalListChild:unitRef
@memory HorizontalListWayPoint:unit

[hiddenAction_parentDeleted]
@copyFrom_skipThisSection:true

[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
setUnitMemory:maxCount=10,spacedPx=35,needTag="NULL"
spawnUnits:HorizontalListWayPoint(offsetX=40,offsetY=40)



[hiddenAction_SetHorizontalListWayPoint]
autoTriggerOnEvent:newMessage(withTag="HorizontalListWayPoint")
setUnitMemory:HorizontalListWayPoint=eventSource
alsoTriggerAction:addNewHorizontalListChild
alsoTriggerActionRepeat:memory.maxCount

[hiddenAction_addNewHorizontalListChild]
spawnUnits:${HorizontalListChild}(spawnSource=self.getOffsetRelative(x=memory.spacedPx*index),copyWaypointsFrom=memory.HorizontalListWayPoint)

[hiddenAction_setNewHorizontalListChild]
autoTriggerOnEvent:newMessage(withTag="setNewHorizontalListChild")
setUnitMemory:HorizontalListChild[memory.HorizontalListChild.size]=eventSource



[hiddenAction_addHorizontalUnitChildToList]
autoTriggerOnEvent:newMessage(withTag="addHorizontalUnitChildToList")
requireConditional:if memory.HorizontalUnitChildList.size < memory.maxCount and eventData("tag",type="string") == memory.needTag
setUnitMemory:commonCallBackFlag=eventData("commonCallBackFlag",type="string"),commonCallBackTarget=eventSource,HorizontalUnitChildList[memory.count]=eventSource,count=memory.count+1
sendMessageTo:eventSource
sendMessageWithTags:HorizontalUnitChildTeleportToHorizontalListChild
sendMessageWithData:listId=memory.count-1,HorizontalListChild=memory.HorizontalListChild[(memory.maxCount-memory.count)]
showMessageToAllPlayers:%{memory.maxCount-memory.count}
alsoTriggerAction:StartCommonCallBack


[hiddenAction_removeAllHorizontalListChild]
