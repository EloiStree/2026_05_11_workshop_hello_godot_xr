
## Note : Le 18, il y avait une grève

Comme la majorité des personnes étaient absentes ce jour-là, nous nous sommes davantage concentrés sur le fait de faire fonctionner Godot sur PC et Quest et terminer la video de Brackeys.

L’objectif de cette journée,du 18 , était de découvrir les inputs.
Aujourd’hui,de base, nous allions setup Godot XR nous concentrer sur les inputs du Quest 3.

Du code est disponible [ici](https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr/blob/main/week_1/day_12/2_after_noon/ReadMe.md).
 
De fait, aujourd’hui, ceux qui n’ont pas encore appris à configurer Godot pour la XR sont invités à s’y essayer.     
Pour ceux qui ont déjà mis en place Godot en XR, l’objectif sera plutôt de se concentrer sur les systèmes d’inputs.     

Demain, la journée entière sera consacrée à la pratique de Godot XR Tool autour de l’interaction avec des objets 3D (pickup) à travers un tutoriel vidéo Godot.         
Je vous laisserai du temps ce mardi pour revoir et pratiquer à nouveau un sujet de votre choix :     
les inputs, la configuration (setup), ou tout autre point que vous souhaitez approfondir.  


-----------------

## Matin : Passons de la 2D à la 3D




Prenons un cube et déplaçons-le :

* de gauche à droite
* de l’avant vers l’arrière
* avec des vitesses de rotation et de déplacement

**Junior :** Tourner, avancer, reculer. C'est deja bien 😋   
**Medior :** Essayez de faire tourner sur le pivot de la roue à gauche et à droite.    
**Senior :** Observez le KS4036 avec des vitesses de rotation de -1.0 à 1.0.    


-------------------------------

Voici les meshes de la voiture de notre exercice, le KS4036.   

C’est de la 3D et du dépliage de développeur ☕😉   
Mais fait avec passion et pied à coulisse.   

