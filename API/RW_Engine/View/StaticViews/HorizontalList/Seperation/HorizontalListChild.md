[core]
copyFrom:"""ROOT:RW_Engine/View/View.md"""
name:HorizontalListChild
tags:HorizontalListChild
#showInEditor:false

isUnselectable:false

[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
setCustomTarget1:self.activeWaypointTarget
sendMessageTo:self.customTarget1
sendMessageWithTags:setNewHorizontalListChild


[hiddenAction_parentDeleted]
@copyFrom_skipThisSection:true
