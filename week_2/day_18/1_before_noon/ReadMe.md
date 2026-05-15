
## Avant-midi : Meta Input

Les inputs de Meta ne sont pas standardisés pour l’InputMap de Godot.
Nous devons créer un script qui communique avec les contrôleurs 3D gauche et droit.

Essayons de lire tous les inputs disponibles et fonctionnels dans Godot (sur PC et Android).


Use Label3D to display text in Godot XR Scene:
to add

Doc:
https://docs.godotengine.org/en/latest/tutorials/xr/xr_action_map.html


bool button
float axis and triger
Vector2 joysticks
Pose position

`The Pose type defines a spatially tracked input. Multiple "pose" inputs are available in OpenXR: aim, grip and palm. Your XRController3D node is automatically positioned based on the pose action assigned to pose property of this node. More about poses later.`


Je n aime pas le principe des paths car ca empeche de renommer les elements.
Mais noter que vous pouvez utiliser ceci et tagger l origin.

``` gdscript
# Helper variables to keep our code readable
@onready var origin_node = $XROrigin3D
@onready var camera_node = $XROrigin3D/XRCamera3D
@onready var neck_position_node = $XROrigin3D/XRCamera3D/Neck


```


``` gdscript
## Listen to a button
static func get_right_trigger_touch() -> bool:
	if not _static_right_hand: return false
	for name in ["trigger_touch"]:
		if _static_right_hand.is_button_pressed(name):
			return true
	return false
```

```gdscript
## Listen to the joystick
static func get_right_joystick_2d_value() -> Vector2:
	if not _static_right_hand:
		return Vector2.ZERO
	
	for name in ["primary", "thumbstick", "joystick", "secondary"]:
		var value = _static_right_hand.get_vector2(name)
		if value.length() > 0.01:   # small deadzone
			return value
	return Vector2.ZERO
```

``` gdscript
## Listen to an axis
## Returns trigger squeeze value on the left controller (0.0 = not pressed, 1.0 = fully pressed)
static func get_trigger_left_value() -> float:
	if not _static_left_hand: return 0.0
	return _static_left_hand.get_float("trigger")  # Default Godot OpenXR action name
```



<img width="1635" height="1114" alt="image" src="https://github.com/user-attachments/assets/6bcbe0a0-ea1d-4fe1-864b-7df362067eb2" />
<img width="1218" height="746" alt="image" src="https://github.com/user-attachments/assets/21ec5d54-7f41-4f32-ac47-57746173d236" />
<img width="1134" height="645" alt="image" src="https://github.com/user-attachments/assets/93dde9ba-bfed-4bb8-ba8a-2eea663c6f84" />



---------

Origin XR

https://docs.godotengine.org/en/latest/tutorials/xr/xr_room_scale.html


-------

Hand tracking

https://docs.godotengine.org/en/stable/tutorials/xr/openxr_hand_tracking.html#introduction

