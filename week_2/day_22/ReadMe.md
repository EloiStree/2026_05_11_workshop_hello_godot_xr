<img width="589" height="122" alt="image" src="https://github.com/user-attachments/assets/3d9de62a-9925-43ce-8b71-eeab8f4bdc2f" />



# Journée

Un cours particulièrement important : les Area3D et leurs collisions.

Qu’il s’agisse de détecter un bouton pressé, un joueur entrant dans une zone ou une balle touchant une cible…
La majorité de ces interactions repose sur des collisions entre deux Area3D.

Pour filtrer ces collisions, on utilise les *layers*, les *groups* et les *layer masks*.
Mais il existe aussi un pattern intéressant que j’appelle le **Script Tag**.

Aujourd’hui, l’objectif est de créer un petit outil réutilisable permettant de lister les objets présents dans une Area3D avec lesquels on peut interagir.

---

## Matin : Qu’est-ce qu’une Area3D ?

Une Area3D est une zone permettant de détecter des interactions sans provoquer de collision physique (les objets peuvent se traverser).

Questions à explorer :

* Comment utiliser une Area3D ?
* Comment détecter lorsqu’une Area3D en touche une autre ?

---

## Avant-midi : Layers et Groups

Pour éviter des problèmes de performance, on peut filtrer les collisions via les *layers*.
C’est rapide et efficace… mais limité à 32 par projet, et peu pratique à réutiliser d’un projet à l’autre.

Une autre approche consiste à utiliser les *groups*, une fonctionnalité disponible dans Godot (et absente dans Unity).

On peut aussi autoriser les collisions, puis utiliser un *layer mask* pour filtrer uniquement celles qui nous intéressent du point de vue du game design.

---

## Après-midi : Script Tag

On sait maintenant filtrer les objets présents dans une zone.

Mais peut-on aller plus loin ?
Par exemple, filtrer ces objets en fonction de la présence d’un script spécifique sur le node, ses parents ou ses enfants ?

C’est là qu’intervient l’idée de **Script Tag**.

---

## Fin de journée : Broadcast d’actions

Imaginons qu’on veuille allumer une lampe sélectionnée avec une sphère, un peu comme dans Gravity Sketch,
ou via un laser que vous auriez codé.

Deux approches possibles :

* Vérifier si un node possède un script d’interaction spécifique
* Ou appeler une méthode sur tous les nodes détectés, sans distinction (version “bourrin assumé”)

Essayons de comparer ces deux approches.

---
