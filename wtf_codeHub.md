# Made by Edgedge
## 武器
 - 弹道效果
[projectile_x]
turnSpeed:0
turnSpeedWhenNear:0
retargetingInFlight:true
-
## 图形
- 条
[decal_bar_mine]
color: #ff2222
image:SHARED:white_pixel.png
imageScale:2
imageScaleX:(self.hp / self.maxHp)*15
alwaysStartDirAtZero:true
isVisible:if not (self.hp == self.maxHp)
onlyOnNonPreview:true
xOffsetAbsolute:((self.hp/self.maxHp)*15)-15
yOffsetAbsolute:-12
alpha:0.8
onlyWhileActive:true
[decal_mine_back]
color: #360b0b
image:SHARED:white_pixel.png
order:-1
isVisible:if not (self.hp == self.maxHp)
imageScale:2
imageScaleX:15
alpha:0.5
alwaysStartDirAtZero:true
onlyOnNonPreview:true
yOffsetAbsolute:-12
onlyWhileActive:true