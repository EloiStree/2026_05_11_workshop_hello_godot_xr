
## Matin : Passons de la 2D à la 3D

Prenons un cube et déplaçons-le :

* de gauche à droite
* de l’avant vers l’arrière
* avec des vitesses de rotation et de déplacement

**Junior :** Tourner, avancer, reculer. C'est deja bien 😋
**Medior :** Essayez de faire tourner sur le pivot de la roue à gauche et à droite.
**Senior :** Observez le KS4036 et déplacez-le à l’aide de la trigonométrie, avec des vitesses de rotation de -1.0 à 1.0.


Une voiture a deux ou a quatre roue vendu sur amazons, cest compose de 4 bouttons pour faire des economies.
Up Left, Up Right, Down Left , Down Right.


Si on traduisait cela avec un clavier ca ressemblerai a cela:

```gdscript
class_name SkidCarDefaultInput
extends Node

@export var is_enabled: bool = true
# --- 4 motor control booleans ---
@export var button_left_front_on: bool = false
@export var button_right_front_on: bool = false
@export var button_left_back_on: bool = false
@export var button_right_back_on: bool = false

# --- Key bindings (customizable in Inspector) ---
@export var key_left_front: Key = KEY_W
@export var key_right_front: Key = KEY_E
@export var key_left_back: Key = KEY_S
@export var key_right_back: Key = KEY_D

# --- Individual signals for each motor ---
signal on_left_front(is_on: bool)
signal on_right_front(is_on: bool)
signal on_left_back(is_on: bool)
signal on_right_back(is_on: bool)

# --- Internal tracking of pressed states ---
var pressed_keys: Dictionary = {}

func _ready() -> void:
	# Initialize all keys as not pressed
	pressed_keys = {
		key_left_front: false,
		key_right_front: false,
		key_left_back: false,
		key_right_back: false
	}

func _input(event: InputEvent) -> void:
	if not is_enabled:
		return
	if event is InputEventKey:
		if event.keycode in pressed_keys:
			if pressed_keys[event.keycode] != event.pressed:
				pressed_keys[event.keycode] = event.pressed
				_update_booleans_and_emit(event.keycode, event.pressed)

func _update_booleans_and_emit(keycode: Key, is_pressed: bool) -> void:
	match keycode:
		key_left_front:
			button_left_front_on = is_pressed
			on_left_front.emit(is_pressed)
		key_right_front:
			button_right_front_on = is_pressed
			on_right_front.emit(is_pressed)
		key_left_back:
			button_left_back_on = is_pressed
			on_left_back.emit(is_pressed)
		key_right_back:
			button_right_back_on = is_pressed
			on_right_back.emit(is_pressed)

```

L avantage de ce code c est qu il est portatif.
Le desavantage c est qu il prend pas en compte les manettes et autres inputs que le clavier.
On pourrait utilise le Input map action de Godot.


``` gdscript
class_name SkidCarDefaultInputMap
extends Node


@export var is_enabled: bool = true
@export var action_left_front: String = "left_front"
@export var action_right_front: String = "right_front"
@export var action_left_back: String = "left_back"
@export var action_right_back: String = "right_back"

var button_left_front_on := false
var button_right_front_on := false
var button_left_back_on := false
var button_right_back_on := false

signal on_left_front(is_on: bool)
signal on_right_front(is_on: bool)
signal on_left_back(is_on: bool)
signal on_right_back(is_on: bool)


func set_left_front_on(is_on: bool) -> void:
	if button_left_front_on != is_on:
		button_left_front_on = is_on
		on_left_front.emit(is_on)
func set_right_front_on(is_on: bool) -> void:
	if button_right_front_on != is_on:
		button_right_front_on = is_on
		on_right_front.emit(is_on)
func set_left_back_on(is_on: bool) -> void:
	if button_left_back_on != is_on:	
		button_left_back_on = is_on
		on_left_back.emit(is_on)
func set_right_back_on(is_on: bool) -> void:
	if button_right_back_on != is_on:
		button_right_back_on = is_on
		on_right_back.emit(is_on)

func _process(_delta: float) -> void:
	if not is_enabled:
		return
	set_left_front_on(get_value(action_left_front))
	set_right_front_on(get_value(action_right_front))
	set_left_back_on(get_value(action_left_back))
	set_right_back_on(get_value(action_right_back))
	

func get_value(name:String) -> bool:
	if InputMap.has_action(name):
		return Input.is_action_pressed(name)
	return false

```



--------------------------- 


## Base du mouvement

### Avec Transform3D


``` gdscript

```

### Avec character Controller

``` gdscript

```


## Quater et deux roues ?

- KS4036 Mini Carr
  - Info: https://github.com/EloiStree/HelloTwoWheelsCarKS4036
  - Code: https://github.com/EloiStree/2025_10_19_gdp_kid_toy_skid_steering
