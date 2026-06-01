

Addons pour rendre cet exercice plus facile :
https://github.com/EloiStree/2026_05_29_gdp_xr_ui_lab

200 nœuds dans Godot et UI :
https://youtu.be/tO2gthp45MA?t=1531

# UI et IDE

Notre but est d'apprendre à utiliser les interfaces utilisateur (UI) de Godot pour créer un éditeur de code en XR.

# La base

Pour créer une UI, nous pouvons créer une scène 2D.

![alt text](image-59.png)

Ajoutons-y un "Hello World" dans un Label.

![alt text](image-60.png)

Comme pour Unity, le layout est dirigé par deux valeurs :

* Anchor Offsets en pixels (orange)
* Anchor Points en pourcentage (vert)

Il vaut mieux éviter d'utiliser des pixels.
Utilisons plutôt des pourcentages.

Nous l'avons déjà fait dans la formation.

Ajoutons maintenant un bouton.

![alt text](image-61.png)

Vous pouvez voir que je l'ai mal placé.
Car je lui ai dit d'être à *n x n* pixels du coin gauche.

![alt text](image-62.png)

Il n'est pas toujours facile de placer nos boutons.

![alt text](image-63.png)

Utilisons un des outils les plus utiles pour aller vite en prototypage : le VBox.

![alt text](image-64.png)

Ou un HBox pour les placer horizontalement.

![alt text](image-65.png)

Nous voulons que cela s'étire.
Utilisons **Expand**.

![alt text](image-66.png)

HBox et VBox avec Expand permettent de réaliser la plupart des interfaces.

![alt text](image-67.png)

Si nous voulions créer une application au format téléphone, nous pourrions utiliser un AspectRatioContainer.

Mettons-le à 100 %.

![alt text](image-69.png)

Utilisons un ratio de `9.0/16.0`.

![alt text](image-70.png)

Ce qu'il nous faut, c'est un éditeur de code.

Restructurons donc un peu tout cela.

![alt text](image-71.png)

Ajoutons des numéros de ligne et un espace pour écrire du code.

![alt text](image-72.png)

Cela manque un peu de couleur.
Ajoutons-en.

```gdscript
extends Node

## The place for the player to write code to be run when requested.
@export var _code_edit:CodeEdit

func _ready() -> void:
	load_text_color_style()

func load_text_color_style():
	var highlighter := CodeHighlighter.new()

	# Keywords
	highlighter.keyword_colors = {
		"if": Color("ff7085"),
		"elif": Color("ff7085"),
		"else": Color("ff7085"),
		"for": Color("ff7085"),
		"while": Color("ff7085"),
		"match": Color("ff7085"),
		"break": Color("ff7085"),
		"continue": Color("ff7085"),
		"pass": Color("ff7085"),
		"return": Color("ff7085"),
		"class": Color("ff7085"),
		"class_name": Color("ff7085"),
		"extends": Color("ff7085"),
		"func": Color("ff7085"),
		"static": Color("ff7085"),
		"const": Color("ff7085"),
		"var": Color("ff7085"),
		"enum": Color("ff7085"),
		"signal": Color("ff7085"),
		"await": Color("ff7085"),
		"yield": Color("ff7085"),
		"assert": Color("ff7085")
	}

	# Built-in types
	highlighter.member_keyword_colors = {
		"int": Color("42ffc2"),
		"float": Color("42ffc2"),
		"bool": Color("42ffc2"),
		"String": Color("42ffc2"),
		"Array": Color("42ffc2"),
		"Dictionary": Color("42ffc2"),
		"Vector2": Color("42ffc2"),
		"Vector3": Color("42ffc2"),
		"Color": Color("42ffc2"),
		"Node": Color("42ffc2"),
		"Object": Color("42ffc2")
	}

	# General token colors
	highlighter.number_color = Color("a1ffe0")
	highlighter.symbol_color = Color("abc9ff")
	highlighter.function_color = Color("57b3ff")
	highlighter.member_variable_color = Color("c6a0ff")

	# Regions
	highlighter.add_color_region("\"", "\"", Color("ffd942"), false)
	highlighter.add_color_region("'", "'", Color("ffd942"), false)
	highlighter.add_color_region("#", "", Color("7a7a7a"), true)

	_code_edit.syntax_highlighter = highlighter
```

![alt text](image-73.png)

Permettons à l'utilisateur d'agrandir le texte.

```gdscript
func increase_text_size():
	_code_edit.add_theme_font_size_override(
		"font_size",
		_code_edit.get_theme_font_size("font_size") + 1
	)

func reduce_text_size():
	_code_edit.add_theme_font_size_override(
		"font_size",
		_code_edit.get_theme_font_size("font_size") - 1
	)
```

Ajoutons deux boutons pour utiliser ces méthodes.

![alt text](image-74.png)

