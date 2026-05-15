
## Matin : Passons de la 2D à la 3D

Prenons un cube et déplaçons-le :

* de gauche à droite
* de l’avant vers l’arrière
* avec des vitesses de rotation et de déplacement

**Medior :** Essayez de faire tourner sur le pivot de la roue à gauche et à droite.
**Senior :** Observez le KS4036 et déplacez-le à l’aide de la trigonométrie, avec des vitesses de rotation de -1.0 à 1.0.





[<img width="896" height="672" alt="image" src="https://github.com/user-attachments/assets/15affd35-dcd6-4d2e-adff-78d5e017688c" />](https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/266)
https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/266


## Quater et deux roues ?

https://github.com/EloiStree/HelloTwoWheelsCarKS4036
https://github.com/EloiStree/2025_10_19_gdp_kid_toy_skid_steering/tree/main

---------------------------




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
