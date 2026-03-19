[core]
copyFrom:"""ROOT:RW_Engine/View/View.md,ROOT:RW_Engine/View/CommonCallBack.md"""
name:HorizontalList
tags:HorizontalList

@memory child:unit[]
@memory count:number
@memory maxCount:number

@memory startX:number
@memory startY:number
@memory spacedPx:number


@memory needTag:string

#showInEditor:false



[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
setUnitMemory:maxCount=10,startX=10,startY=5,spacedPx=35,needTag="NULL"


[hiddenAction_addHListChildToList]
autoTriggerOnEvent:newMessage(withTag="addHListChildToList")
requireConditional:if memory.child.size < memory.maxCount
setUnitMemory:commonCallBackFlag=eventData("commonCallBackFlag",type="string"),commonCallBackTarget=eventSource,child[memory.count]=eventSource,count=memory.count+1
sendMessageTo:eventSource
sendMessageWithTags:addHListChildToList
sendMessageWithData:listId=memory.count,startX=memory.startX,startY=memory.startY,spacedPx=memory.spacedPx,needTag=memory.needTag
alsoTriggerAction:StartCommonCallBack

[hiddenAction_deleteHListChildFromList]
setUnitMemory:commonCallBackFlag=eventData("commonCallBackFlag",type="string"),commonCallBackTarget=eventSource
alsoTriggerAction:StartCommonCallBack


[hiddenAction_getHListChildById]
setUnitMemory:commonCallBackFlag=eventData("commonCallBackFlag",type="string"),commonCallBackTarget=eventSource
sendMessageTo:eventSource
sendMessageWithTags:getHListChildById
sendMessageWithData:child=memory.child[(eventData("id",type="number"))],flag=memory.commonCallBackFlag
alsoTriggerAction:CleanCommonCallBack


[hiddenAction_getHListCount]
setUnitMemory:commonCallBackFlag=eventData("commonCallBackFlag",type="string"),commonCallBackTarget=eventSource
sendMessageTo:eventSource
sendMessageWithTags:getHListCount
sendMessageWithData:count=memory.count,flag=memory.commonCallBackFlag
alsoTriggerAction:CleanCommonCallBack


[hiddenAction_setHListMaxCount]
setUnitMemory:commonCallBackFlag=eventData("commonCallBackFlag",type="string"),commonCallBackTarget=eventSource
sendMessageTo:eventSource
sendMessageWithTags:setHListMaxCountSuccessed
sendMessageWithData:flag=memory.commonCallBackFlag
alsoTriggerAction:CleanCommonCallBack

[hiddenAction_refreshLocation]


