
<img width="530" height="119" alt="image" src="https://github.com/user-attachments/assets/69d7a6f9-1dd4-4598-849c-fb04b673a37c" />


# Journée

Un des grands avantages de Godot est qu’il s’agit d’un langage interprété livré avec son propre interpréteur.
Cela signifie qu’aucune installation supplémentaire n’est nécessaire pour permettre le modding.

Créons un premier jeu 2D moddable, puis chargeons du code en 3D lorsqu’une touche du clavier est pressée.

---

# Matin : Le modding, c’est quoi ?

Qu’est-ce que le modding ?

Quelques pistes :

* VRChat et les Asset Bundles de Unity
* Lua : toujours utile avec Godot ?
* Comment fonctionne le modding dans les grandes lignes ?
* Créons un nœud capable de charger du code à l’exécution (runtime)

---

# Avant-midi : Expérimenter avec un nœud

Rédigeons un manuel définissant les méthodes, variables et signaux requis pour les moddeurs.

Prenons le code chargé par l’utilisateur :

* Vérifions que les méthodes définies dans le manuel sont présentes
* Affectons les variables correspondant aux noms définis
* Connectons les signaux disponibles selon le manuel

Ajoutons ensuite un petit éditeur de code pour l’utilisateur.

---

# Après-midi : Dumb Mower

Essayons de créer une tondeuse autonome… mais pas très intelligente.

Relions :

* les capteurs de distance (raycasts)
* deux capteurs de ligne
* deux LEDs
* un écran SSD1306

…à notre véhicule moddable.

Objectif :

* avancer jusqu’à une collision
* selon le capteur activé, tourner de *n* degrés avec les clignotants allumés
* puis continuer tout droit

Voilà. Simple, imparfait, mais fonctionnel.

Regarder une vidéo du Robotarium pour inspiration.
