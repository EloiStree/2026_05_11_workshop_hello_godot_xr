**Objectifs:** RGB Led

Allez un petit pause.    
    
Parlons materiels, duplication et texture.   
Creeons ensembles un led RGB 💡🚦   


Parlons d' un strip led 8x8x8    
Ca fait vite 512 led ;)  

Ici, ils nous faut un nouveau type solution avec un texture et un mesh generer   
Ou une animation bone et une piscine.   


-----------------------

-----------------------


**Solution:**
Change the Color of a MeshInstance3D
``` gdscript
class_name MicroBitSetMeshInstanceColor3D
extends Node

@export var to_affect: MeshInstance3D = null
@export var duplicate_material_at_ready: bool = true

func _ready() -> void:
	if to_affect == null:
		push_warning("No MeshInstance3D assigned to 'to_affect'")
		return
	
	if duplicate_material_at_ready:
		var mat:Material = to_affect.get_active_material(0)
		if mat:
			var new_mat = mat.duplicate()
			to_affect.set_surface_override_material(0, new_mat)

func set_color_of_the_mesh_material(new_color: Color):
	var mat = to_affect.get_active_material(0)
	if mat:
		mat.albedo_color = new_color

```



Creer une couleur
``` gdscript

class_name SensorViewLedRgbColor
extends Node

signal on_color_updated(color:Color)
signal on_color_changed(color:Color)

@export var current_color:Color
@export_range(0,255) var byte_red_255:int;
@export_range(0,255) var byte_green_255:int;
@export_range(0,255) var byte_blue_255:int;


func set_rgb_with_bytes_0_255(red_0_255:int, green_0_255:int, blue_0_255:int)->void:
	var changed = byte_red_255 != red_0_255 or byte_green_255 != green_0_255 or byte_blue_255 != blue_0_255
	if not changed:
		on_color_updated.emit(current_color)
		return
	byte_red_255 = red_0_255	
	byte_green_255= green_0_255
	byte_blue_255= blue_0_255
	current_color =  Color(byte_red_255/255.0, byte_green_255/255.0, byte_blue_255/255.0)
	on_color_updated.emit(current_color)
	on_color_changed.emit(current_color)

func set_rgb_with_three_on_off(is_red_on:bool, is_green_on:bool, is_blue_on:bool)->void:
	var br = 255 if is_red_on else 0
	var bg = 255 if is_green_on else 0
	var bb = 255 if is_blue_on else 0
	set_rgb_with_bytes_0_255(br, bg, bb)

func turn_rgb_to_off_black():
	set_rgb_with_three_on_off(false, false, false)

func _turn_rgb_to_on_color_off_black(is_on:bool, r:int, g:int, b:int):
	if is_on:
		set_rgb_with_three_on_off(r, g, b)
	else :
		turn_rgb_to_off_black()

func turn_rgb_to_on_off_white(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 255, 255, 255)

func turn_rgb_to_on_off_red(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 255, 0, 0)

func turn_rgb_to_on_off_green(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 0, 255, 0)

func turn_rgb_to_on_off_blue(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 0, 0, 255)


func turn_rgb_to_on_off_yellow(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 255, 255, 0)	

func turn_rgb_to_on_off_cyan(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 0, 255, 255)

func turn_rgb_to_on_off_magenta(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 255, 0, 255)

func turn_rgb_to_on_off_orange(is_on:bool):
	_turn_rgb_to_on_color_off_black(is_on, 255, 165, 0)

	
func set_rgb_with_color(color:Color)->void:
	set_rgb_with_bytes_0_255(int(color.r*255), int(color.g*255), int(color.b*255))

func set_color_with_hexa_string_ffffff(hexa_color_string:String)->void:
	set_rgb_with_color(Color(hexa_color_string))


func get_f_value_in_decimal(text_0_16_f)->int:
	match text_0_16_f:
		"0":
			return 0
		"1":
			return 1
		"2":
			return 2
		"3":
			return 3
		"4":
			return 4
		"5":
			return 5
		"6":
			return 6
		"7":
			return 7
		"8":
			return 8
		"9":
			return 9
		"A", "a":
			return 10
		"B", "b":
			return 11
		"C", "c":
			return 12
		"D", "d":
			return 13
		"E", "e":
			return 14
		"F", "f":
			return 15
	return 0

func get_ff_value_in_decimal(hexa_color_string:String)->int:
	var first_digit = get_f_value_in_decimal(hexa_color_string[0])
	var second_digit = get_f_value_in_decimal(hexa_color_string[1])
	return first_digit*16 + second_digit

#region RED


func set_red_with_analog_1024(analog_value:int)->void:
	var red_0_255 = int(analog_value/1023.0*255)
	set_red_with_byte_0_255(red_0_255)

func set_red_with_byte_0_255(red:int)->void:
	set_rgb_with_bytes_0_255(red, byte_green_255, byte_blue_255)

func set_red_with_integer_0_255(red:int)->void:
	set_red_with_byte_0_255(red)

func set_red_with_percent01(red:float)->void:
	set_red_with_byte_0_255(int(red*255))

func set_red_with_hexa_string_00_ff(hexa_color_string:String)->void:
	set_red_with_byte_0_255(get_ff_value_in_decimal(hexa_color_string))


func set_red_with_on_off(is_on:bool)->void:
	if is_on:
		set_red_with_byte_0_255(255)
	else:
		set_red_with_byte_0_255(0)

#endregion

#region GREEN

func set_green_with_analog_1024(analog_value:int)->void:
	var green_0_255 = int(analog_value/1023.0*255)
	set_green_with_byte_0_255(green_0_255)

func set_green_with_byte_0_255(green:int)->void:
	set_rgb_with_bytes_0_255(byte_red_255, green, byte_blue_255)

func set_green_with_integer(green:int)->void:
	set_green_with_byte_0_255(green)	

func set_green_with_percent01(green:float)->void:
	set_green_with_byte_0_255(int(green*255))

func set_green_with_hexa_string_00_ff(hexa_color_string:String)->void:
	set_green_with_byte_0_255(get_ff_value_in_decimal(hexa_color_string))


func set_green_with_on_off(is_on:bool)->void:
	if is_on:
		set_green_with_byte_0_255(255)
	else:
		set_green_with_byte_0_255(0)


#endregion	


#region BLUE

func set_blue_with_analog_1024(analog_value:int)->void:
	var blue_0_255 = int(analog_value/1023.0*255)
	set_blue_with_byte_0_255(blue_0_255)

func set_blue_with_byte_0_255(blue:int)->void:
	set_rgb_with_bytes_0_255(byte_red_255, byte_green_255, blue)

func set_blue_with_integer_0_255(blue:int)->void:
	set_blue_with_byte_0_255(blue)

func set_blue_with_percent01(blue:float)->void:
	set_blue_with_byte_0_255(int(blue*255))

func set_blue_with_hexa_string_00_ff(hexa_color_string:String)->void:
	set_blue_with_byte_0_255(get_ff_value_in_decimal(hexa_color_string))

func set_blue_with_on_off(is_on:bool)->void:
	if is_on:
		set_blue_with_byte_0_255(255)
	else:
		set_blue_with_byte_0_255(0)

#endregion


```


 Gestion de group
``` gdscript
class_name SensorViewStripOfLedsRgbColor
extends Node


@export var leds:Array[SensorViewLedRgbColor] = []


@export_group("Add from childrens")
@export var parent_node_of_leds:Node


func turn_on():
	for led in leds:
		if led:
			led.turn_rgb_to_on_off_white(true)

func turn_off():
	for led in leds:
		if led:
			led.turn_rgb_to_on_off_white(false)


func turn_on_off(is_on:bool):
	if is_on:
		turn_on()
	else:
		turn_off()
		
func set_with_random_color():
	for led in leds:
		if led:
			var r = randi()%256
			var g = randi()%256
			var b = randi()%256
			led.set_rgb_with_bytes_0_255(r, g, b)



func _ready() -> void:
	_recusrive_add_leds_from_parent(parent_node_of_leds)



func _recusrive_add_leds_from_parent(node:Node)->void:
	if node:
		for child in node.get_children():
			if child is SensorViewLedRgbColor:
				leds.append(child)
			_recusrive_add_leds_from_parent(child)
```









