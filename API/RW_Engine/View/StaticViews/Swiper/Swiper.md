[core]
copyFrom:"""ROOT:RW_Engine/View/View.md,ROOT:RW_Engine/View/CommonCallBack.md"""
name:Swiper
tags:Swiper

@memory index:number
#showInEditor:false

@memory min:number
@memory max:number


[hiddenAction_created]
autoTriggerOnEvent:created
resetCustomTimer:true
setUnitMemory:min=1,max=10,index=memory.min

[hiddenAction_previousPage]
autoTriggerOnEvent:newMessage(withTag="previousPage")
setUnitMemory:index=select(memory.index-1 < memory.min,memory.index,memory.index-1),commonCallBackFlag=select(memory.index-1 < memory.min,"min","previousPage"),commonCallBackTarget=eventSource
alsoTriggerAction:StartCommonCallBack,swiperOperation

[hiddenAction_nextPage]
autoTriggerOnEvent:newMessage(withTag="nextPage")
setUnitMemory:index=select(memory.index+1 > memory.max,memory.index,memory.index+1),commonCallBackFlag=select(memory.index+1 > memory.max,"max","nextPage"),commonCallBackTarget=eventSource
alsoTriggerAction:StartCommonCallBack,swiperOperation

[hiddenAction_swiperOperation]
addResources:hp=1

