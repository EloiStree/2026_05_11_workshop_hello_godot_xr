**Objectif : Script de tagging**

Les layers et groupes, c’est pratique… jusqu’au moment où ça devient inutilisable dès que tu veux faire une vraie boîte à outils réutilisable.

Une approche que j’aime bien consiste à ne pas dépendre uniquement des groupes, mais à utiliser un système de tags via scripts, que d’autres scripts peuvent ensuite rechercher et exploiter.



**Area3D**   
L’Area3D filtre les collisions et envoie une liste d’objets à un script.
Ce script applique ensuite un second filtre : il conserve uniquement les objets possédant un tag, que ce soit sur le node lui-même, ses enfants ou son parent.


**Approche “Static Singleton”**  
Le tag est enregistré dans une liste statique globale de l’application, ou dans une resource singleton partagée.

Ensuite, le script de détection vérifie simplement si l’objet est compatible avec le tag recherché.
Exemple: trouver les lumières dans une scène pour des capteurs de résistance a la lumière.   




**Exemple : Un tag pour être découvert**

Le principe ici est de faire un script de tag à trouver lorsque l’on recherche dans les enfants d’un nœud.

Une fois trouvé, on peut lui transmettre une couleur.
Le script, via un signal, ajoute une couche d’abstraction et délègue la gestion de cette couleur à d’autres systèmes.

```gdscript
class_name CrocoColorSetterTagRgb
extends Node

signal on_color_at_ready(color: Color)
signal on_color_updated(color: Color)

@export var color_resource: CrocoColorResourceRgb

func _ready():
	# await get_tree().create_timer(0.01).timeout
	if color_resource:
		on_color_at_ready.emit(color_resource.color)

func set_color(color: Color):
	color_resource.color = color
	on_color_updated.emit(color)
```

Maintenant, il nous faut retrouver ces nœuds contenant ce script.

Ce script ne sait pas quoi faire de la couleur : il la transmet simplement via des signaux à d’autres scripts, qui eux seront chargés de l’appliquer ou de la traiter sur le nœud concerné.

``` gdscript

class_name NodeListFromCollidableArea3D
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
signal on_node_entering_valide(node:Node)
signal on_node_exiting_valide(node:Node)


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

@export var allow_if_have_this_script_as_node: Script
func find_microbit_interactive_source(node: Node) -> Node:
	if node.get_script() == allow_if_have_this_script_as_node:
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
		on_node_entering_valide.emit(area)
		
		#print("Hey you touched my tralala ", in_zone_nodes.size())

func _on_area_exited(area: Area3D) -> void:	
	if not _is_valid_target(area):
		return
	if in_zone_nodes.has(area):
		on_node_entering_valide.emit(area)
		in_zone_nodes.erase(area)	
		check_if_active()
		on_node_exiting_valide.emit(area)
		#print("Leaving soo sooon ?", in_zone_nodes.size())
```



