[core]
copyFrom:"""ROOT:RW_Engine/View/View.md,ROOT:RW_Engine/View/CommonCallBack.md"""
name:HorizontalListChild
tags:HorizontalListChild

@memory listId:number

[hiddenAction_addHListChildToList]
autoTriggerOnEvent:newMessage(withTag="addHListChildToList")
requireConditional:if eventData("needTag",type="string") == "NULL"
setUnitMemory:commonCallBackFlag=eventData("commonCallBackFlag",type="string"),commonCallBackTarget=eventSource,listId=eventData("listId",type="number")

#sendMessageWithData:listId=memory.count,startX=memory.startX,startY=memory.startY,spacedPx=memory.spacedPx

teleportTo:createMarker(x=eventSource.x+eventData("startX",type="number")+eventData("spacedPx",type="number")*(memory.listId-1),y=eventSource.y+eventData("startY",type="number"))
alsoTriggerAction:StartCommonCallBack