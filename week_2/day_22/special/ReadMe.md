
**Objectif de la journée**

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

``` gdscript
```


Filter by finding the first children that have the given name
``` gdscript
func search_in_children_node_by_name(node: Node) -> Node:
	var input: Node = node.find_child("KS4036Input", true, false)
	if input:
		return input
	return null
```


Filter by all the node having a given name
``` gdscript
func search_in_children_nodes_by_name(node: Node) -> Array[Node]:
	var results: Array[Node] = []
	for child in node.get_children():
		if child.name == search_for_script_by_name:
			results.append(child)
		if search_by_name_recusrively:
			var result = search_in_children_nodes_by_name(child)
			results += result
	return results
```

Filter by layermask
```
@export_flags_3d_physics var layer_mask_filter: int = 0xFFFFFFFF

func something():
	if use_layer_filter:
		if not (body.collision_layer & layer_mask_filter):
			return false

```

Filter by layermask
```
@export_flags_3d_physics var layer_mask_filter: int = 0xFFFFFFFF
@export var allow_group_to_interact: String = "interactable_group"

func something():

	if use_group_filter:
		if not body.is_in_group(allow_group_to_interact):
			return false

```


### White List

Same

### Black List


Same








------------------
--------------------
















#
#@export_group("Pre-Filter")
#@export_group("Pre-Filter / Layer")
#@export var use_layer_filter: bool = false
#@export_flags_3d_physics var layer_mask_filter: int = 0xFFFFFFFF
#@export_group("Pre-Filter / Group")
#@export var use_group_filter: bool = false
#@export var allow_group_to_interact: String = "interactable_group"
#@export_group("Pre-Filter / Scripts")
#@export var use_script_tag_interactive_source:bool=true
#
#@export var allow_if_have_this_script_as_node: Script
#
#@export_group("Pre-Filter / Scripts")
#@export var use_search_by_name: bool = false
#@export var search_for_script_by_name: String = "KS4036Input"
#@export var search_by_name_recusrively: bool = true
#@export var search_by_name_owned: bool = false




func _is_valid_target(body:Node3D):
	# you can still allow collision and check with your code
	#
	#if use_layer_filter:
		#if not (body.collision_layer & layer_mask_filter):
			#return false
#
	#if use_group_filter:
		#if not body.is_in_group(allow_group_to_interact):
			#return false
#
			#
	#if use_script_tag_interactive_source:
		#var has_children:Node= find_microbit_interactive_source(body)
		#if has_children==null:
			#return false	
	return true




#func find_microbit_interactive_source(node: Node) -> Node:
	#if node.get_script() == allow_if_have_this_script_as_node:
		#return node		
	#for child in node.get_children():
		#var result = find_microbit_interactive_source(child)
		#if result:
			#return result
	#return null


#func search_in_children_node_by_name(node: Node) -> Node:
	#var input: Node = node.find_child("KS4036Input", true, false)
	#if input:
		#return input
	#return null
#
#func search_in_children_nodes_by_name(node: Node) -> Array[Node]:
	#var results: Array[Node] = []
	#for child in node.get_children():
		#if child.name == search_for_script_by_name:
			#results.append(child)
		#if search_by_name_recusrively:
			#var result = search_in_children_nodes_by_name(child)
			#results += result
	#return results		