- OVNI: https://github.com/EloiStree/2025_04_15_gdp_kid_toy_ovni_code
  - Script: [V1](https://github.com/EloiStree/2025_04_15_gdp_kid_toy_ovni_code/blob/950a0a4557273e3f50f1a7e450406db14f785fc4/scripts/core/move/v000/ovni__move_drone_from_two_joysticks.gd) [V2](https://github.com/EloiStree/2025_04_15_gdp_kid_toy_ovni_code/blob/950a0a4557273e3f50f1a7e450406db14f785fc4/scripts/core/move/v001/ovni__move_drone_character_from_two_joysticks.gd)
- Drone:
  - [FPV script](https://github.com/EloiStree/2025_04_15_gdp_kid_toy_ovni_code/blob/950a0a4557273e3f50f1a7e450406db14f785fc4/scripts/core/move/fpv_move_basic_character.gd) 
  - [Tello script](https://github.com/EloiStree/2025_04_15_gdp_kid_toy_ovni_code/blob/950a0a4557273e3f50f1a7e450406db14f785fc4/scripts/core/move/tello_move_lerp_character.gd) 



---------------------------







---------------------------------

## Ecoutons aux Input:

Des git code pour ecouter aux inputs:    
https://github.com/EloiStree/2026_01_03_gdp_input_to_primitive  


Ecouter a des characters `1234567890-=~!@#$%^&*()_+][;.,'/{:<>?"}...`:   
``` gscript
func _input(event):
	if event is InputEventKey and event.pressed and event.unicode > 0:
		var char := char(event.unicode)
```

Ecouter a des inputs de differents appareils:
``` gdscript
func _input(event: InputEvent) -> void:
	if event is InputEventMouseMotion or event is InputEventMouseButton:
		return
	if event is InputEventKey:
		return
	var device_id := event.get_device()
	var device_name := Input.get_joy_name(device_id)
	var description := event.as_text()
	print("Input Event from device %d (%s): %s" % [device_id, device_name, description])

```

Afficher la liste des joysticks et gamepad disponible:
``` gdscript
  var text := "Joystick List:\n"
	var joypads := Input.get_connected_joypads()
	for i in range(joypads.size()):
		var device_id := joypads[i]
		var name := Input.get_joy_name(device_id)
		var axis_count: int = get_axis_count(device_id)
		var button_count: int = get_button_count(device_id)
		text += "Joystick %d: %s | Axis Count: %d | Button Count: %d\n" % [
			i + 1,
			name,
			axis_count,
			button_count
		]
```

<img width="1301" height="387" alt="image" src="https://github.com/user-attachments/assets/1b3b8476-1ba3-4d82-8213-515954d8b834" />


<img width="1726" height="1092" alt="image" src="https://github.com/user-attachments/assets/9ee2d231-3a48-45fa-91b6-d8b2484cc079" />



Listen to Keyboard:
``` gdscript

class_name ListenToKeyboard
extends Node

signal on_first_time_key_on(key_code: int)
signal on_key_on_off(key_code: int, value_is_down: bool)
signal on_key_on_off_with_label(key_code: int, label_name: String, value_is_down: bool)

# Tracks the current state of any key
var _key_states := {}
var keys_id_to_label_name: Dictionary = {}

@export var dictionary_key_id_to_label_name_to_load: Dictionary[int,String] = {}
@export var default_key_to_label_to_load: Array[KeyboardToolbox.IntegerIdToName] = []

func _ready() -> void:
	for key_to_label in default_key_to_label_to_load:
		if key_to_label != null:
			set_label_of_key_id(key_to_label.integer_value, key_to_label.name_value)
	for key_id in dictionary_key_id_to_label_name_to_load.keys():
		if key_id !=null:
			var label_name = dictionary_key_id_to_label_name_to_load[key_id]
			if label_name != null:
				set_label_of_key_id(key_id, label_name)

func _input(event):
	if event is InputEventKey:
		var key_code = event.keycode
		var is_down = event.pressed

		# Initialize the key state if not tracked yet
		if not _key_states.has(key_code):
			_key_states[key_code] = false

		# Detect first time the key is pressed
		if is_down and not _key_states[key_code]:
			on_first_time_key_on.emit(key_code)

		if _key_states[key_code] != is_down:
			_key_states[key_code] = is_down
			on_key_on_off.emit( key_code, is_down)
			on_key_on_off_with_label.emit( key_code, get_label_of_key_id(key_code), is_down)

func is_key_down(key_code: int) -> bool:
	return _key_states.get(key_code, false)
	
func set_label_of_key_id(key_id: int, label_name: String) -> void:
		keys_id_to_label_name[key_id] = label_name

func get_label_of_key_id(key_id: int) -> String:
	return keys_id_to_label_name.get(key_id, "")
```
