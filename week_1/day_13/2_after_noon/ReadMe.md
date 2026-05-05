
**Objectif :** Passer de 1D à 2D et vice versa

Vous ne saurez pas tout faire en une après-midi.   
Mais essayez, selon votre niveau, de pratiquer avec la liste suivante.   

Pensez comme à une boîte à outils que vous allez réutiliser :    
* pas de fautes d’orthographe   
* réfléchissez au nom des méthodes et des variables
* vérifiez votre code en produisant des tests
* ...

Utilisons une classe statique pour stocker votre code.   
Par exemple :   
`StaticDrawer128x64.set_pixel_lrtd(array, x, y, is_on)`   

```gdscript
class_name StaticDrawer128x64
extends Resource

# C'est l'occasion de parler du concept de paramètres par copie et par référence 😅
func set_pixel_lrtd(array: Array[bool], x_left_right: int, y_top_down: int, is_on: bool) -> void:
    pass
```


**Checklist :**
* [ ] Passer de 1D à 2D
  * [ ] Créer une boîte à outils qui reçoit un tableau `Array[]`
    * [ ] Permet de définir un pixel (X, Y) comme vrai ou faux
      * [ ] de gauche à droite et de haut en bas
      * [ ] de gauche à droite et de bas en haut
    * [ ] Permet de demander la valeur d’un pixel
    * [ ] Permet de déplacer un pixel
    * [ ] Permet de dupliquer un pixel
  * [ ] Afficher les coins de notre écran avec leurs coordonnées (X, Y)
  * [ ] Dessiner les bordures avec un pixel
  * [ ] Bit shift 2D
    * [ ] Dessiner un pixel sur l’écran
    * [ ] Déplacer l’écran de gauche à droite
    * [ ] Déplacer l’écran de haut en bas
    * [ ] Ajouter une option pour conserver les pixels d’un bord lors du déplacement
    * [ ] Déplacer en diagonale
* [ ] Passer de LRTD à LRDT
  * [ ] En LRDT :
    * [ ] Dessiner une ligne verticale depuis (X, Y)
    * [ ] Dessiner une ligne horizontale depuis (X, Y)
    * [ ] Savoir dessiner une ligne dans les directions : haut, droite, bas, gauche
    * [ ] Savoir dessiner une diagonale depuis (X, Y)
    * [ ] Vous savez faire des lignes droites ?
      * [ ] Dessiner les bords d’un rectangle
      * [ ] Dessiner un rectangle de X1Y1 à X2Y2
    * [ ] Vous savez faire des rectangles ?
      * [ ] Dessiner un plateau d’échecs
      * [ ] Dessiner un damier avec des carrés de n pixels
  * [ ] **Challenge :** Dessiner une ligne non droite 😅
    * [ ] Dessiner les bords d’un triangle avec trois points
    * [ ] Dessiner un triangle avec trois points

