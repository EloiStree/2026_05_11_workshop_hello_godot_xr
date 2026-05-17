
## Fin de journée : Débogueur SSD1306

Nous avons appris à utiliser le SSD1306, car cela nous permettra de déboguer en XR.

Créez un code permettant d’afficher :

* **les inputs des manettes du Quest**
* Optionnel :

  * les inputs du clavier au format `Input`
  * les inputs du gamepad via `Input`
  * la position et le delta de la souris
* Challenge :

  * Utiliser les caractères Unicode pour détecter :

    * un [barcode scanner](https://www.amazon.com.be/s?k=scan+code)
    * un [badge NFC](https://www.amazon.com.be/s?k=badge+nfc)
  * Cela permettra de charger des scènes via un identifiant unique.

Par facilité, vous pouvez aussi ne pas utiliser le SSD1306 et utiliser simplement un `Label3D`.

(Nico peut utiliser le `CanvasLayer` avec les UI de Godot s’il veut 😉)

Le but ici est de vous laisser le temps de vous familiariser avec le système `Input` de votre choix.

---

Pour lire les inputs du Quest 3, il vous faudra vous référer aux `XRController3D` `left_hand` et `right_hand`.

Si vous ne faites pas un mod, vous pouvez simplement faire :

```gdscript
@export var left: XRController3D
@export var right: XRController3D
```

Si c’est votre projet et que vous décidez d’utiliser une convention de nommage avec **Unique Name** :

<img width="736" height="494" alt="image" src="https://github.com/user-attachments/assets/78554ad3-f061-477e-8a78-8924bb25fcf7" />

Alors vous pouvez écrire ceci :

`Access As Unique Name`

```gdscript
@onready var left: XRNode3D = $LeftController
@onready var right: XRNode3D = $RightController
@onready var origin: XRNode3D = $XRCamera3D
@onready var camera: XRNode3D = $XROrigin3D
```

Voici un exemple :

```gdscript
extends Node

@onready var xr_origin_from_unique_path: XROrigin3D = $%XROrigin3D

@export var left_controller_path: NodePath = "XROrigin3D/LeftController"
@export var right_controller_path: NodePath = "XROrigin3D/RightController"
@export var camera_path: NodePath = "XROrigin3D/XRCamera3D"

@export_group("Found")
@export var xr_origin: XROrigin3D
@export var xr_camera: XRCamera3D
@export var xr_left_controller: XRController3D
@export var xr_right_controller: XRController3D

var xr_interface: XRInterface

func _ready() -> void:

	xr_origin = xr_origin_from_unique_path
	
	if xr_origin == null:
		push_error("You are not in an XR scene with Unique Name Access on Origin")
		return

	# Get references safely
	xr_camera = get_node_or_null(camera_path) as XRCamera3D
	xr_left_controller = get_node_or_null(left_controller_path) as XRController3D
	xr_right_controller = get_node_or_null(right_controller_path) as XRController3D

	# Optional: fallback to direct children search
	if not xr_camera:
		xr_camera = xr_origin.get_node_or_null("XRCamera3D") as XRCamera3D

	if not xr_left_controller:
		xr_left_controller = xr_origin.get_node_or_null("LeftController") as XRController3D

	if not xr_right_controller:
		xr_right_controller = xr_origin.get_node_or_null("RightController") as XRController3D
```

Dans le code d’un **mod layer chargé depuis le web**, vous ne connaissez pas forcément la convention de nommage choisie dans le projet dans lequel vous arrivez.

En revanche, vous savez que vous avez besoin :

* d’un `XRController3D` gauche
* d’un `XRController3D` droit
* d’une caméra XR
* d’un `XROrigin3D`

Vous pouvez donc scanner la scène de manière plus brutale afin de les trouver automatiquement à votre arrivée.

```gdscript
class_name ScanForXrControllerAndOrigin
extends Node

@export_group("Found")
@export var xr_origin: XROrigin3D
@export var xr_camera: XRCamera3D
@export var xr_left_controller: XRController3D
@export var xr_right_controller: XRController3D

func _ready() -> void:
	# If not all references are set in the editor,
	# try to find them automatically

	if not (xr_origin and xr_camera and xr_left_controller and xr_right_controller):
		var all_nodes := get_all_nodes_of_scene()
		find_xr_elements_in_nodes(all_nodes)

	# Optional validation
	if not xr_origin:
		push_warning("XR Origin not found!")

	if not xr_camera:
		push_warning("XR Camera not found!")

	if not xr_left_controller:
		push_warning("XR Left Controller not found!")

	if not xr_right_controller:
		push_warning("XR Right Controller not found!")

func get_all_nodes_of_scene() -> Array[Node]:
	var nodes: Array[Node] = []

	_collect_nodes_recursive(get_tree().root, nodes)

	return nodes

func _collect_nodes_recursive(
	node: Node,
	collected: Array[Node]
) -> void:

	if not node:
		return

	collected.append(node)

	for child in node.get_children():
		_collect_nodes_recursive(child, collected)

func find_xr_elements_in_nodes(nodes: Array[Node]) -> void:
	for node in nodes:

		if not node:
			continue

		if not xr_origin and node is XROrigin3D:
			xr_origin = node

		elif not xr_camera and node is XRCamera3D:
			xr_camera = node

		elif node is XRController3D:

			var controller := node as XRController3D

			if controller.tracker == "left_hand" \
			or controller.name.to_lower().contains("left"):

				if not xr_left_controller:
					xr_left_controller = controller

			elif controller.tracker == "right_hand" \
			or controller.name.to_lower().contains("right"):

				if not xr_right_controller:
					xr_right_controller = controller

		if xr_origin \
		and xr_camera \
		and xr_left_controller \
		and xr_right_controller:
			break
```
