[<img width="720"  alt="image" src="https://github.com/user-attachments/assets/7b5d7830-dab5-49d9-b09a-879a6697de94" />](https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr)        
[<img width="720"  alt="image" src="https://github.com/user-attachments/assets/0965456b-a5a9-4db8-b8ad-bd35532d51c9" />](https://github.com/EloiStree/2026_05_11_workshop_hello_godot_xr)  

« Ce n’est pas avec neuf femmes qu’on peut faire un enfant en un mois. »  
- Coder prend du temps
- Coder demande des pauses pour réfléchir
- Coder demande parfois de “dormir dessus” pour laisser les idées mûrir

Clean Code: [Video](https://github.com/EloiStree/HelloCleanCode)
- “Clean code always looks like it was written by someone who cares.”
- "Always code as if the person who ends up maintaining your code will be a violent psychopath who knows where you live." — John Woods

En gros : le but de la semaine n’est pas forcément d’aller vite ni de produire un maximum. L’objectif, c’est surtout de pratiquer le travail en équipe. Le code que vous écrirez cette semaine sera utilisé par d’autres la semaine prochaine 😅🍻



---

## Planning :

* 26–29 : Préparer le KS4036
* 1–2 : Revoir les Nodes et l’UI
* 3–5 : Game Jam

### But de la semaine :

Préparer une boîte à outils en groupe sur Git simulant un KS4036, sans avoir le jeu de la Jam en tête.

Le but est de l’utiliser durant la JAM.

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


Note: Circuit et Dessin
<img width="720" alt="image" src="https://github.com/user-attachments/assets/4643da8d-038f-4c64-ae28-70dfc6f299e5" />


[<img width="720" alt="image" src="https://github.com/user-attachments/assets/a8d54e4e-2946-4fda-a3d1-5c3aeed90721" />](https://www.youtube.com/watch?v=BnndejesWyA)   
https://www.youtube.com/watch?v=BnndejesWyA   

