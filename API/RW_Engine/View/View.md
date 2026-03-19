[core]
class:CustomUnitMetadata
price:0
maxHp:1
mass:0
buildSpeed:0
isBuilding: true
radius:0
tags:View
stayNeutral:true
isBio:false
showOnMinimap:false
canNotBeDirectlyAttacked: true
canNotBeDamaged:true
autoTriggerCooldownTime:0.0s
autoTriggerCooldownTime_allowDangerousHighCPU:true
disableAllUnitCollisions:true
@global soltSize:12

[graphics]
total_frames:1
image:ROOT:RW_Engine/View/空.png
image_wreak:NONE
image_turret:NONE
imageSmoothing:true
[hiddenAction_created]
autoTriggerOnEvent:created
[hiddenAction_commonRequest]
autoTriggerOnEvent:created
resetCustomTimer:true
sendMessageTo:self.customTarget1
sendMessageWithTags:commonRequest
sendMessageWithData:name="${core.name}",tags="${core.tags}"

[hiddenAction_parentDeleted]
autoTrigger:if self.customTarget1 == null
deleteSelf:true


[hiddenAction_getViewById]
autoTriggerOnEvent:tookDamage(withTag="getViewById")
sendMessageTo:eventSource
sendMessageWithTags:getViewByIdReport


[hiddenAction_TouchAutoAddHp]
autoTriggerOnEvent:tookDamage(withTag="Touch")
addResources:hp=1


[hiddenAction_deletedView]
autoTriggerOnEvent:newMessage(withTag="deletedView")
deleteSelf:true

[hiddenAction_stringToInt]



[attack]

canAttack:false

[movement]
movementType:NONE
moveSpeed:0

