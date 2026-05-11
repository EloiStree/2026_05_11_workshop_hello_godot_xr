<img width="512" height="256" src="objectif.svg" alt="Objectif of the day"/>

**Objectif :** installer un écran OLED 128×64 et dire bonjour à GDScript  

[<img width="720"  alt="image" src="https://github.com/user-attachments/assets/1be1ba64-1073-4eca-ac88-d6016e77aca4" />](https://github.com/EloiStree/2026_04_27_gdp_oled_128x64)   


Dans la vidéo de Brackeys, vous avez vu que l’on utilise GDScript pour coder dans Godot.     
Ici, on met de côté le moteur de jeu pour se concentrer uniquement sur le code.     

Une fois l’addon ajouté, vous pourrez par exemple afficher un tableau de valeurs booléennes (vrai/faux) directement sur l’écran 😋    
Rien de plus compliqué que ça.     

---


Le principe de base reste très simple : un écran OLED 128×64 pour :  
* s’entraîner à coder   
* apprendre à modifier / modder   
* servir de petit outil de debug en XR  

---

**OLED 128×64 :**  

[<img width="720"  alt="image" src="https://github.com/user-attachments/assets/7a832c95-d7d9-41cc-8d6e-f2c61a638ed8" />](https://github.com/EloiStree/2026_04_27_gdp_oled_128x64)     
[https://github.com/EloiStree/2026_04_27_gdp_oled_128x64](https://github.com/EloiStree/2026_04_27_gdp_oled_128x64)    





--------------


TODO :
- Une variable c est quoi ?
  -  `var ma_variable = 42`
  -  un type statique : 
  -  `var ma_variable : int = 42`
  -  Editable par le game designer :
  -  `export var ma_variable : int = 42`
  -  Afficher la variable dans la console :
  -  `print(ma_variable)`
  -  Afficher la variable sur l'UI du jeu :
  -  `@export var title_label : Label`
  -  `title_label.text = str(ma_variable)`
  - Afficher la variable au demarrage du jeu :
    - `func _ready():`
    - ` print("Hello world")`
  - Afficher le temps entre chaque frame :
    - `func _process(delta):`
    - ` print(delta)`
  - Print plusieur variables :
    - `var a = 42`
    - `var b = "Hello"`
    - `print("Des variables", a, ". Une Autre ", b)`
    - Utilisant join :
    - `var afficher:String="Des variables " + str(a) + ". Une Autre " + b`
    - `var afficher_format:String="Des variables %s. Une Autre %s" % [a, b]`
    - `var afficher_join:String=join(["Des variables ", str(a), ". Une Autre ", b])`
    - `print("Des variables " + str(a) + ". Une Autre " + b)`
- Variable primitive :
    - `int` : nombre entier
      - `var ma_variable : int = 42` 
    - `float` : nombre à virgule
      - `var ma_variable : float = 3.14` 
    - `bool` : vrai ou faux
      - `var ma_variable : bool = true`
    - `String` : chaîne de caractères
      - `var ma_variable : String = "Hello"`
- Variable classique:
  - `Vector2` : vecteur 2D
    - `var ma_variable : Vector2 = Vector2(1, 2)`
  - `Vector3` : vecteur 3D
    - `var ma_variable : Vector3 = Vector3(1, 2, 3)`
  - `Vector2i` : vecteur 2D entier
    - `var ma_variable : Vector2i = Vector2i(1, 2)`
  - `Vector3i` : vecteur 3D entier
    - `var ma_variable : Vector3i = Vector3i(1, 2, 3)`
  - `Color` : couleur
    - `var ma_variable : Color = Color(1, 0, 0)`
  - `Quaternion`* : rotation
    - `var ma_variable : Quaternion = Quaternion(0, 0, 0, 1)`
- Variable complexe :
    - `Array` : tableau de valeurs
      - `var ma_variable : Array = [1, 2, 3]`
    - `range()` : plage de valeurs
      - `var list_element_0_10: Array = range(1, 10)`
    - `for elememt in list_element_0_10:`
      - `print(element)` 
    - `Dictionary` : dictionnaire de valeurs
      - `var ma_variable : Dictionary = {"key": "value"}`
  - Variable de type Node :
    - `Node` : nœud de la scène
      - `var ma_variable : Node = get_node("NodePath")`
    - `Node2D` : nœud 2D
      - `var ma_variable : Node2D = get_node("NodePath")`
    - `Node3D` : nœud 3D
      - `var ma_variable : Node3D = get_node("NodePath")`
    - `Label3D` : nœud de texte 3D
      - `var ma_variable : Label3D = get_node("NodePath")`







