[core]
@memory commonCallBackFlag:string
@memory commonCallBackTarget:unit


[hiddenAction_StartCommonCallBack]
buildSpeed:0.0s
sendMessageTo:memory.commonCallBackTarget
sendMessageWithTags:commonCallBack
sendMessageWithData:flag=memory.commonCallBackFlag

#setUnitMemory:commonCallBackFlag="",commonCallBackTarget=null
alsoTriggerAction:CleanCommonCallBack
[hiddenAction_CleanCommonCallBack]
buildSpeed:0.0s
setUnitMemory:commonCallBackFlag="",commonCallBackTarget=null
