<img width="512" height="256" src="objectif.svg" alt="Objectif of the day"/>

**Objectif :** Apprendre à utiliser `_unhandled_input` et `InputMap`

Le but ici n’est pas de tout maîtriser, mais de comprendre le principe des inputs dans Godot et de créer des outils via les signaux pour les écouter.
On créera ensuite un simulateur de manette NES.

---

**Checklist :**

* [ ] Savoir écouter un clavier avec `_input`
* [ ] Comprendre la différence entre `_unhandled_input` et `_input`
* [ ] Écouter un bouton avec `InputMap`
* [ ] Écouter un trigger avec `InputMap`
* [ ] Gérer un axe 2D (gauche / droite) avec `InputMap`
  * [ ] Écouter un joystick (`Vector2`) avec `InputMap`
* [ ] Écouter un joystick (`Vector2`) avec `_unhandled_input`
* [ ] Écouter les boutons de la souris
  * [ ] Bonus : écouter le mouvement de la souris (delta)
  * [ ] Bonus : écouter la position de la souris sur l'ecran
    * [ ] Raycaster une sphere sur un object 3D
  

---

Les inputs… ce n’est pas si simple en réalité.

C’est pour cela que, sur Unity, des outils comme [*Rewired*](https://assetstore.unity.com/packages/tools/input-management/rewired-21676) ont été créés, puis repris dans le *Input Action System*.

L’idée est d’arrêter de dépendre directement du hardware en définissant des **actions** à écouter, puis de laisser le designer décider à quoi ces actions correspondent.

En résumé : un système d’actions.

Mais pour un prototype, surtout au début d’un projet, il est parfois plus simple et plus rapide d’écouter directement les inputs de base.



---------

--------


Une video pour plus tard: Cliquer sur un Area3D     
[<img width="1443" height="813" alt="image" src="https://github.com/user-attachments/assets/4f5ceb7a-412d-4f20-92c3-bd4941a71d60" />](https://youtu.be/LmxfQG9vquw?t=206)   
https://youtu.be/LmxfQG9vquw?t=206      



**Une solution:**

Pour la souris:
``` gdscript

class_name SSD1306MouseEventForDebug
extends Node

signal on_scroll_up()
signal on_scroll_down()
signal on_scroll_left()
signal on_scroll_right()

signal on_left_click()
signal on_right_click()
signal on_middle_click()
signal on_left_release()
signal on_right_release()
signal on_middle_release()

signal on_left_click_changed(is_pressing: bool)
signal on_right_click_changed(is_pressing: bool)
signal on_middle_click_changed(is_pressing: bool)

@export var is_clicking_left: bool = false
@export var is_clicking_right: bool = false
@export var is_clicking_middle: bool = false

func _input(event: InputEvent) -> void:
	if event is InputEventMouseButton:
		# --- Scroll handling ---
		if event.pressed:
			match event.button_index:
				MOUSE_BUTTON_WHEEL_UP:
					on_scroll_up.emit()
				MOUSE_BUTTON_WHEEL_DOWN:
					on_scroll_down.emit()
				MOUSE_BUTTON_WHEEL_LEFT:
					on_scroll_left.emit()
				MOUSE_BUTTON_WHEEL_RIGHT:
					on_scroll_right.emit()

		# --- Click handling ---
		match event.button_index:
			MOUSE_BUTTON_LEFT:
				_handle_click(event.pressed, "left")
			MOUSE_BUTTON_RIGHT:
				_handle_click(event.pressed, "right")
			MOUSE_BUTTON_MIDDLE:
				_handle_click(event.pressed, "middle")


func _handle_click(is_pressing: bool, button: String) -> void:
	match button:
		"left":
			var changed := is_pressing != is_clicking_left
			is_clicking_left = is_pressing
			if changed:
				if is_pressing:
					on_left_click.emit()
				else:
					on_left_release.emit()
				on_left_click_changed.emit(is_pressing)

		"right":
			var changed := is_pressing != is_clicking_right
			is_clicking_right = is_pressing
			if changed:
				if is_pressing:
					on_right_click.emit()
				else:
					on_right_release.emit()
				on_right_click_changed.emit(is_pressing)

		"middle":
			var changed := is_pressing != is_clicking_middle
			is_clicking_middle = is_pressing
			if changed:
				if is_pressing:
					on_middle_click.emit()
				else:
					on_middle_release.emit()
				on_middle_click_changed.emit(is_pressing)
```



Pour le clavier et le gamepad:

``` gdscript


class_name SSD1306ModFakeNesInput
extends Node

signal on_nes_input_updated(up:bool,right:bool,down:bool,left:bool,a:bool,b:bool,menu_left_select:bool,menu_right_restart:bool)


@export var is_fake_input_enabled:bool=true

func set_fake_nes_input_as_enabled(enabled:bool):
	self.is_fake_input_enabled = enabled

@export_group("Keyboard input")
@export var input_name_up:String="Up"
@export var input_name_right:String="Right"
@export var input_name_down:String="Down"
@export var input_name_left:String="Left"
@export var input_name_a:String="3"
@export var input_name_b:String="4"
@export var input_name_menu_left_select:String="1"
@export var input_name_menu_right_restart:String="2"

@export_group("Gamepad buttons")
@export var input_gamepad_button_up:Array[int]=[11]
@export var input_gamepad_button_right:Array[int]=[14]
@export var input_gamepad_button_down:Array[int]=[12]
@export var input_gamepad_button_left:Array[int]=[13]
@export var input_gamepad_button_a:Array[int]=[0,9,7]
@export var input_gamepad_button_b:Array[int]=[3,1,2,10,8]
@export var input_gamepad_button_menu_left_select:Array[int]=[4]
@export var input_gamepad_button_menu_right_restart:Array[int]=[6]

@export_group("Gamepad joystick")
@export var joystick_threshold:float=0.33

@export_group("Print")
@export var use_print_keyboard_input:bool=false
@export var use_print_gamepad_button_input:bool=false
@export var use_print_gamepad_joystick_input:bool=false

@export_group("Listen to")
@export var listen_to_keyboard_input:bool=true
@export var listen_to_gamepad_button_input:bool=true
@export var listen_to_gamepad_joystick_input:bool=true

@export_group("Debug")
@export var button_up:bool=false
@export var button_right:bool=false
@export var button_down:bool=false
@export var button_left:bool=false
@export var button_a:bool=false
@export var button_b:bool=false
@export var button_menu_left_select:bool=false
@export var button_menu_right_restart:bool=false
@export var joystick_left_state:Vector2=Vector2.ZERO
@export var joystick_right_state:Vector2=Vector2.ZERO


func is_button(value:int, button_array:Array[int])->bool:
	for button in button_array:
		if value == button:
			return true
	return false

func _unhandled_input(event: InputEvent) -> void:
	if not is_fake_input_enabled:
		return	

	## display keybaord input
	if listen_to_keyboard_input and event is InputEventKey:
		var name :String= event.as_text()
		var is_pressed :bool= event.pressed
		if use_print_keyboard_input:
			print("Key event: ", event.as_text(), " pressed: ", event.pressed)
		if name == input_name_up:
			button_up = is_pressed
		elif name == input_name_right:
			button_right = is_pressed
		elif name == input_name_down:
			button_down = is_pressed
		elif name == input_name_left:
			button_left = is_pressed
		elif name == input_name_a:
			button_a = is_pressed
		elif name == input_name_b:
			button_b = is_pressed
		elif name == input_name_menu_left_select:
			button_menu_left_select = is_pressed
		elif name == input_name_menu_right_restart:
			button_menu_right_restart = is_pressed
		on_nes_input_updated.emit(button_up, button_right, button_down, button_left, button_a, button_b, button_menu_left_select, button_menu_right_restart)
	if listen_to_gamepad_button_input and event is InputEventJoypadButton:
		var name :String= "JoypadButton"+str(event.button_index)
		var is_pressed :bool= event.pressed
	
		if use_print_gamepad_button_input:
			print("Gamepad button event: ", event.button_index, " pressed: ", event.pressed)
		if is_button(event.button_index, input_gamepad_button_up):
			button_up = is_pressed
		elif is_button(event.button_index, input_gamepad_button_right):
			button_right = is_pressed
		elif is_button(event.button_index, input_gamepad_button_down):
			button_down = is_pressed
		elif is_button(event.button_index, input_gamepad_button_left):
			button_left = is_pressed
		elif is_button(event.button_index, input_gamepad_button_a):
			button_a = is_pressed
		elif is_button(event.button_index, input_gamepad_button_b):
			button_b = is_pressed
		elif is_button(event.button_index, input_gamepad_button_menu_left_select):
			button_menu_left_select = is_pressed
		elif is_button(event.button_index, input_gamepad_button_menu_right_restart):
			button_menu_right_restart = is_pressed
		on_nes_input_updated.emit(button_up, button_right, button_down, button_left, button_a, button_b, button_menu_left_select, button_menu_right_restart)


	if listen_to_gamepad_joystick_input and event is InputEventJoypadMotion:
		var name :String= "JoypadMotion"+str(event.axis)
		var value :float= event.axis_value
		if abs(value) > 0.1:	
			if use_print_gamepad_joystick_input:
				print("Gamepad motion event: ", event.axis, " value: ", event.axis_value)
			if event.axis== 0:
				joystick_left_state.x = value
			elif event.axis == 1:
				joystick_left_state.y = -value
			elif event.axis == 2:
				joystick_right_state.x = value
			elif event.axis == 3:
				joystick_right_state.y = -value

			var changed = false
			if (joystick_left_state.x > joystick_threshold or joystick_right_state.x > joystick_threshold) and not button_right:
				button_right = true
				changed = true
			if (joystick_left_state.x < -joystick_threshold or joystick_right_state.x < -joystick_threshold) and not button_left:
				button_left = true
				changed = true
			if (joystick_left_state.y > joystick_threshold or joystick_right_state.y > joystick_threshold) and not button_up:
				button_up = true
				changed = true
			if (joystick_left_state.y < -joystick_threshold or joystick_right_state.y < -joystick_threshold) and not button_down:
				button_down = true
				changed = true
			if (joystick_left_state.x < joystick_threshold and joystick_right_state.x < joystick_threshold) and button_right:
				button_right = false
				changed = true
			if (joystick_left_state.x > -joystick_threshold and joystick_right_state.x > -joystick_threshold) and button_left:
				button_left = false
				changed = true
			if (joystick_left_state.y < joystick_threshold and joystick_right_state.y < joystick_threshold) and button_up:
				button_up = false
				changed = true
			if (joystick_left_state.y > -joystick_threshold and joystick_right_state.y > -joystick_threshold) and button_down:
				button_down = false
				changed = true

			if changed:
				on_nes_input_updated.emit(button_up, button_right, button_down, button_left, button_a, button_b, button_menu_left_select, button_menu_right_restart)

```


Ecouter au scroll de la soursi:
``` gdscript
class_name SSD1306MouseScrollToScale
extends Node

signal on_scroll_up()
signal on_scroll_down()

@export var what_to_scale: Node3D
@export var scale_factor: float = 1.1

func _input(event: InputEvent) -> void:
	if event is InputEventMouseButton and event.button_index == MOUSE_BUTTON_WHEEL_UP and event.pressed:
		on_scroll_up.emit()
		if what_to_scale:
			what_to_scale.scale *= scale_factor
	elif event is InputEventMouseButton and event.button_index == MOUSE_BUTTON_WHEEL_DOWN and event.pressed:
		on_scroll_down.emit()
		if what_to_scale:
			what_to_scale.scale /= scale_factor
```




Un example de raycast depuis la camera

``` gdscript
class_name SSD1306MouseClickToNodeMove
extends Node3D

@export var main_camera: Camera3D
@export var what_to_move: Node3D

@export var use_collision_mask: bool = false
@export_flags_3d_physics var collision_mask: int = 1

@export var use_black_list: bool = false
@export var black_list_group: Array[StringName] = []

@export var use_white_list: bool = false
@export var white_list_group: Array[StringName] = []

@export var collide_with_bodies: bool = true
@export var collide_with_areas: bool = true

@export var use_on_click_move: bool = true
@export var use_on_pressing_move: bool = true

func _input(event: InputEvent) -> void:
	if use_on_click_move and event is InputEventMouseButton and event.pressed and event.button_index == MOUSE_BUTTON_LEFT:
		move_to_click(event.position)
	elif use_on_pressing_move and event is InputEventMouseMotion and Input.is_mouse_button_pressed(MOUSE_BUTTON_LEFT):
		move_to_click(event.position)
	if use_on_click_move and event is InputEventMouseButton and event.pressed and event.button_index == MOUSE_BUTTON_RIGHT:
		move_to_click(event.position)
	elif use_on_pressing_move and event is InputEventMouseMotion and Input.is_mouse_button_pressed(MOUSE_BUTTON_RIGHT):
		move_to_click(event.position)

func move_to_click(mouse_pos: Vector2) -> void:
	if not main_camera or not what_to_move:
		return

	var from: Vector3 = main_camera.project_ray_origin(mouse_pos)
	var direction: Vector3 = main_camera.project_ray_normal(mouse_pos)
	var to: Vector3 = from + direction * 2000.0  # Increased range

	var space_state := get_world_3d().direct_space_state

	var query := PhysicsRayQueryParameters3D.create(from, to)
	query.collide_with_bodies = collide_with_bodies
	query.collide_with_areas = collide_with_areas
	query.collision_mask = collision_mask if use_collision_mask else 0xFFFFFFF

	var result: Dictionary = space_state.intersect_ray(query)

	if not result or not result.has("collider"):
		return

	var collider = result.collider

	if use_black_list and not black_list_group.is_empty():
		for group in black_list_group:
			if collider.is_in_group(group):
				return

	if use_white_list and not white_list_group.is_empty():
		var in_white_list := false
		for group in white_list_group:
			if collider.is_in_group(group):
				in_white_list = true
				break
		if not in_white_list:
			return

	if collider is Area3D and not collider.monitoring:
		return

	what_to_move.global_position = result.position
```
