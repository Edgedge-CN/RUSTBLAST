[core]
copyFrom:"""ROOT:RW_Engine/View/View.md"""
name:ColliderDetectionObjectList
tags:ColliderDetectionObjectList
showInEditor:false
isBuilding: false
#isUnselectable:true
@memory TriggerMarkList:unit[]
@memory TriggerObjectList:unit[]
@memory TriggerReferenceList:number[]

@memory ColliderDetectionObjectListQueueManager:unit
@memory isBusy:boolean


@memory TemporaryNearestDetectionPonit:unit
@memory TemporaryIndex:number
@memory TemporarySize:number


@global MARK:eventData("mark",type="unit")
@global COLLIDER_MARK_RANG:self.customTarget1.readUnitMemory("ColliderMarkRange",type="number")

[attachment_ColliderDetectionObjectListQueueManager]
x:0
y:0
onCreateSpawnUnitOf:ColliderDetectionObjectListQueueManager(addResources=ViewId=1)
isUnselectable:false

[hiddenAction_ColliderDetectionObjectListQueueManager]
autoTriggerOnEvent:newMessage(withTag="ColliderDetectionObjectListQueueManager")
setUnitMemory:ColliderDetectionObjectListQueueManager=eventSource
[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
sendMessageTo:self.customTarget1
sendMessageWithTags:ColliderDetectionObjectList



[action_a]
text:%{memory.TriggerReferenceList[0]}\n%{memory.TriggerMarkList[0]}\n%{memory.TriggerObjectList[1]}
alsoTriggerAction:created

[hiddenAction_CheckMarkInList]
autoTriggerOnEvent:newMessage(withTag="CheckMarkInList")

setUnitMemory:TemporaryNearestDetectionPonit=self.customTarget1.readUnitMemory("ColliderDetectionPointsList",type="unit[]")[0],isBusy=true

alsoTriggerAction:checkRealDistance
alsoTriggerActionRepeat:self.customTarget1.readUnitMemory("ColliderDetectionPointsList",type="unit[]").size

[hiddenAction_checkRealDistance]
setUnitMemory:TemporaryNearestDetectionPonit=select(distanceBetweenSquared(self.customTarget1.readUnitMemory("ColliderDetectionPointsList",type="unit[]")[index], ${MARK}) < distanceBetweenSquared(memory.TemporaryNearestDetectionPonit, ${MARK}),self.customTarget1.readUnitMemory("ColliderDetectionPointsList",type="unit[]")[index],memory.TemporaryNearestDetectionPonit)

alsoTriggerAction:Add,Remove,Ignore

alsoTriggerOrQueueActionConditional:if index+1 == self.customTarget1.readUnitMemory("ColliderDetectionPointsList",type="unit[]").size

#showMessageToAllPlayers:%{self.customTarget1.readUnitMemory("ColliderDetectionPointsList",type="unit[]")[index].id}

[hiddenAction_Ignore]
requireConditional:if (memory.TriggerMarkList.contains(${MARK}) == true and select(${MARK}.x >= memory.TemporaryNearestDetectionPonit.x-${COLLIDER_MARK_RANG} and ${MARK}.x <= memory.TemporaryNearestDetectionPonit.x+${COLLIDER_MARK_RANG} and ${MARK}.y >= memory.TemporaryNearestDetectionPonit.y-${COLLIDER_MARK_RANG} and ${MARK}.y <= memory.TemporaryNearestDetectionPonit.y+${COLLIDER_MARK_RANG},false,true) == false) or (memory.TriggerMarkList.contains(${MARK}) == false and select(${MARK}.x >= memory.TemporaryNearestDetectionPonit.x-${COLLIDER_MARK_RANG} and ${MARK}.x <= memory.TemporaryNearestDetectionPonit.x+${COLLIDER_MARK_RANG} and ${MARK}.y >= memory.TemporaryNearestDetectionPonit.y-${COLLIDER_MARK_RANG} and ${MARK}.y <= memory.TemporaryNearestDetectionPonit.y+${COLLIDER_MARK_RANG},false,true) == true) 


alsoQueueAction:CheckMarkInList-Shrink

[hiddenAction_Add]
requireConditional:if memory.TriggerMarkList.contains(${MARK}) == false and select(${MARK}.x >= memory.TemporaryNearestDetectionPonit.x-${COLLIDER_MARK_RANG} and ${MARK}.x <= memory.TemporaryNearestDetectionPonit.x+${COLLIDER_MARK_RANG} and ${MARK}.y >= memory.TemporaryNearestDetectionPonit.y-${COLLIDER_MARK_RANG} and ${MARK}.y <= memory.TemporaryNearestDetectionPonit.y+${COLLIDER_MARK_RANG},false,true) == false

setUnitMemory:TriggerReferenceList[memory.TriggerReferenceList.size]=select(memory.TriggerObjectList.contains(${MARK}.customTarget1),0,1),TriggerObjectList[memory.TriggerObjectList.size]=select(memory.TriggerObjectList.contains(${MARK}.customTarget1),nullUnit,${MARK}.customTarget1),TriggerMarkList[memory.TriggerMarkList.size]=select(memory.TriggerMarkList.contains(${MARK}),nullUnit,${MARK})
alsoTriggerAction:CheckMarkInList-AddTriggerReference,ObjectEnter
alsoQueueAction:CheckMarkInList-Shrink
alsoTriggerActionRepeat:memory.TriggerObjectList.size

#showMessageToAllPlayers:ADD%{eventData("mark",type="unit").y}  %{memory.TemporaryNearestDetectionPonit.y+self.customTarget1.readUnitMemory("ColliderMarkRange",type="number")}

[hiddenAction_ObjectEnter]
requireConditional:if memory.TriggerReferenceList[(memory.TriggerReferenceList.size-1)] == 1

showMessageToAllPlayers:物体:%{${MARK}.customTarget1}(id:%{${MARK}.customTarget1.id}) Enter

[hiddenAction_CheckMarkInList-AddTriggerReference]
requireConditional:if memory.TriggerMarkList[(memory.TriggerMarkList.size-1)] != nullUnit and memory.TriggerReferenceList[(memory.TriggerReferenceList.size-1)] == 0 and memory.TriggerObjectList[index] == memory.TriggerMarkList[(memory.TriggerMarkList.size-1)].customTarget1
setUnitMemory:TriggerReferenceList[index]=memory.TriggerReferenceList[index]+1

[hiddenAction_CheckMarkInList-Shrink]
buildSpeed:0.0s
shrinkArrays:TriggerMarkList,TriggerObjectList,TriggerReferenceList
setUnitMemory:isBusy=false
sendMessageTo:memory.ColliderDetectionObjectListQueueManager
sendMessageWithTags:unlocked




[hiddenAction_Remove]
requireConditional:if memory.TriggerMarkList.contains(${MARK}) == true and select(${MARK}.x >= memory.TemporaryNearestDetectionPonit.x-${COLLIDER_MARK_RANG} and ${MARK}.x <= memory.TemporaryNearestDetectionPonit.x+${COLLIDER_MARK_RANG} and ${MARK}.y >= memory.TemporaryNearestDetectionPonit.y-${COLLIDER_MARK_RANG} and ${MARK}.y <= memory.TemporaryNearestDetectionPonit.y+${COLLIDER_MARK_RANG},false,true) == true


setUnitMemory:TemporarySize=memory.TriggerObjectList.size
#MessageToAllPlayers:REMOVE
alsoTriggerAction:CheckMarkInList-RemoveTriggerReference,RemoveMarkBefore

alsoTriggerActionRepeat:memory.TemporarySize
alsoTriggerOrQueueActionWithTarget:${MARK}
[hiddenAction_CheckMarkInList-RemoveTriggerReference]
requireConditional:if memory.TriggerObjectList[index] == thisActionTarget.customTarget1

setUnitMemory:TriggerReferenceList[index]=memory.TriggerReferenceList[index]-1,TemporaryIndex=index

alsoTriggerAction:ObjectLeft

[hiddenAction_ObjectLeft]
requireConditional:if memory.TriggerReferenceList[memory.TemporaryIndex]==0


showMessageToAllPlayers:物体:%{memory.TriggerObjectList[memory.TemporaryIndex]}(id:%{memory.TriggerObjectList[memory.TemporaryIndex].id}) Left
alsoTriggerAction:RemoveObject

[hiddenAction_RemoveObject]
setUnitMemory:TriggerObjectList[memory.TemporaryIndex]=nullUnit

#  MessageToAllPlayers:%{select(memory.TriggerReferenceList[index]==0,"Left","")}

[hiddenAction_RemoveMarkBefore]
buildSpeed:0.0s
requireConditional:if index+1 == memory.TemporarySize
alsoTriggerAction:RemoveMark
alsoTriggerActionRepeat:memory.TriggerMarkList.size
showMessageToAllPlayers:RemoveMarkBefore
[hiddenAction_RemoveMark]
showMessageToAllPlayers:h%{thisActionTarget}
requireConditional:if memory.TriggerMarkList[index] == thisActionTarget
setUnitMemory:TriggerMarkList[index] = nullUnit

alsoQueueAction:CheckMarkInList-Shrink


