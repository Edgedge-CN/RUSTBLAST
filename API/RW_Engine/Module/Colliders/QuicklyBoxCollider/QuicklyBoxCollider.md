[core]
copyFrom:"""ROOT:RW_Engine/View/View.md"""
name:QuicklyBoxCollider
tags:QuicklyBoxCollider,Collider
isUnselectable:true
showInEditor:false

isBuilding: true
@memory TriggerMarkList:unit[]
@memory TriggerObjectList:unit[]
@memory TriggerReferenceList:number[]
@memory index:number

autoTriggerCooldownTime:0.0s
autoTriggerCooldownTime_allowDangerousHighCPU:true

updateUnitMemoryRate:0
updateUnitMemory:index=select(memory.isBusy==true,memory.index,select(memory.index == memory.TriggerMarkList.size,0,memory.index+1))


@global Width:100
@global Height:100
@global Range:150


@memory ColliderStatus:boolean
@memory isBusy:boolean
@memory waitShrinkNum:number


@memory TemporaryIndex:number
@memory TemporaryObject:unit

#本图像默认1倍大小x只能放4个字y2行
[decal_1]
layer:beforeUI
alwaysStartDirAtZero: true
xOffsetAbsolute:0
yOffsetAbsolute:0
imageScaleX:${Width}
imageScaleY:${Height}
image:ROOT://RW_Engine/Module/Colliders/QuicklyBoxCollider/Box.png




[graphics]
total_frames:1
image:ROOT:RW_Engine/View/空.png



#----------待复写函数----------#
[hiddenAction_ObjectEnterChain]
[hiddenAction_ObjectLeftChain]
[hiddenAction_ObjectChangedChain]

[hiddenAction_parentDeleted]
@copyFrom_skipThisSection:true


[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
setUnitMemory:ColliderStatus=true


[hiddenAction_Trigger]
autoTrigger:if memory.ColliderStatus == true and memory.isBusy == false and memory.waitShrinkNum == 0
#takeResources:hp=1

takeResources_searchOnly:true
takeResources_excludeUnitsWithoutTags:ColliderMark
takeResources_includeUnitsWithinRange:${Range}
takeResources_triggerActionForEach:Add

[hiddenAction_Add]

requireConditional:if thisActionTarget != self.customTarget1 and thisActionTarget.customTarget1 != self.customTarget1 and thisActionTarget.customTarget1 != nullUnit and memory.TriggerMarkList.contains(thisActionTarget) == false and select(thisActionTarget.x >= self.x - ${Width}/2 and thisActionTarget.x <= self.x + ${Width}/2 and thisActionTarget.y >= self.y - ${Height}/2 and thisActionTarget.y <= self.y + ${Height}/2,true,false) == true
#select中，在范围内为true
setUnitMemory:TriggerMarkList[memory.TriggerMarkList.size]=thisActionTarget,TriggerReferenceList[memory.TriggerReferenceList.size]=select(memory.TriggerObjectList.contains(thisActionTarget.customTarget1)==true,0,1),TriggerObjectList[memory.TriggerObjectList.size]=select(memory.TriggerObjectList.contains(thisActionTarget.customTarget1)==true,nullUnit,thisActionTarget.customTarget1),isBusy=true,waitShrinkNum=memory.waitShrinkNum+1
alsoTriggerAction:AddTriggerReference,ObjectEnter
alsoTriggerActionRepeat:memory.TriggerObjectList.size
alsoQueueAction:Shrink

[hiddenAction_AddTriggerReference]
requireConditional:if memory.TriggerReferenceList[(memory.TriggerReferenceList.size-1)] == 0 and memory.TriggerObjectList[index] == thisActionTarget.customTarget1
setUnitMemory:TriggerReferenceList[index]=memory.TriggerReferenceList[index]+1

[hiddenAction_ObjectEnter]
requireConditional:if memory.TriggerReferenceList[(memory.TriggerReferenceList.size-1)] == 1 and memory.TriggerObjectList[index] == thisActionTarget.customTarget1

alsoTriggerAction:ObjectEnterChain
alsoTriggerOrQueueActionWithTarget:thisActionTarget.customTarget1



[hiddenAction_Remove]
autoTrigger:if  memory.ColliderStatus == true

requireConditional:if memory.TriggerMarkList[memory.index] != nullUnit and select(memory.TriggerMarkList[memory.index].x >= self.x - ${Width}/2 and memory.TriggerMarkList[memory.index].x <= self.x + ${Width}/2 and memory.TriggerMarkList[memory.index].y >= self.y - ${Height}/2 and memory.TriggerMarkList[memory.index].y <= self.y + ${Height}/2,true,false) == false
 

setUnitMemory:isBusy=true,waitShrinkNum=memory.waitShrinkNum+1
alsoQueueAction:Shrink

alsoTriggerAction:RemoveTriggerReference
alsoTriggerActionRepeat:memory.TriggerObjectList.size
alsoTriggerOrQueueActionWithTarget:memory.TriggerMarkList[memory.index]

[hiddenAction_RemoveTriggerReference]
requireConditional:if memory.TriggerObjectList[index] == thisActionTarget.customTarget1

setUnitMemory:TriggerReferenceList[index]=memory.TriggerReferenceList[index]-1,TemporaryIndex=index,TriggerMarkList[memory.index] = nullUnit

alsoTriggerAction:ObjectLeft,ObjectChangedChain
alsoTriggerOrQueueActionWithTarget:memory.TriggerObjectList[memory.TemporaryIndex]
[hiddenAction_ObjectLeft]
requireConditional:if memory.TriggerReferenceList[memory.TemporaryIndex] == 0

setUnitMemory:TemporaryObject=memory.TriggerObjectList[memory.TemporaryIndex],TriggerObjectList[memory.TemporaryIndex] = nullUnit
alsoTriggerAction:ObjectLeftChain
alsoTriggerOrQueueActionWithTarget:memory.TemporaryObject

[hiddenAction_Shrink]
highPriorityQueue:true
buildSpeed:0.0s
setUnitMemory:waitShrinkNum=memory.waitShrinkNum-1,isBusy=select(memory.waitShrinkNum==0,false,true)
shrinkArrays:TriggerMarkList,TriggerObjectList,TriggerReferenceList

