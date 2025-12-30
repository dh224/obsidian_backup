这两天是端午节。我正好有空制作一个太空射击游戏。
这个游戏原本是我的学生（2118 范小雨等人）经过讨论后确定下的毕业设计主题，素材由他们绘制完成，而我，在指导他们工作之余，还需要帮助他们写一点代码，实现基础的游戏循环。
首先让我来描述一下这个太空射击游戏。玩家需要操作飞船在地球的近地轨道LEO、中轨道MEO以及高空轨道GEO之间来回穿梭，消灭空中的太空垃圾以及不怀好意的外星人。玩家除了需要注意飞船自身的健康度以外，还需要注意燃料，燃料消耗结束同样会结束游戏。三条轨道的燃料消耗程度不同；敌人的情况也有所不同。该游戏计划使用godot进行开发。

一开始，我只想要做一个简单的UI，显示玩家的血量、燃料、得分以及暂停游戏。心想，这还不简单，然后事情就不出意外的出意外了。

一开始，我找到了一些游戏素材，用来显示当前的血量。
![[Pasted image 20250602101328.png]]

很可惜这是一个Sprite Sheet，但是也可以使用TextureRect中的AtlasTexture，然后调整不同的region来实现血量的显示变化。以下是实现的代码

``` python
extends Control
class_name GameUI

const HELATH_BAR_NUMBER_MAX = 4
@export var health_bar_rect : TextureRect

var health_bar_texture : Texture2D 

var health_bar_region_array : Array[Rect2] = [
	Rect2(0,32,16,16), #0
	Rect2(16,64,16,16), #1
	Rect2(48,64,16,16), #2
	Rect2(80,64,16,16), #3
	Rect2(112,64,16,16), #4
]

var health_bar_number : int


func _ready() -> void:
	var texture = health_bar_rect.texture
	print("设置")
	health_bar_texture = texture
	health_bar_number = HELATH_BAR_NUMBER_MAX
	print_stack()
	set_health_bar(HELATH_BAR_NUMBER_MAX)
		
		
func set_health_bar(number : int) -> void:
	health_bar_rect.texture.region = health_bar_region_array[number]
	health_bar_number = number
	print(health_bar_rect.texture.region)
	
func add_health(number : int) -> void:
	print("增加")
	
	if health_bar_number + number > HELATH_BAR_NUMBER_MAX:
		health_bar_number = HELATH_BAR_NUMBER_MAX
		
	else:
		health_bar_number += number
		set_health_bar(health_bar_number)

func minus_health(number : int) -> void:
	print("减少")
	if health_bar_number - number > 0:
		health_bar_number -= number
		set_health_bar(health_bar_number)
	else:
		health_bar_number = 0
		set_health_bar(health_bar_number)
		# 结束游戏
	

```

然而当我运行游戏后，我发现并不能起到修改的作用