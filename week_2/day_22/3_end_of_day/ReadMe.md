**Objectif : créer un broadcaster**

On met en place une sphère scalable, dans l’esprit de ce qu’on peut voir dans Gravity Sketch.
*Ajouter une image*

Toutes les Area3D présentes dans cette zone et appartenant au groupe ciblé recevront les instructions suivantes :
* `mod_on_turn_on()`
* `mod_on_turn_off()`

Ces événements seront déclenchés lors de l’appui et du relâchement de la touche Enter.

---

On en profitera pour tester notre système de script tags via un script d’interface :
```gdscript
class_name XrModListenToTurnOnOff 
extends Node

func mod_on_turn_on()
func mod_on_turn_off()
func mod_on_rotating_left_right_percent(percent_11: float)
func mod_on_joystick_input_received(joystick: Vector2)
```

Le joystick gauche de la manette sera utilisé comme entrée principale,
et les triggers permettront de gérer la rotation gauche/droite.

Toutes les scripts présents dans la sphère recevront ces événements.