Nous pouvons l'afficher, mais comment l'exécuter ?

Allons chercher un code préparé pour l'occasion :

* [ui_lab_run_code_with_target.gd](https://github.com/EloiStree/2026_05_29_gdp_xr_ui_lab/blob/4a912a941cffeeb54acfdb3ac4bec4e5c3337d03/core/run_code_edit_with_target/script/ui_lab_run_code_edit_with_target_node.gd)
* [ui_lab_run_code_edit_with_target_node.gd](https://github.com/EloiStree/2026_05_29_gdp_xr_ui_lab/blob/4a912a941cffeeb54acfdb3ac4bec4e5c3337d03/core/run_code_edit_with_target/script/ui_lab_run_code_edit_with_target_node.gd)

Vous pouvez regarder cette vidéo si vous souhaitez comprendre le code suivant :

https://www.youtube.com/watch?v=hob2zVfCavc



``` gdscript
func unload_current_code():
	on_destroy_previous_node_holding_code_start.emit(_created_node_holding_code)
	if _created_node_holding_code:
		## if it existe. kill it. I means... lets is free 
		_created_node_holding_code.queue_free()
		_created_node_holding_code = null
	on_destroy_previous_node_holding_code_end.emit()		


func is_url(text:String):
	var t = text.strip_edges()
	return t.begins_with("http://") or t.begins_with("https://")
	
## I am the method that load the text given as code of the player to run as Godot script in a new node.
func load_and_run_text_as_godot_script(code:String):
	if is_url(code):
		on_url_to_handle_by_third_party_script_detected.emit(code)
		return
	if not code.contains("extends "):
		code = "extends Node\n  "+code
		
	## When we start we need to destroy the previous one.
	unload_current_code()
	## code cant be loaded like that. you need to load from file
	## we can create the file in folde of our application
	
	if _unique_code_file_name=="":
		_unique_code_file_name = str(get_instance_id())+".gd"
	var script_path: String = "user://"+_unique_code_file_name
	## print(script_path)
	## to see where it is store in the end
	print(ProjectSettings.globalize_path(script_path))
	var file_connection =FileAccess.open(script_path, FileAccess.WRITE)
	if file_connection:
		file_connection.store_string(code)
		file_connection.close()
	else:
		push_error("File was not created")
		return
	
	# lets try to execute it now.
	var script: Script = ResourceLoader.load(
		script_path,
		"GDScript",
		ResourceLoader.CACHE_MODE_IGNORE
	)

	if not script is GDScript:
		push_error("That not a Godot Script")
		on_fail_to_load_code.emit(code)
		return
	
	## we need for that a node
	var node :Node =  Node3D.new() if _create_node_as_node_3d else Node.new()
	# we have a new node but not yet in the scene
	node.set_script(script)
	# he has our code 
	node.set_process(true)
	# he now use _process(delta)
	node.set_physics_process(true)
	# in case we need it later
	
	## now we add it in the scene
	_created_node_holding_code = node
	if _where_to_run_code:
		_where_to_run_code.add_child(node)
	else:
		add_child(node)
	_notify_to_created_node_the_target()
	on_created_node.emit(node)
	
```







Si vous ajoutez le script : [ui_lab_run_code_edit_with_target_node.gd](https://github.com/EloiStree/2026_05_29_gdp_xr_ui_lab/blob/4a912a941cffeeb54acfdb3ac4bec4e5c3337d03/core/run_code_edit_with_target/script/ui_lab_run_code_edit_with_target_node.gd)

![alt text](image-75.png)

Vous avez un premier IDE fonctionnel qui cible un objet.

Il nous faudrait donc un objet.

Ajoutons un Character Controller.

![alt text](image-76.png)

Mais pour cela, il nous faut un CanvasLayer pour afficher notre IDE.

![alt text](image-77.png)

![alt text](image-78.png)

Ajoutons un script à notre CharacterBody3D.

```gdscript
extends CharacterBody3D

const SPEED = 5.0
const JUMP_VELOCITY = 4.5

func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var input_dir := Input.get_vector("ui_left", "ui_right", "ui_up", "ui_down")
	var direction := (transform.basis * Vector3(input_dir.x, 0, input_dir.y)).normalized()

	if direction:
		velocity.x = direction.x * SPEED
		velocity.z = direction.z * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)
		velocity.z = move_toward(velocity.z, 0, SPEED)

	move_and_slide()
```

Modifions-le pour qu'il soit utilisable par un développeur.

```gdscript
extends CharacterBody3D

const SPEED = 5.0
const JUMP_VELOCITY = 4.5

@export var joystick: Vector2

func set_joystick(joystick_given: Vector2):
	joystick = joystick_given

func jump():
	if is_on_floor():
		velocity.y = JUMP_VELOCITY

func _physics_process(delta: float) -> void:
	if not is_on_floor():
		velocity += get_gravity() * delta

	var input_dir := joystick
	var direction := (transform.basis * Vector3(input_dir.x, 0, -input_dir.y)).normalized()

	if direction:
		velocity.x = direction.x * SPEED
		velocity.z = direction.z * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)
		velocity.z = move_toward(velocity.z, 0, SPEED)

	move_and_slide()
```

```gdscript
extends Node

func _ready():
	print("Hello World")

func _on_received_target(target: Node):
	target.set_joystick(Vector2(-1, 1))
```

![alt text](image-79.png)

Appuyez sur **Run** et vous devriez avoir un cube qui bouge.

Pas nécessairement dans la bonne direction, mais il bouge.

Vous pouvez utiliser ce personnage pour la suite de l'exercice.

Mais si vous préférez utiliser une voiture que j'ai préparée, vous pouvez déposer la voiture suivante :

![alt text](image-80.png)

`prefab_two_wheels_car.tscn`

Et y ajouter une caméra.

Vous disposez de ces méthodes pour contrôler la voiture :

```gdscript
func set_wheels(left:float,right:float):
func set_left_wheel_percent_power(percent_power11: float) -> void:
func set_right_wheel_percent_power(percent_power11: float) -> void:
func get_front_wheel_left_distance() -> float
func get_front_wheel_right_distance() -> float
func get_left_line_sensor_color() -> Color:
func get_right_line_sensor_color() -> Color:

func set_screen_128x64_to(array_1d_128x64:Array[bool]):
func print_text(array:Array[bool], text:String, letter_color:bool=true, use_background:bool=true, top_left_text_corner:Vector2i=Vector2i.ZERO)

func get_car_id() -> int:
func get_car_position() -> Vector3:
func get_car_rotation() -> Quaternion:
func get_car_euler() -> Vector3:
```

Exemple :

```gdscript
extends Node

func _ready():
	print("Hello Mini Car")

func _on_received_target(target: Node):
	target.set_wheels(-0.1, 0.2)
```

Vous devriez maintenant avoir quelque chose qui ressemble à ceci :

![alt text](image-58.png)

Notre but est de créer un IDE.

Essayons d'insérer du code.

Ajoutons ce bout de code et utilisons-le avec un bouton.

```gdscript
extends Node

@export var code_edit:CodeEdit
@export_multiline() var code_to_insert:String
@export var button:Button

func _ready() -> void:
	button.button_down.connect(insert_inspector_text)

func insert_inspector_text():
	insert_text_next_line(code_to_insert)

func insert_text_next_line(text:String):
	s_insert_text_next_line(code_edit, text)

static func s_insert_text_next_line(editor: CodeEdit, text: String) -> void:
	var line := editor.get_caret_line()

	if editor.has_selection():
		line = editor.get_selection_to_line()

	var target := line + 1
	var count := editor.get_line_count()

	if target >= count:
		editor.insert_line_at(count, "")
		target = count

	var indent := editor.get_indent_level(target)

	editor.set_caret_line(target)
	editor.set_caret_column(indent)
	editor.insert_text_at_caret(text + "\n")
```

![alt text](image-81.png)



----------

## Et la XR du coup

Je n'ai pas eu le temps d'écrire un tutoriel.

Car j'ai expérimenté sur le sujet ce week-end.

Je vous propose donc de le faire ensemble, à la main.

## Workshop

Nous avons deux objectifs pour la journée d'aujourd'hui :

- Apprendre à utiliser les interfaces utilisateur (UI) de Godot.
- Expérimenter quelles interfaces fonctionnent bien en XR 😋



-----------------------------

-------------------------

# UI to know


Container
![alt text](image.png)
https://youtu.be/tO2gthp45MA?t=1536


Ratio
![alt text](image-1.png)
https://youtu.be/tO2gthp45MA?t=1545


Box Container
![alt text](image-2.png)
https://youtu.be/tO2gthp45MA?t=1560


Center Container
![alt text](image-3.png)
https://youtu.be/tO2gthp45MA?t=1577

Flow Container
![alt text](image-4.png)
https://youtu.be/tO2gthp45MA?t=1583

Grid
![alt text](image-5.png)
https://youtu.be/tO2gthp45MA?t=1596

Vertical container
![alt text](image-6.png)
https://youtu.be/tO2gthp45MA?t=1610

Margin
![alt text](image-7.png)
https://youtu.be/tO2gthp45MA?t=1626  

Panel
![alt text](image-8.png)
https://youtu.be/tO2gthp45MA?t=1636

Scroll
![alt text](image-9.png)
https://youtu.be/tO2gthp45MA?t=1646

Tab 😋
![alt text](image-10.png)
https://youtu.be/tO2gthp45MA?t=1662


Label 
![alt text](image-11.png)
https://youtu.be/tO2gthp45MA?t=1691

Rich Text Label
![alt text](image-12.png)

https://youtu.be/tO2gthp45MA?t=1697


Color Rect
![alt text](image-13.png)
https://youtu.be/tO2gthp45MA?t=1705

Texture Rect
![alt text](image-14.png)
https://youtu.be/tO2gthp45MA?t=1711

Video Stream  
![alt text](image-15.png)  
https://youtu.be/tO2gthp45MA?t=1717  


Separator
![alt text](image-16.png)
https://youtu.be/tO2gthp45MA?t=1724

Nine Patch Rect
![alt text](image-17.png)
https://youtu.be/tO2gthp45MA?t=1743


Button  
![alt text](image-18.png)  
https://youtu.be/tO2gthp45MA?t=1755  

Texture Button  
![alt text](image-19.png)  
https://youtu.be/tO2gthp45MA?t=1776  

Link Button
![alt text](image-20.png)
https://youtu.be/tO2gthp45MA?t=1789

Check Button
![alt text](image-21.png)
https://youtu.be/tO2gthp45MA?t=1799

Menu Button
![alt text](image-22.png)
https://youtu.be/tO2gthp45MA?t=1805

Option Button
![alt text](image-23.png)
https://youtu.be/tO2gthp45MA?t=1817

Color Picker Button
![alt text](image-24.png)
https://youtu.be/tO2gthp45MA?t=1824


Line Editor
![alt text](image-25.png)
https://youtu.be/tO2gthp45MA?t=1832

Code Editor
![alt text](image-26.png)
https://youtu.be/tO2gthp45MA?t=1839

Range
![alt text](image-27.png)
https://youtu.be/tO2gthp45MA?t=1857

Slider
![alt text](image-31.png)
https://youtu.be/tO2gthp45MA?t=1894

Progress Bar
![alt text](image-28.png)
https://youtu.be/tO2gthp45MA?t=1863


SpinBox
![alt text](image-29.png)
https://youtu.be/tO2gthp45MA?t=1874

Scroll Bar
![alt text](image-30.png)
https://youtu.be/tO2gthp45MA?t=1890

Item List
![alt text](image-32.png)
https://youtu.be/tO2gthp45MA?t=1907

Menu Bar  
![alt text](image-33.png)  
![alt text](image-34.png)  
https://youtu.be/tO2gthp45MA?t=1914


Tab Bar
![alt text](image-35.png)
https://youtu.be/tO2gthp45MA?t=1925


Reference Rect
![alt text](image-36.png)
https://youtu.be/tO2gthp45MA?t=1935


Tree node
![alt text](image-37.png)
https://youtu.be/tO2gthp45MA?t=1945


GraphNode (WIP)
![alt text](image-38.png)


Touch Screen Button
![alt text](image-39.png)
https://youtu.be/tO2gthp45MA?t=1964

----------------


Layout
![alt text](image-40.png)
https://youtu.be/tO2gthp45MA?t=1989


Localisation
![alt text](image-41.png)
https://youtu.be/tO2gthp45MA?t=2067

ToolTip
![alt text](image-42.png)
https://youtu.be/tO2gthp45MA?t=2075

Focus
![alt text](image-43.png)
https://youtu.be/tO2gthp45MA?t=2079


Shortcut
![alt text](image-45.png)
https://youtu.be/tO2gthp45MA?t=2183

Theme
![alt text](image-46.png)
https://youtu.be/tO2gthp45MA?t=2198


Viewport
[![alt text](image-47.png)](https://youtu.be/tO2gthp45MA?t=2230)
https://youtu.be/tO2gthp45MA?t=2230

SubViewport
![alt text](image-48.png)
https://youtu.be/tO2gthp45MA?t=2251


AudioStreamPlayer
![alt text](image-56.png)
https://youtu.be/tO2gthp45MA?t=2361

HttpRequest
![alt text](image-57.png)
https://youtu.be/tO2gthp45MA?t=2390


Window
[![alt text](image-49.png)](https://youtu.be/tO2gthp45MA?t=2262)
https://youtu.be/tO2gthp45MA?t=2262



Confirm Dialog
![alt text](image-50.png)
https://youtu.be/tO2gthp45MA?t=2281

File Dialog
[![alt text](image-51.png)](https://youtu.be/tO2gthp45MA?t=2291)
https://youtu.be/tO2gthp45MA?t=2291

Pop Up Menu
![alt text](image-52.png)
https://youtu.be/tO2gthp45MA?t=2303


PopupMenu
![alt text](image-53.png)
https://youtu.be/tO2gthp45MA?t=2312


Canvas Item
![alt text](image-54.png)
https://youtu.be/tO2gthp45MA?t=2328

Canvas Layer
![alt text](image-55.png)


-------------


