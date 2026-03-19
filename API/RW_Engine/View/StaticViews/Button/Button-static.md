[core]
copyFrom:"""ROOT:RW_Engine/View/View.md,ROOT:RW_Engine/Module/LayoutInterpreter/EnabledLayoutInterpreter.md,ROOT:RW_Engine/Module/LayoutInterpreter/ImportLayoutInterpreter.md,ROOT:RW_Engine/View/CommonCallBack.md"""
name:Button-static
tags:Button-static
showInEditor:false
isUnselectable:true
footprint: 0,0,1,0
constructionFootprint:0,0,1,0
displayFootprint: 0,0,1,0
buildingToFootprintOffsetX:20
@memory LTVSystem:unit
@memory enableTextAutoCenter:boolean
@memory text:string

@memory initLayoutViewSucceedFlag:string
@memory initLayoutViewSucceedTarget:unit
@global setEnableTextAutoCenter:true


[graphics]
total_frames:1
image:${ViewImagePath}/Button.png


[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
setUnitMemory:enableTextAutoCenter=${setEnableTextAutoCenter}

[hiddenAction_spawnLayoutView]
#创建文本框
spawnUnits:LineTextViewSystem(addResources=ViewId=1)



[hiddenAction_initLayoutView]
buildSpeed:0.0s
requireConditional:if memory.enableTextAutoCenter == true
sendMessageTo:memory.LayoutInterpreter
sendMessageWithTags:LayoutInterpreterInit
sendMessageWithData:InitializeData="///Id:1,Attribute:layout_marginTop=0,layout_marginLeft="+str(int((length(memory.text)-1)*${defaultfontSize}*memory.LTVSystem.readUnitMemory("textScale",type="number")/2*(-1)))+",///",commonCallBackFlag=memory.initLayoutViewSucceedFlag,commonCallBackTarget=memory.initLayoutViewSucceedTarget


#alsoTriggerAction:StartCommonCallBack


#showMessageToAllPlayers:%{str(int((length(memory.text)-1)*${defaultfontSize}*memory.LTVSystem.readUnitMemory("textScale",type="number")/2*(-1)))}

#文本框会自动发送tag为addLTVSystemToEpoll给该单位
[hiddenAction_addLTVSystemToEpoll]
autoTriggerOnEvent:newMessage(withTag="addLTVSystemToEpoll")
setUnitMemory:LTVSystem = eventSource
alsoTriggerAction:addLTVSystemToEpollSucceed
[hiddenAction_addLTVSystemToEpollSucceed]
addResources:hp=1
[hiddenAction_refreshLocation]
sendMessageTo:memory.LTVSystem
sendMessageWithTags:setSoltId
sendMessageWithData:slotId=0

[hiddenAction_Touch]
autoTriggerOnEvent:tookDamage(withTag="Touch")



