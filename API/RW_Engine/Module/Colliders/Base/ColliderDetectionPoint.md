[core]
copyFrom:"""ROOT:RW_Engine/View/View.md"""
name:ColliderDetectionPoint
tags:ColliderDetectionPoint,ColliderMark
isUnselectable:true
showInEditor:false

isBuilding: false
@memory TriggerMarkList:unit[]
@memory index:number

autoTriggerCooldownTime:0.0s
autoTriggerCooldownTime_allowDangerousHighCPU:true

updateUnitMemoryRate:0
updateUnitMemory:index=select(memory.index == memory.TriggerMarkList.size,0,memory.index+1)
@memory ColliderDetectionObjectListQueueManager:unit

[graphics]
total_frames:1
image:point.png

[hiddenAction_parentDeleted]
@copyFrom_skipThisSection:true


[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
setCustomTarget1:self.activeWaypointTarget
sendMessageTo:self.customTarget1
sendMessageWithTags:ColliderDetectionPointAdd
setUnitMemory:ColliderDetectionObjectListQueueManager=self.activeWaypointTarget.readUnitMemory("ColliderDetectionObjectList",type="unit").readUnitMemory("ColliderDetectionObjectListQueueManager",type="unit")

[hiddenAction_Trigger]
autoTrigger:if self.customTarget1.readUnitMemory("ColliderStatus",type="boolean") == true
#takeResources:hp=1
takeResources_searchOnly:true
takeResources_excludeUnitsWithoutTags:ColliderMark
takeResources_includeUnitsWithinRange:8
takeResources_triggerActionForEach:CheckMarkInList-Add

[hiddenAction_CheckMarkInList-Add]

requireConditional:if thisActionTarget != self.customTarget1 and thisActionTarget.customTarget1 != self.customTarget1 and memory.TriggerMarkList.contains(thisActionTarget)==false

setUnitMemory:TriggerMarkList[memory.TriggerMarkList.size]=thisActionTarget


sendMessageTo:memory.ColliderDetectionObjectListQueueManager
sendMessageWithTags:CheckMarkInList
sendMessageWithData:mark=thisActionTarget


[hiddenAction_CheckMarkInList-Remove]
autoTrigger:if self.customTarget1.readUnitMemory("ColliderStatus",type="boolean") == true
requireConditional:if memory.TriggerMarkList[memory.index] != nullUnit and select(memory.TriggerMarkList[memory.index].x >= self.x-self.customTarget1.readUnitMemory("ColliderMarkRange",type="number") and memory.TriggerMarkList[memory.index].x <= self.x+self.customTarget1.readUnitMemory("ColliderMarkRange",type="number") and memory.TriggerMarkList[memory.index].y >= self.y-self.customTarget1.readUnitMemory("ColliderMarkRange",type="number") and memory.TriggerMarkList[memory.index].y <= self.y+self.customTarget1.readUnitMemory("ColliderMarkRange",type="number"),false,true) == true

sendMessageTo:memory.ColliderDetectionObjectListQueueManager
sendMessageWithTags:CheckMarkInList
sendMessageWithData:mark=memory.TriggerMarkList[memory.index]
alsoTriggerAction:Remove


[hiddenAction_Remove]
setUnitMemory:TriggerMarkList[memory.index] = nullUnit
shrinkArrays:TriggerMarkList

