<img width="596" height="132" alt="image" src="https://github.com/user-attachments/assets/e2dfdac7-0038-4709-9e8f-8891fe3e122e" />



# Semaine

On a appris à faire bouger une voiture et à manipuler des objets 3D.
Essayons de créer un jeu de voitures moddable, sur lequel on peut charger du code en le “snappant”, comme avec un BBC micro:bit.

---

# Journée

## Matin : Distance et Raycast

Notre voiture a besoin de savoir ce qu’il y a en face d’elle.
Créons un système qui permet de détecter les collisions avec un raycast, ainsi que la distance jusqu’à la collision.

Fournissons cette valeur sous différents formats de signal :

* en mètres
* en pourcentage
* en valeur analogique
* en valeur digitale

---

## Avant-midi : Dual Line Sensors

Même concept : utiliser le raycast ou un Area3D pour fournir au joueur deux valeurs booléennes de détection de ligne.

Essayez de faire en sorte que vos capteurs soient suffisamment génériques pour être réutilisables dans d’autres projets.

---

## Après-midi : Parlons couleurs LED RGB

Notre KS4036 possède deux gros clignotants RGB configurables.

Essayons de :

* créer un système basé sur un changement de matériau
* écrire un script utilisant un duplicata du matériau de base, configurable avec 3 valeurs RGB (bytes)

---

## Fin de journée : SG90 et moteur pas à pas

Une grande partie des projets IoT que vous pouvez réaliser avec des étudiants en électronique repose sur des servomoteurs et des moteurs pas à pas.

Essayons de créer un système avec un servo SG90 micro servo pour commencer.

Si vous êtes à l’aise :

* créez un moteur pas à pas où la précision est importante

Essayez de vous rapprocher de la réalité autant que possible.

**Niveau intermédiaire :** vous pouvez utiliser des ressources pour configurer les types de valeurs, si vous voulez vous entraîner à créer des ressources.

---

## But de la journée

Le but est de pratiquer :

* les rotations
* les mathématiques
* les raycasts
* la création d’outils réutilisables
