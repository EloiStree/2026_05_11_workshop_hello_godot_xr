[<img width="720"  alt="image" src="https://github.com/user-attachments/assets/7b5d7830-dab5-49d9-b09a-879a6697de94" />](https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr)        

« Ce n’est pas avec neuf femmes qu’on peut faire un enfant en un mois. »  
- Coder prend du temps
- Coder demande des pauses pour réfléchir
- Coder demande parfois de “dormir dessus” pour laisser les idées mûrir

Clean Code: [Video](https://github.com/EloiStree/HelloCleanCode)
- “Clean code always looks like it was written by someone who cares.”
- "Always code as if the person who ends up maintaining your code will be a violent psychopath who knows where you live." — John Woods

"La valeur d’un livre ne se mesure pas au fait de l’avoir lu une fois, mais au fait d’avoir envie de le relire."     
Lié à ce sujet: Un code propre se reconnaît au code qui a été retiré.   


**En gros:**   
Le but de la semaine n’est pas forcément d’aller vite ni de produire un maximum.   
L’objectif, c’est surtout de pratiquer le travail en équipe.  
Le code que vous écrirez cette semaine sera utilisé par d’autres la semaine prochaine 😅🍻   


---


## Planning :

* 26–29 mai: Préparer le KS4036
* 1–2 juin: Revoir les Nodes et l’UI
* 3–5 juin: Game Jam

### But de la semaine :

[<img width="720"  alt="image" src="https://github.com/user-attachments/assets/0965456b-a5a9-4db8-b8ad-bd35532d51c9" />](https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr)  

Préparer une boîte à outils en groupe sur Git simulant un KS4036, sans avoir le jeu de la Jam en tête.
Le but est de l’utiliser durant la Jam.   
Et qu'il soit theoriquement utilisable sur [Godot Lib](https://godotengine.org/asset-library/asset).

Essayer de rester le plus « propre » possible, dans la mesure du possible.

---

Comme dit la semaine passée, si je continue dans la matière, vous n’aurez rien retenu par manque de temps donné pour pratiquer.

Comme vous avez tous un niveau différent :

* de ceux qui n’ont jamais codé ;
* à ceux qui ont fait des études d’informatique et/ou de gaming.

Nous allons nous répartir les tâches de l’atelier de cette semaine.

Essayez de trouver des exercices simples qui aident à apprendre dans le contexte du projet pour les débutants :

* Input
* LED
* Lumière
* Raycast
* ...

Essayez de trouver des exercices plus complexes qui font avancer le projet pour les plus avancés :

* Modding
* Mouvement du KS4036
* Snapper du code sur la voiture
* S’assurer de lier les scripts ensemble
* S’assurer que votre boîte à outils sera utilisable le 3 juin
* Bonus : Faire bouger la voiture avec la rotation de la main
* Savoir charger/changer du code sur la voiture


**Scoper :**   
Posez-vous la question de ce qui est nécessaire et de ce qui est optionnel (nice to have).  
Ne commencez pas par ce qui est optionnel tant que ce qui est nécessaire ne fonctionne pas.   


---

### On a vu :

* Les bases de Godot avec Brackeys 2D
* Comment faire un projet XR avec Godot
* Les Input Maps
* Les Input Sets pour la XR
* Un peu de Godot avec le principe des tableaux
* Comment attraper des objets
* Un peu de 3D
* Un peu de rotation
  * Comment créer un joystick avec un tilt/roll
* Comment setup un casque
* Un peu d’Area3D
* On a dû créer de temps à autre des matériaux

Il reste deux sujets dont on a besoin pour pouvoir passer à la suite :

* LED RGB
* Raycast
* Modding

Le LED et le Raycast, je peux les survoler.

Mais le modding, il faut que vous testiez.

---



**La thématique de la Jam du 3 juin :**
« Godotarium avec des KS4036 pour apprendre à coder »

[<img width="828" height="433" alt="image" src="https://github.com/user-attachments/assets/bd3fa43a-fb74-4491-b8b9-662180cfdad2" />](https://www.youtube.com/watch?v=5CaVhGTG8eA)      
[Demo Video](https://www.youtube.com/watch?v=5CaVhGTG8eA)   

- [Un robotarium ?](https://youtu.be/V10LkFVFppw?t=43)
- [Youtube Channel](https://www.youtube.com/channel/UC95etX3555MyNyOjoWHKFXg)
- [Robotarium Website](https://www.robotarium.gatech.edu/get-started)

Imaginez que vous vouliez apprendre à programmer:   
_Créez un jeu qui vous donne envie d’apprendre a coder._  

Voir :
- [CodInGame: Platinium Rift](https://www.codingame.com/multiplayer/bot-programming/platinum-rift-episode-2)
- [Farmer Drone](https://store.steampowered.com/app/2060160/The_Farmer_Was_Replaced/)
  - [Video](https://www.youtube.com/watch?v=Yf9H4doN-d4)  

---

# KS4036

[<img width="720"  alt="Image" src="https://github.com/user-attachments/assets/f09409d5-9025-45d7-87da-68344bb8f31b" />](https://github.com/keyestudio/KS4036F-KEYESTUDIO-Microbit-Smart-Robot-Car/blob/master/docs/1.Introduction.md)


Info : [Quick](https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/220)

`~Sujet~` Pas certain de vouloir explorer cela ensemble.

Composition : [Info](https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/220)

**⚠️ Zip et Asset pour le projet:**  [Voir](https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr/issues/8)        
(Si vous le desirez, aucune obligation 😋 )     

* Une LED RGB à gauche et une LED RGB à droite
* Deux roues
* Mouvement en `differential drive`
  * Une vitesse propre au KS4036
* Un détecteur de distance frontal
* Deux détecteurs de ligne en dessous
* Un port pour le Micro:bit
  * Un Micro:bit avec le code
* ~Challenge : Deux capteurs lumineux~
* Un récepteur infrarouge
  * Une télécommande infrarouge
* Autre : ~Batterie~, ~Des pins~

Le Micro:bit sur le slot possède :

* ~Un microphone~
* ~Un capteur de température~
* Un compas
* Un gyroscope
* Un speaker
* ~Challenge : Bluetooth UART BLE~

Vous pouvez rajouter à votre boîte à outils des senseurs pour votre futur jeu.
* All KeyeStudio : [https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors/issues/3](https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors/issues/3)
  * KeyeStudio : [https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues?q=Keye](https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues?q=Keye)
* TinkerCard : [https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/145](https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/145)
  * Tous les composants électriques : [https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/159](https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/159)

Inspiration Microbots :
[https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/277](https://github.com/EloiStree/2026_03_23_doc_micro_bit_sensor/issues/277)

Trouvez des éléments 3D pour les senseurs :
[https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors](https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors)



-----------------------


**Challenge : Dessinez sur le sol une carte :**   
<img width="720" alt="image" src="https://github.com/user-attachments/assets/4643da8d-038f-4c64-ae28-70dfc6f299e5" />   
<img width="1024" height="1024" alt="circuit_line" src="https://github.com/user-attachments/assets/ab1ab76e-9ec9-4bed-ad66-d1a42cbcc91c" />   

Dans un tournoi de robots, il y a généralement un sol avec des lignes.
Pour cela, il faut deux outils :

* un pour éditer une Texture2D (image)
* un pour lire une Texture2D (image)

*(Attention : éditer et lire en même temps… c’est compliqué. À éviter si possible.)*


**Solution : KS4036 moddé**   
[<img width="720" alt="image" src="https://github.com/user-attachments/assets/a8d54e4e-2946-4fda-a3d1-5c3aeed90721" />](https://www.youtube.com/watch?v=BnndejesWyA)
[https://www.youtube.com/watch?v=BnndejesWyA](https://www.youtube.com/watch?v=BnndejesWyA)   
[📼 Video](https://www.youtube.com/watch?v=BnndejesWyA)   

Je ne vous aurais pas donné cet exercice si je ne savais pas le faire moi-même.
Voici un exemple de ce que cela peut donner.



**Example sur le modding**
https://github.com/EloiStree/2026_05_22_gdp_modding_lab


<img width="1156" height="718" alt="image" src="https://github.com/user-attachments/assets/7c58d61a-787c-4902-bb5d-2659e107370c" />

<img width="740" height="344" alt="image" src="https://github.com/user-attachments/assets/f6c6a737-70b0-4c17-8eb7-a93e11ef8ee9" />

**Mecredit 27:** Modding     
https://github.com/EloiStree/HelloGodotModding/blob/main/Note/2026_05_26   

-----------

Hesitez pas ajouter ce plug-in dans votre projet:   
[<img width="720" alt="image" src="https://github.com/user-attachments/assets/6c3da063-9d06-430f-879a-927eb021f656" />](https://godotengine.org/asset-library/asset/1766)   
https://godotengine.org/asset-library/asset/1766  








-------------------------


# Hello Input and Git

Le premier exercice, le plus simple, est de se répartir sur les inputs :

* Clavier, gamepad, XRController...
  Voir : [https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr/issues/8#issuecomment-4534585189](https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr/issues/8#issuecomment-4534585189)

Cela permet de repratiquer le GDScript chez tout le monde en attendant que les dépôts Git soient créés.


Il vous faut techniquement deux dépôts Git (ou trois) :
* Un pour la boîte à outils que vous allez créer ;
* Un pour le projet Godot XR contenant la boîte à outils.
   
(Trois projets si vous voulez créer une version non XR et pratiquer les submodules.
Un seul projet si vous ne voulez pas faire une boîte à outils et que tout le monde code en XR Link.)   
   


Mode d’input conseillé :

* deux flottants pour les roues avec leur intensité ;
* un joystick pour aller de gauche à droite et de derrière à devant ;
* 4 boutons pour simuler une manette low cost Amazon.

Je vous invite à adapter la voiture que vous avez codée la semaine passée, ou la voiture de Nico, afin de pouvoir tester cela au plus vite.

Je vous laisse coder la voiture, je vous fais confiance.

Si vous n’y arrivez pas, vous pouvez trouver une solution ici :
[https://github.com/EloiStree/2025_10_19_gdp_kid_toy_skid_steering](https://github.com/EloiStree/2025_10_19_gdp_kid_toy_skid_steering)

En attendant, si jamais...
``` gdscript
class_name ModMiniCarFromJoystickCharacter
extends CharacterBody3D

@export var move_speed_in_ms: float = 0.2
@export var rotation_speed_angle: float = 90
@export var gravity: float = 0.2 

@export var joystick: Vector2 = Vector2.ZERO
@export var forward_rayscast_distance:float
@export var left_line_sensor_color:Color
@export var right_line_sensor_color:Color
@export var global_euler_for_debug:Vector3
@export var global_position_for_debug:Vector3

## Expect a joystick from -1 to 1 in X Yup
func set_joystick(joystick_value: Vector2) -> void:
	joystick_value.x = clamp(joystick_value.x, -1, 1)
	joystick_value.y = clamp(joystick_value.y, -1, 1)
	self.joystick = joystick_value    

func _physics_process(delta: float) -> void:
	
	global_position_for_debug = car_ground_wheel_center_point.global_position
	global_euler_for_debug = car_ground_wheel_center_point.global_rotation_degrees
	
	## On tourne sur le Y du character avec le temps qui passe par frame (delta)
	# Attention le rotation sont en radian et pas en degrees.
	if abs(joystick.x) > 0.1: 
		rotate_y(-joystick.x * deg_to_rad(rotation_speed_angle)     * delta)

	# Le Z de godot est inverse du Z de Unity que l on utilise nous.
	var forward_direction = -global_transform.basis.z      
	## On utilise la vitesse et le joystick pour donner la direction
	var target_velocity = forward_direction * (joystick.y) * move_speed_in_ms    
	## On donne les informations a velocity pour bouger le character
	velocity.x = target_velocity.x
	velocity.z = target_velocity.z    
	velocity.y = -gravity

	## on demande au code d être calculer.
	move_and_slide()


func get_forward_distance()->float:
	return forward_rayscast_distance

func get_left_line_color()->Color:
	return left_line_sensor_color

func get_right_line_color()->Color:
	return right_line_sensor_color



#region COMPLEXITY

@export var car_ground_wheel_center_point:Node3D
@export var car_forward_point:Node3D
@export var car_right_side_point:Node3D
@export var car_left_line_tracker:Node3D
@export var car_right_line_tracker:Node3D


func set_by_developer_distance_rayscast(distance:float):
	forward_rayscast_distance = distance

func set_by_developer_left_line_color(color:Color):
	left_line_sensor_color = color

func set_by_developer_right_line_color(color:Color):
	right_line_sensor_color = color

func get_left_line_tracker_global_position()->Vector3:
	return car_left_line_tracker.global_position if car_left_line_tracker else Vector3.ZERO
	
func get_right_line_tracker_global_position()->Vector3:
	return car_right_line_tracker.global_position if car_right_line_tracker else Vector3.ZERO

func get_global_position()->Vector3:
	return car_ground_wheel_center_point.global_position if car_ground_wheel_center_point else Vector3.ZERO

func get_global_euler_radian()->Vector3:
	return car_ground_wheel_center_point.global_rotation if car_ground_wheel_center_point else Vector3.ZERO

func get_global_euler_euler()->Vector3:
	return car_ground_wheel_center_point.global_rotation_degrees if car_ground_wheel_center_point else Vector3.ZERO

func get_global_quaternion()->Quaternion:
	return Quaternion.from_euler(car_ground_wheel_center_point.global_rotation) if car_ground_wheel_center_point else Quaternion.IDENTITY

func get_global_unity_forward_direction()->Vector3:
	return -car_ground_wheel_center_point.global_basis.z if car_ground_wheel_center_point else Vector3.ZERO
	
#endregion
```







