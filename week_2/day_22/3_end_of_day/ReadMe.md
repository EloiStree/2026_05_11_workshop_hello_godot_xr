**Objectif : créer un broadcaster**

On met en place une sphère scalable, dans l’esprit de ce qu’on peut voir dans Gravity Sketch.    
[<img width="994" height="530" alt="image" src="https://github.com/user-attachments/assets/61d7fbff-23ee-402a-b87b-84234cbb4517" />](https://www.creativebloq.com/how-to/use-gravity-sketch)       
https://www.creativebloq.com/how-to/use-gravity-sketch    


 Voir : [has_method(string)](https://forum.godotengine.org/t/what-is-the-replacement-of-gdscript-has-method-the-doc-is-short-and-unclear-to-me/39471) et .call()   
 
 _Pas confondre avec les [callable](https://docs.godotengine.org/fr/4.x/classes/class_callable.html) sujet trop bien mais hard si on debut 😅_      

Toutes les Area3D présentes dans cette zone et appartenant au groupe ciblé recevront les instructions suivantes :
* `mod_on_turn_on()`
* `mod_on_turn_off()`
   
Ou simplement utiliser des methodes de Godot:   
- `queue_free()`  
- `hide()`  
- `show()`
- `set_text(string)`

Ces événements seront déclenchés lors de l’appui et du relâchement de la touche Enter.

---

On en profitera pour tester notre système de script tags via un script d’interface :
```gdscript
class_name XrModListenToTurnOnOff 
extends Node

func mod_on_turn_on()
func mod_on_turn_off()
func mod_on_rotating_left_right_in_percent(percent_11: float)
func mod_on_translate_back_front_in_percent(percent_11: float)
func mod_on_joystick_input_received(joystick: Vector2)
```

Toutes les scripts présents dans la sphère recevront ces événements.



Ici, on utilise une sphère, mais on pourrait tout aussi bien travailler avec un cylindre attaché à la main du joueur ou à sa tête, afin d’interagir avec un objet ciblé ou simplement celui qu’il est en train de regarder.
  
Autre exemple : la télécommande infrarouge, qu’on pourrait simuler dans notre projet.
