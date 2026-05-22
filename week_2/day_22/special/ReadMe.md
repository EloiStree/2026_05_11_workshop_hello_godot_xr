
**Objectif de la journée**
   
[<img width="994" height="530" alt="image" src="https://github.com/user-attachments/assets/61d7fbff-23ee-402a-b87b-84234cbb4517" />](https://www.creativebloq.com/how-to/use-gravity-sketch)       
https://www.creativebloq.com/how-to/use-gravity-sketch    
  

- Créer un cursor
  - Changer la taille du cursor avec le joystick du `XRController3D` (right)
- Allumer et éteindre des lumières avec le Trigger
- Détruire des objets avec `queue_free()` en appuyant sur B
  - Créer des objets aléatoires avec le bouton A
- Changer la couleur d’un `MeshInstance3D` aléatoirement
- Code Builder
  - Quand un node entre, vérifier s’il possède un `CodeHolder`
    - Générer un texte en fusionnant tous les `CodeHolder`

---

**Contexte**

Je suis nouveau sur Godot.

Concernant **Area3D**, voici ma compréhension actuelle :

- Utilise les **Layers** si tu travailles sur un game jam ou un projet à long terme.
- Utilise les **Groups** si tu es pressé et que tu n’as pas beaucoup de collisions.
- Utilise un **script** si tu veux simplement lister des éléments spécifiques.
- Dès que les solutions ci-dessus ne conviennent plus, personnalise ta recherche (comme un cuisinier qui improvise).

**Proposition** : Créer un listener de collision personnalisé pour Area3D qui maintient un `Array[Node3D]` des objets actuellement en collision. Tu pourras ensuite appliquer des filtres empilables sur cette liste.


-------------------------------------------------



**Les bullets** : Elles sont très rapides, avec des téléportations de 1,5 mètre ou plus par frame.

La plupart des moteurs de jeu ne gèrent pas bien les collisions d’objets très rapides.

Vous pouvez utiliser un **Character Controller** à la place d’un **RigidBody** pour vos balles.

Dans tous les cas, il est recommandé de mettre en place une vérification de sécurité supplémentaire.

Deux approches principales :

1. Envoyer un **raycast** entre l’ancienne et la nouvelle position à chaque frame (nous verrons cela en détail ensuite).

2. Générer un **collider en capsule** (ou une Area) qui relie le point précédent et le point actuel.

Vous pouvez trouver un exemple ici :  
→ https://github.com/EloiStree/2026_05_21_gdp_area_for_xr   
→ https://github.com/EloiStree/2026_04_12_gdp_nerf_dart   



---------------------------



## Clicker sur un Area3D


Petit code qui peut vous être utile pour déboguer.
Il permet de cliquer sur une Area3D


```gdscript
class_name AreaXrToolTouchableArea3D
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


-------------------------


## Le curseur

Ce que l'on voudrait c'est une forme qui nous dit j'ai autant d'éléments en moi.

Pour ça il faut pouvoir traverser les objets. 
Donc pas un RigidBody ou StaticCollider ici.

On va utiliser un Area3D.

Celui-ci a besoin d'une forme donc un ColliderShape3D


Pour pouvoir écouter cette zone on peut hériter de celle-ci.
Ou on peut s'abonner à des événements.


``` gdscript
@export var listen_at_area_collision:bool=true
@export var listen_at_body_collision:bool=true
func _ready():
	if listen_at_area_collision:
		observed_area.area_entered.connect(_on_area_entered)
		observed_area.area_exited.connect(_on_area_exited)
	if listen_at_body_collision:
		observed_area.body_entered.connect(_on_body_entered)
		observed_area.body_exited.connect(_on_body_exited)
```

Attention comme vous pouvez voir, il y a une différence entre body et area.

Ça reste la même chose. Nous on veut juste détecter des notes qui entrent et qui sortent.

Créez une méthode commune

``` gdscript

signal on_node_entering_valide(node:Node)
signal on_node_exiting_valide(node:Node)
signal on_nodes_array_updated(nodes:Array[Node])

@export var in_zone_nodes:Array[Node]
func _add_node(node: Node3D):
	if node == null or in_zone_nodes.has(node):
		return
	in_zone_nodes.append(node)
	_remove_null_in_array_and_check()
	on_node_entering_valide.emit(node)
	on_nodes_array_updated.emit(in_zone_nodes)

func _remove_node(node: Node3D):
	if node == null or not in_zone_nodes.has(node):
		return
	in_zone_nodes.erase(node)
	_remove_null_in_array_and_check()
	on_node_exiting_valide.emit(node)
	on_nodes_array_updated.emit(in_zone_nodes)
```



Allons voir du côté des enters et sorties

``` gdscript
func _on_body_entered(body: Node3D) -> void:
	print("Body entered: ", body)
	_add_node(body)

func _on_body_exited(body: Node3D) -> void:
	print("Body exited: ", body)
	_remove_node(body)

func _on_area_entered(area: Area3D) -> void:
	print("Area in ",area)
	if area ==null:
		return
	_add_node(area)

func _on_area_exited(area: Area3D) -> void:	
	print("Area out",area)
	if area ==null:
		return
	_remove_node(area)
```


Si un objet est détruit avant de sortir de notre curseur il sera null.
Retirons-les


``` gdscript

func _remove_null_in_array_and_check():
	for i in range(in_zone_nodes.size() - 1, -1, -1):
		if in_zone_nodes[i]==null:
			in_zone_nodes.remove_at(i)
	check_if_active()
	on_nodes_array_updated.emit(in_zone_nodes)

func _ready():
	var cleanup_timer := Timer.new()
	cleanup_timer.wait_time = clean_for_null_interval_in_seconds
	cleanup_timer.autostart = true
	cleanup_timer.timeout.connect(_remove_null_in_array_and_check)
	add_child(cleanup_timer)
```

Et comme on veut pouvoir utiliser nos area pour avoir des nodes mais aussi pour faire des boutons:

``` gdscript
signal on_active_state_changed(is_active:bool)
signal on_down()
signal on_up()
@export var has_nodes:bool=false
func is_pressed()->bool:
	return in_zone_nodes.size()>0

func is_released()->bool:
	return not is_pressed()

func check_if_active()-> bool:
	_set_as_active(in_zone_nodes.size()>0)
	return has_nodes

func _set_as_active(value_on:bool):
	var changed:bool= value_on != has_nodes
	has_nodes = value_on
	if changed:
		if has_nodes:
			on_down.emit()
		else :
			on_up.emit()
		on_active_state_changed.emit(value_on)
```



## Filtrage

Ok, on arrive à avoir un groupe de Node avec les collisions de notre Area.
Maintenant, il nous faut trouver les nodes qui nous intéressent.

Fort probable que ce soit dans les enfants.

``` gdscript
static func get_node_and_all_childrens_not_recusively(node:Node) -> Array[Node]:
	var nodes:Array[Node] = []
	nodes.append(node)
	for child in node.get_children():
		nodes.append(child)
	return nodes


static func get_all_nodes_in_childrens_recursive(node:Node)->Array[Node]:
	var nodes:Array[Node] = []
	var queue:Array[Node] = [node]
	
	while queue.size() > 0:
		var current = queue.pop_front()
		for child in current.get_children():
			nodes.append(child)
			queue.append(child)	
	return nodes
```

Maintenant que nous avons les enfants, on veut les isoler par LayerMask:

En utilisant une liste blanche:

``` gdscript
@export_flags_3d_physics var layer_mask_white_list: int = 0xFFFFFFFF
func push_in_nodes(nodes: Array[Node]) -> void:
	## UNTESTED YET
	var result: Array[Node] = []	
	for node in nodes:
		if not node is CollisionObject3D:
			continue

		var collision_object: CollisionObject3D = node as CollisionObject3D
		if collision_object.collision_layer & layer_mask_white_list != 0:
			result.append(node)	
	notify_nodes_filtered(result)
```

Ou en utilisant une liste noire:

``` gdscript
@export_flags_3d_physics var layer_mask_black_list: int = 0x00000000
func push_in_nodes(nodes: Array[Node]) -> void:
	## UNTESTED YET
	var result: Array[Node] = []
	for node in nodes:
		if not node is CollisionObject3D:
			continue
		var collision_object: CollisionObject3D = node as CollisionObject3D
		if collision_object.collision_layer & layer_mask_black_list == 0:
			result.append(node)
	notify_nodes_filtered(result)
```



On peut faire la même chose avec le concept de groupe de Godot 😋


``` gdscript
@export var white_list_group:Array[String]
func push_in_nodes(nodes:Array[Node]):
	# NOT TESTED YET
	var result:Array[Node] = []
	for node in nodes:
		for group in white_list_group:
			if node.is_in_group(group):
				result.append(node)
				break
	notify_nodes_filtered(result)
```

La liste noire
``` gdscript
@export var black_list_group:Array[String]
func push_in_nodes(nodes:Array[Node]):
	# NOT TESTED YET
	var result:Array[Node] = []
	for node in nodes:
		var is_blacklisted = false
		for group in black_list_group:
			if node.is_in_group(group):
				is_blacklisted = true
				break
		if not is_blacklisted:
			result.append(node)
	notify_nodes_filtered(result)
```


Comme je l'expliquais dans le cours initial d'aujourd'hui, ma technique favorite est de tagger par script.


Par exemple, on fixe ce script sur la main gauche du joueur.


``` gdscript
class_name LeftFingerXrTag
extends Node
```

Puis on essaie de trouver les nodes avec ce script:
``` gdscript
@export var scripts_white_list:Array[Script]=[]
func push_in_nodes(nodes:Array[Node]):
	var result:Array[Node] = []
	for node in nodes:
		var script:Script = node.get_script()
		if script in scripts_white_list:
			result.append(node)
	notify_nodes_filtered(result)
	
```


Maintenant, il nous reste à émettre ces nodes à des codes qui savent quoi en faire.
On peut préfiltrer pour eux si l'on veut.

``` gdscript
signal on_mesh_instance_3d_filtered(meshes:Array[MeshInstance3D])
@export var last_mesh_filtered:Array[MeshInstance3D] = []
@export var last_nodes_filtered:Array[Node] = []
func push_in_nodes(nodes:Array[Node]):
	var result:Array[MeshInstance3D] = []
	var node_result:Array[Node] = []

	for n in nodes:
		if n is MeshInstance3D:
			result.append(n)
			node_result.append(n)

	last_mesh_filtered = result
	last_nodes_filtered = node_result

	on_mesh_instance_3d_filtered.emit(result)
	notify_nodes_filtered(node_result)	
```



Ok on a une liste de nodes avec leurs enfants maintenant quoi ?
Bah c'est là que l'on s'amuse.  
On les stocke et on les traite.  
`get_nodes()` va chercher les nodes que vous venez de stocker

Par example:
``` gdscript

## Allons chercher le material surface pour changer sa couleur
func _set_color_surface_to_node(node:Node, color:Color):	
	var children = get_all_nodes_in_childrens_recursive(node)
	children.append(node)
	var mesh_instances := find_mesh_instances_3d_in_nodes(children)
	for mesh in mesh_instances:
		var mesh_instance: MeshInstance3D = mesh
		var material: StandardMaterial3D = mesh_instance.get_surface_override_material(0)
		if material:
			material.albedo_color = color	

## Et l on donnne une couleur aleatoire a tout les nodes dans la selection.
func group_random_color():
	for node in get_nodes():
		if node:
			var random_color = Color(randf(), randf(), randf())
			_set_color_surface_to_node(node, random_color)


## Ou on les detruits ;)
func group_queue_free():
	for node in get_nodes():
		if node:
			node.queue_free()
```


### Modding de la sélection

Vous l'aurez compris, je suis sur Godot car il permet de modder et de laisser le joueur coder dans mon application 😋


On vérifie que les nodes ont une méthode avec un nom spécifique et lui "broadcaster" une info


``` gdscript
func group_broadcast_method_call(method_name:String):
	for node in get_all_nodes_in_childrens_recursive_from_source():
		if node and node.has_method(method_name):
			node.call(method_name)

func group_broadcast_method_call_with_one_value(method_name:String, value):
	for node in get_all_nodes_in_childrens_recursive_from_source():
		if node and node.has_method(method_name):	
			node.call(method_name, value)
```

Un peu compliqué pour les débutants de la classe, on peut s'abonner au signal des nodes

``` gdscript
func group_broadcast_signal_hook_connect(signal_name:String, callback :Callable):
	for node in get_all_nodes_in_childrens_recursive_from_source():
		if node and node.has_signal(signal_name):
			node.connect(signal_name, callback)
```


Mais mon préféré, on peut fusionner du code ;)
En créant un script avec la méthode `get_code()`

``` gdscript
class_name AreaXrCodeHolder
extends Node

@export_multiline var code:String

func set_code(new_code:String):
	code = new_code

func get_code() -> String:
	return code

```

Puis en la cherchant dans tous les enfants.
On liste et on assemble.
(Noter que on pourrait le faire de haut en bas sur le Y ou du plus proche au plus loin.)


``` gdscript
func group_broadcast_get_code_as_array() -> Array[String]:
	var nodes: Array[Node] = get_all_nodes_in_childrens_recursive_from_source()
	nodes = remove_double(nodes)
	var codes: Array[String] = []
	for node in nodes:
		if not is_instance_valid(node):
			continue
		if node.has_method("get_code"):
			var code: String = node.call("get_code")
			if code: 
				codes.append(code)
	return codes

func print_code_in_childrens():
	var text :String= get_broadcast_get_code()
	print("Code:\n" + text)

func get_broadcast_get_code()-> String:
	var codes: Array[String] = group_broadcast_get_code_as_array()
	return "\n".join(codes)

func get_broadcast_get_code_with_split(split:String="")-> String:
	var codes: Array[String] = group_broadcast_get_code_as_array()
	return split.join(codes)
```






----------------