[<img width="572" height="362" alt="image" src="https://github.com/user-attachments/assets/48ce8ed5-4565-42da-9316-04ae356427a6" />](https://github.com/user-attachments/files/27905192/ks_4036_meshes.zip)   
Download: 
- [ks_4036_meshes.zip](https://github.com/user-attachments/files/27905192/ks_4036_meshes.zip)   
- [micro_bit_shape.zip](https://github.com/user-attachments/files/27905238/micro_bit_shape.zip)
  
Parlons de pied à coulisse si vous en voulez un :   
[caliper_not_prepared.zip](https://github.com/user-attachments/files/27905289/caliper_not_prepared.zip)   
(Je ne l’ai pas encore nettoyé)   

_Je vous avais préparé de nombreux assets 3D ici pour apprendre à coder :_   
_[https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors](https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors)_   
_Avant de me rappeler que nous n’avons que 14 jours éparpillés (3 + 4 + 5 + 2)._   




--------------

Une voiture à deux ou quatre roues vendue sur Amazon est composée de quatre boutons afin de réduire les coûts.
**Up Left, Up Right, Down Left, Down Right**


Si l’on traduisait cela avec un clavier, cela ressemblerait à ceci :


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

L’avantage de ce code, c’est qu’il est portable.
Le désavantage, c’est qu’il ne prend pas en compte les manettes ni les autres types d’inputs que le clavier.
On pourrait utiliser les *Input Map Actions* de Godot.



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

Inspirons-nous de ce code pour parler un peu de rotation et de translation.  

### Avec Transform3D  

Code d'un drone basique :  
``` gdscript
extends Node

var joystick_left: Vector2 = Vector2.ZERO
var joystick_right: Vector2 = Vector2.ZERO

@export var what_node_to_affect: Node3D
@export var move_speed: float = 5.0
@export var rotation_speed: float = 90.0
@export var vertical_speed: float = 3.0

func set_drone_joysticks(joystick_left_up_rotation: Vector2, joystick_right_move: Vector2) -> void:
	joystick_left = joystick_left_up_rotation
	joystick_right = joystick_right_move
	# print("Test received " + str(joystick_left) + " " + str(joystick_right))

func _physics_process(delta: float) -> void:
	if not what_node_to_affect:
		return

	# --- Rotation (yaw, left stick X) ---
	what_node_to_affect.rotate_y(-joystick_left.x * deg_to_rad( rotation_speed) * delta)
	what_node_to_affect.translate(Vector3(0, joystick_left.y * vertical_speed * delta, 0) )
	what_node_to_affect.position += what_node_to_affect.basis.x* joystick_right.x * move_speed * delta
	what_node_to_affect.position -= what_node_to_affect.basis.z* joystick_right.y * move_speed * delta
```

`extends Node` :  
Je suis un objet dans la scène Godot.  

`var joystick_left: Vector2`  
Je suis une zone mémoire nommée joystick_left, pas visible par le designer.  
Ca manque de `_` `var _joystick_left: Vector2` pour dire que la variable est privée.   
`_` Je ne devrai pas etre modifiable par les autres developpeurs et etre cacher du designer.   

`var joystick_left: Vector2 = Vector2.ZERO`  
J'ai la valeur par défaut de Vector(0,0) quand je suis créé.  

`@export var what_node_to_affect: Node3D`  
J'ai besoin que le game designer me dise ce que je dois tourner et bouger.  

`@export var move_speed: float = 5.0`  
J'ai besoin d'une vitesse de déplacement qui peut changer à tout moment mais sera à 5 par défaut.  

`@export var rotation_speed: float = 90.0`  
D'une vitesse de rotation et je vous laisse deviner que c'est en degrés.  
Il manque de la documentation ou un nom de variable plus parlant.  

`@export var vertical_speed: float = 3.0`  
On aura besoin d'une vitesse différente pour monter et descendre.  
Nous, on verra une voiture donc on n'en aura pas besoin.  

`func set_drone_joysticks(joystick_left_up_rotation: Vector2, joystick_right_move: Vector2) -> void:`  
Pour bouger le drone, je demande au designer ou développeur de me fournir deux joysticks.      
⚠️ Noter que je n’utilise pas d’input dans ce script pour laisser le choix aux autres membres de l’équipe.     

`joystick_left_up_rotation: Vector2`  
Celui de gauche en premier avec une valeur x et y.  

`joystick_right_move: Vector2`  
Celui de droite en second avec une valeur x et y.  

` -> void:`  
Je n'ai pas besoin de donner un résultat en retour, je ne retourne rien : `void`.  

`joystick_left = joystick_left_up_rotation`  
Je prends ce que l'on m'a donné et je le stocke dans notre espace mémoire.  

`joystick_right = joystick_right_move`  
Pareil, on sauvegarde pour plus tard.  

`#print("Test received " + str(joystick_left) + " " + str(joystick_right))`  
Pour mieux déboguer et m'assurer que mon code fonctionne, je veux pouvoir voir les deux valeurs dans la console.  

`print(...)`  
Je demande d'afficher dans la console du développeur un texte.  

`str(...)`  
Comme j'ai besoin d'une string, je demande de la convertir.  

Voir : `_to_string` https://docs.godotengine.org/en/stable/classes/class_object.html  

`" "+ str() +""`  
C'est ce que l'on appelle de la concaténation : ajouter des bouts de string ensemble.  

On aurait pu utiliser `"".join([ , , ])`  
Ou les formats `var text = "Joystick: %s | %s" % [joystick_left, joystick_right]`  

`func _physics_process(delta: float) -> void:`  
À chaque dessin de l'image au moment dédié à la physique du jeu, allons bouger.  

`delta: float`  
Le game engine nous donne combien de temps a pris la dernière `frame` pour que l'on prédise la suivante.  
Delta hors game engine, ça veut juste dire la différence entre deux valeurs.  

`if not what_node_to_affect:`  
Si le game designer a oublié de nous donner un objet à bouger.  

`return`  
Je me casse et je ne fais rien.  
Je pourrais aussi lui casser les noix en utilisant push_error("T'as pas oublié un truc ?")  

`what_node_to_affect.rotate_y(-joystick_left.x * deg_to_rad( rotation_speed) * delta)`  
Ce que l'on veut, c'est tourner notre objet avec l'intensité du joystick.  

`node.rotate_y`  
On veut faire tourner notre objet sur son axe Y (vertical) avec Euler.  

`rotation_speed`  
Notre rotation est en degrés mais Rotate Y est en radians.  
Comment je le sais ? RTFM : https://docs.godotengine.org/en/stable/classes/class_node3d.html  

`deg_to_rad( rotation_speed)`  
Prenons l'angle de rotation humainement lisible et transformons-le pour les maths en radians.  

`deg_to_rad( rotation_speed) * delta`  
Par contre ce n'est pas 90 degrés mais 90 x 0.016+- secondes.  
On le multiplie par la dernière frame reçue.  

`joystick_left.x`  
Mais il nous faut prendre la volonté du joueur via le joystick.  
X c'est de gauche à droite et Y de haut en bas (ou de bas en haut).  

`-joystick_left.x`  
Comme Euler va à gauche niveau rotation mais que notre joystick demande d'aller à droite,  
on inverse la valeur du joystick et donc notre équation.  

Tadaam, on a demandé de tourner selon la volonté du joueur.  

`what_node_to_affect.translate(Vector3(0, joystick_left.y * vertical_speed * delta, 0) )`  
Nous allons faire bouger notre drone de haut en bas avec une méthode faite pour nous.  

`joystick_left.y * vertical_speed * delta`  
Selon la vitesse, le temps qui passe et l'intensité du joystick, donne-nous une valeur de déplacement.  

`Vector3(0, distance, 0)`  
Créons une direction qui va vers le dessus avec la distance de déplacement.  

`node.translate(Vector3(0, distance , 0))`  
Déplaçons notre objet sur Y (vertical) par rapport à lui-même.  

`node.position += self.basis.z *distance`  
On aurait pu l'écrire comme ceci.  

`node.basis.z` donne-nous la direction verticale localement de mon node.  

`node.position +=` Ajoute-moi la valeur qui suit à ma position locale.  

`basis.z * distance` Notons que l'on peut multiplier un `Vector3()` avec un `float`.  

Allons voir du côté horizontal.  

`what_node_to_affect.position += what_node_to_affect.basis.x* joystick_right.x * move_speed * delta`  

Essayez de deviner ce que fait cette phrase vu mes commentaires précédents.  

`what_node_to_affect.position -= what_node_to_affect.basis.z* joystick_right.y * move_speed * delta`  

Ici une petite nuance `-=`... Selon le développeur ou le mathématicien, "z" n'est pas illustré du même côté.  

Les couleurs et directions XYZ des axes sont purement conventionnelles. 😋  

En Godot il y a du code qui respecte le Z de Godot pour aller vers l'avant et d'autres l'inverse.  

Ici j'ai pris l'inverse de Godot car c'est ce que j'utilisais en cours de maths en secondaire.  

Et voilà, vous arrivez à tourner et bouger un objet avec les données du game designer 🍻  

Petit problème...  
Vous traversez les murs...  

Pour gagner en performance, on ne calcule pas tous les mouvements de la même manière.  
Si votre objet ne doit pas traverser un autre, cela demande plus de maths.  

On utilise donc un CharacterBody3D ou CharacterBody2D  

### Avec character Controller

Code d'un drone basique:
``` gdscript
extends CharacterBody3D

var joystick_left: Vector2 = Vector2.ZERO
var joystick_right: Vector2 = Vector2.ZERO

@export var move_speed: float = 5.0
@export var rotation_speed: float = 270
@export var vertical_speed: float = 3.0

func set_drone_joysticks(joystick_left_up_rotation: Vector2, joystick_right_move: Vector2) -> void:
	joystick_left = joystick_left_up_rotation
	joystick_right = joystick_right_move
	# print("Test received " + str(joystick_left) + " " + str(joystick_right))

func _physics_process(delta: float) -> void:
	rotate_y(-joystick_left.x * deg_to_rad(rotation_speed) * delta)
	var new_velocity: Vector3 = Vector3.ZERO
	new_velocity.y = joystick_left.y * vertical_speed
	var direction = (transform.basis.x * joystick_right.x) + (-transform.basis.z * joystick_right.y)
	if direction.length() > 0.01:
		direction = direction.normalized()
		new_velocity.x = direction.x * move_speed
		new_velocity.z = direction.z * move_speed

	velocity = new_velocity
	move_and_slide()
```

Ici, plutôt que de le bouger nous-mêmes, on va préparer le mouvement avec `velocity` et lui demander de faire les calculs avec `move_and_slide()`     

`var direction = (transform.basis.x * joystick_right.x) + (-transform.basis.z * joystick_right.y)`   
Petite nuance ici : on crée une flèche vers la droite et une vers l’avant, puis on les additionne pour obtenir la direction.   

`direction = direction.normalized()`    
Pour s’assurer que celle-ci reste dans un rayon de 1, on simplifie les calculs avec `normalized()`.   

Sinon, en soi, rien ne change.    

Notez que la rotation n’est pas prise en compte dans le character controller.   


## À vous de faire ;)

Vous savez maintenant comment écouter le clavier, et vous avez le code de mouvement et de rotation.  
Essayez d’adapter cela pour une voiture pilotable avec deux floats ou quatre boutons.   



-----------------

--------------------

## Pour la suite 

Pour les exercices qui suivent, vous pouvez utiliser votre code de cette exercice ou piochez dans les suivants.

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

# Plus d’informations   

## Écoutons les inputs :  

Des exemples de code Git pour écouter les inputs :       
- https://github.com/EloiStree/2026_01_03_gdp_input_to_primitive  
-https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr/blob/main/week_1/day_12/2_after_noon/ReadMe.md

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
