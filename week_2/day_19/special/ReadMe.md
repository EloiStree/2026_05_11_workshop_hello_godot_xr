## Exercice : Rotation, Servo moteur et préparation XR avec Godot

Profitons de l’énergie du matin pour revoir le code de rotation travaillé hier, puis nous continuerons avec Godot XR. Oui, encore des rotations.

Vous trouverez dans ce fichier ZIP :
* un servo moteur,
* la voiture utilisée hier,
* une télécommande IR pour l’exercice suivant.

[Télécharger car_servor_ir.zip](https://github.com/user-attachments/files/27984166/car_servor_ir.zip?utm_source=chatgpt.com)

---

# Partie 1 : La carrosserie

## Préparation du véhicule

1. Prenez le modèle 3D du **KS4036** et placez-le dans le cube créé hier.
2. Conservez le cube comme référence de dimensions, mais masquez-le dans la scène.
3. Ajoutez deux mini-moteurs au véhicule.

## Structure des mini-moteurs

Pour chaque mini-moteur :
* ajoutez un objet vide servant d’**ancrage** (`Anchor`) ;
* ajoutez dans cet ancrage un objet vide servant de **pivot** (`Pivot`) ;
* ajoutez ensuite le modèle 3D de la roue dans le pivot.

---

# Partie 2 : Faire tourner une roue

<img width="289" height="155" alt="image" src="https://github.com/user-attachments/assets/8ab1ac95-50c3-447d-9470-437815786dd6" />

Hier, pour faire tourner la voiture sur l’axe vertical (`Y`), nous avons utilisé :
* `rotate_y`
* `deg_to_rad`

Cette fois, nous allons appliquer une logique similaire aux roues.

## Objectifs

Le système doit permettre au game designer de définir :
* une **vitesse maximale de rotation** ;
* une **vitesse actuelle** de la roue.

## Méthodes à créer

Le développeur doit fournir deux méthodes :
* `set_speed_with_percentage(percent)`
* `set_speed_with_degree(speed_degree)`

## Contraintes

L’angle de rotation de la roue ne doit jamais dépasser `720°`
Mettez en place une logique permettant de moduler l'angle afin d’éviter les problèmes liés à l’accumulation des valeurs flottantes pendant de longues sessions d’exécution.

---

# Partie 3 : Servo moteur SG90

<img width="446" height="256" alt="image" src="https://github.com/user-attachments/assets/76b5fa13-1d7a-4396-acdc-e8ea6495bdaa" />

Nous allons maintenant travailler avec un servo moteur **SG90**.

## Contraintes du servo

Le servo peut tourner uniquement entre :
* `0°`
* `180°`

## Structure demandée

1. Importez le modèle 3D.
2. Créez :
   * un `Anchor`
   * un `Pivot`

## Script à réaliser

Le script doit proposer les méthodes suivantes :

* `set_with_angle_0_180(angle)`
* `set_with_percent(percent)`

Le système doit garantir que les rotations restent toujours dans les limites autorisées du servo.

---

# Partie 4 : Télécommande IR et préparation XR

<img width="543" height="385" alt="image" src="https://github.com/user-attachments/assets/e0c218b8-33fd-40c1-8258-a27b320e8a07" />

Pour le prochain exercice Godot XR :

1. Importez la télécommande IR.
2. Essayez de :
   * l’attraper correctement ;
   * la positionner naturellement dans la main du joueur en VR.

**Niveau Medior**

Lorsque la télécommande est tenue en main :
* les boutons de la manette doivent envoyer des entiers via un singleton statique.

**Niveau Senior***
Les entiers ne doivent être transmis :
* qu’aux objets :
  * visibles devant la manette ;
  * situés dans un certain rayon d’action.


---------------------


# NFC Reader

Dans la vraie vie, il existe des lecteurs NFC. Ce sont basiquement des claviers qui tapent rapidement un code lorsqu’ils détectent une onde.

Le même principe est utilisé pour les scanners : ce n’est rien d’autre qu’un clavier.

Dans vos jeux, vous pourriez avoir des cartes qui, une fois placées sur une zone, envoient un texte :

* `DDKWEJ546` : pour charger un objet
* `OIUEW-KJ654` : pour charger un debugger
* `scene:demo_level_1` : pour charger la démo du niveau 1



Comment coderiez-vous cela ?
  
( N'utiliser pas un Area3d, car on les vera vendredi.  
Utiliser une liste de badge dans la scene et la distance entre les objets. )   

  
<img width="1514" height="586" alt="image" src="https://github.com/user-attachments/assets/574d61a8-4344-4699-8c4b-96d73cc1cc92" />
<img width="1487" height="591" alt="image" src="https://github.com/user-attachments/assets/1e70a723-c32f-4897-a0c8-a3de87c38414" />

