[core]
@memory show:boolean[]
@global tier = 1
[canBuild_0]
name:reclaim,repair
pos:-1
[action_inf]
isVisible:true
unitShownInUI:${core.name}
buildSpeed:0
pos:32
text:信息
displayType:infoOnlyNoBox
description:建造等级${tier}\n-建造速度${core.nanoBuildSpeed}\n-回收速度${core.nanoReclaimSpeed}\n-维修速度${core.nanoRepairSpeed}\n-光束距离${core.nanoRange/20}格
[canBuild_resource]
@define idx = 1
pos:${idx}
isVisible:if memory.show[${idx}] == true
name:drill1,gen1,dust_coll,presser,calCon,table
[canBuild_resource2]
@define idx = 1
pos:${idx}
isVisible:if memory.show[${idx}] == true and ${tier}>1
name:drill2,gen2,glassCargo
[canBuild_resource3]
@define idx = 1
pos:${idx}
isVisible:if memory.show[${idx}] == true and ${tier}>2
name:drill3,gen3
[canBuild_defend]
@define idx = 2
pos:${idx}
isVisible:if memory.show[${idx}] == true
name:staTur1,misTur1,tractionBeam,Ltur1,repairTur,wall,wall2,walli,walli2,walla,walla2,mine,bigmine,cymine
[canBuild_defend2]
@define idx = 2
pos:${idx}
isVisible:if memory.show[${idx}] == true and ${tier}>1
name:staTur2,staLTur2,staTTur2,staRTur2,3tur1,misBlaster,misTurH2,hyperTractionBeam,Ltur2,3tur-repair,radarTur,wall3,walli3,walla3
[canBuild_defend3]
@define idx = 2
pos:${idx}
isVisible:if memory.show[${idx}] == true and ${tier}>2
name:staTur3,staLTur3,staTTur3,staRTur3,misTra,misTurH3,Ltur3
[canBuild_factory]
@define idx = 3
pos:${idx}
isVisible:if memory.show[${idx}] == true
name:stafc1,airfc1,mechfc1,dronefc1
[canBuild_factory2]
@define idx = 3
pos:${idx}
isVisible:if memory.show[${idx}] == true and ${tier}>1
name:stafc2,airfc2,dronefc2
[canBuild_factory3]
@define idx = 3
pos:${idx}
isVisible:if memory.show[${idx}] == true and ${tier}>2
name:stafc3,airfc3,dronefc3
[canBuild_electric]
@define idx = 4
pos:${idx}
isVisible:if memory.show[${idx}] == true
name:Scell,solPan1,ASPC,ASPU,reactor,RTG,fuelCell,capac,capac2,capac4,batteryBase
[canBuild_special]
@define idx = 5
pos:${idx}
isVisible:if memory.show[${idx}] == true
name:signSta,slander,slauncher,laserEmit,rlaserEmit,allaserEmit,laserRelay,laserSTO,laserOVF,nukePad,block,gball
[action_tog1]
displayType:infoOnlyNoBox
@define idx = 1
pos:${idx-0.5}
description:单击展开或折叠
text:资源
buildSpeed:0s
alwaysSinglePress:true
iconImage:ROOT:icon/res.png
iconExtraImage:ROOT:icon/folder.png
iconExtraIsVisible:if memory.show[${idx}] == true
setUnitMemory:show[${idx}]=not memory.show[${idx}]
[action_tog2]
@define idx = 2
@copyFromSection:action_tog1
text:防御
iconImage:ROOT:icon/defi.png
[action_tog3]
@define idx = 3
@copyFromSection:action_tog1
text:工厂
iconImage:ROOT:icon/uniti.png
[action_tog4]
@define idx = 4
@copyFromSection:action_tog1
text:电力
iconImage:ROOT:icon/elec.png
[action_tog5]
@define idx = 5
@copyFromSection:action_tog1
text:特殊
iconImage:ROOT:icon/spec.png
[action_clear]
displayType:infoOnlyNoBox
pos:-0.5
description:单击收起所有
text:收起
buildSpeed:0s
alwaysSinglePress:true
iconImage:ROOT:icon/folder2.png
setUnitMemory:show[0]=false,show[1]=false,show[2]=false,show[3]=false,show[4]=false,show[5]=false