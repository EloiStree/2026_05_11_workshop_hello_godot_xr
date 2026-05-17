

**Objectif : utiliser un Area3D pour détecter un contact**

Godot Engine propose avec **Area3D** un outil simple mais puissant pour gérer les interactions en 3D, notamment la détection de collisions et d’objets dans une zone.

Ce système peut être utilisé pour construire rapidement des “boîtes à outils” en XR.
On réutilisera ce code à plusieurs endroits pour notre jeu de la semaine prochaine.

L’objectif de cet exercice est de se familiariser avec Area3D dans Godot, puis d’explorer les **layers** et **groups** afin de filtrer précisément les objets détectés par la zone.

---

### Mise en scène

Attachez un **Nerf Gun** à votre main gauche et un **sabre laser** (un cylindre en *unlit*) à votre main droite.

---

### Exercice 1.1 : Des cibles

Ajoutez des cubes proches du joueur et des cylindres plus éloignés pour représenter des cibles.

Lorsque les cibles sont touchées, elles doivent disparaître.

---

### Extensions (à réaliser après réussite de l’exercice 1.1)

**Exercice 1.2 : filtrage par groupe**
Ne détecter les objets que s’ils appartiennent à un groupe spécifique.

**Exercice 1.3 : filtrage par layer**
Ne détecter les objets que s’ils sont sur un layer précis.

**Exercice 1.4 : filtrage par script**
Ne détecter que les objets possédant un script attendu.

**Exercice 1.5 : vérification dans les enfants**
Ne détecter que si un objet ou ses enfants possèdent un script spécifique.

**Exercice 1.6 : vérification dans les parents**
Ne détecter que si un objet ou ses parents possèdent un script spécifique.



------------------

<img width="500" height="279" alt="image" src="https://github.com/user-attachments/assets/aa860242-36e8-4c19-b517-ee542dd02b63" />

------------------

**Une solution:**   
``` gdscript  

class_name MicroBitAreaCollidableArea3D
extends Area3D

@export var is_active:bool=false
@export var in_zone_nodes:Array[Node3D]

@export var clean_for_null_interval_in_seconds:float=0.1
@export_group("Use filtering ")
@export var use_layer_filter: bool = false
@export_flags_3d_physics var layer_mask_filter: int = 0xFFFFFFFF
@export var use_group_filter: bool = false
@export var allow_group_to_interact: String = "interactable_group"
@export var use_script_tag_interactive_source:bool=true

signal on_active_state_changed(is_active:bool)
signal on_down()
signal on_up()

func is_pressed()->bool:
	return in_zone_nodes.size()>0

func is_released()->bool:
	return not is_pressed()

func check_if_active():
	_set_as_active(in_zone_nodes.size()>0)

func _set_as_active(value_on:bool):
	var changed:bool= value_on != is_active
	is_active = value_on
	if changed:
		if is_active:
			#print('Down')
			on_down.emit()
		else :
			#print('Up')
			on_up.emit()
		#print('Changed')
		on_active_state_changed.emit(value_on)
			
	
func _ready():
	area_entered.connect(_on_area_entered)
	area_exited.connect(_on_area_exited)
	var cleanup_timer := Timer.new()
	cleanup_timer.wait_time = clean_for_null_interval_in_seconds
	cleanup_timer.autostart = true
	cleanup_timer.timeout.connect(_remove_null_in_array_and_check)
	add_child(cleanup_timer)

func _remove_null_in_array_and_check():
	#TODO; SHOULD WORK NOT TESTED
	for i in range(in_zone_nodes.size() - 1, -1, -1):
		if in_zone_nodes[i]==null:
			in_zone_nodes.remove_at(i)
	check_if_active()
	


func _is_valid_target(body:Node3D):
	# you can still allow collision and check with your code
	#
	if use_layer_filter:
		if not (body.collision_layer & layer_mask_filter):
			return false

	if use_group_filter:
		if not body.is_in_group(allow_group_to_interact):
			return false

			
	if use_script_tag_interactive_source:
		var has_children:Node= find_microbit_interactive_source(body)
		if has_children==null:
			return false	
	return true

func find_microbit_interactive_source(node: Node) -> Node:
	if node is MicroBitInteractiveSource:
		return node

	for child in node.get_children():
		var result = find_microbit_interactive_source(child)
		if result:
			return result

	return null


func _on_area_entered(area: Area3D) -> void:
	if not _is_valid_target(area):
		return
	if not in_zone_nodes.has(area):
		in_zone_nodes.append(area)
		check_if_active()
		#print("Hey you touched my tralala ", in_zone_nodes.size())

func _on_area_exited(area: Area3D) -> void:	
	if not _is_valid_target(area):
		return
	if in_zone_nodes.has(area):
		in_zone_nodes.erase(area)	
		check_if_active()
		#print("Leaving soo sooon ?", in_zone_nodes.size())

```


Detect a touch on Area3D
``` gdscript
class_name MicroBitTouchableArea3D
extends Area3D

signal on_down()
signal on_up()

@export var use_print_debug:bool =true
@export var pressing_state:bool = false

func _ready() -> void:
	input_ray_pickable =true
	
func is_pressed()->bool:
	return pressing_state==true
func is_released()->bool:
	return pressing_state==false

func _set_pressing_state(value_on:bool):
	var changed := value_on != pressing_state
	pressing_state= value_on
	if changed:
		if value_on:
			on_down.emit()
		else:
			on_up.emit()

func _input_event(camera, event, position, normal, shape_idx):
	# print(event)
	if (event is InputEventMouseButton or event is InputEventScreenTouch) and event.pressed:
		if use_print_debug:
			print("Pressed:",str(event)," Named:",name)
		_set_pressing_state(true)
	if (event is InputEventMouseButton or event is InputEventScreenTouch) and not event.pressed:
		if use_print_debug:
			print("Released:",str(event)," Named:",name)
		_set_pressing_state(false)
		

```
