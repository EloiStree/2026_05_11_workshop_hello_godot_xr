
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
