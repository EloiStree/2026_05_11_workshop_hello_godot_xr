## Exercice : Rotation, Servo moteur et préparation XR avec Godot

Profitons de l’énergie du matin pour revoir le code de rotation travaillé hier, puis nous continuerons avec Godot XR. Oui, encore des rotations. L’humanité a décidé que presque tout devait tourner un jour ou l’autre.

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

La hiérarchie doit permettre une rotation propre de la roue.

---

# Partie 2 : Faire tourner une roue

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

L’angle de rotation de la roue ne doit jamais dépasser `720°`.

Mettez en place une logique permettant de limiter ou réinitialiser proprement l’angle afin d’éviter les problèmes liés à l’accumulation des valeurs flottantes pendant de longues sessions d’exécution.

---

# Partie 3 : Servo moteur SG90

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

Pour préparer le prochain exercice Godot XR :

1. Importez la télécommande IR.
2. Essayez de :
   * l’attraper correctement ;
   * la positionner naturellement dans la main du joueur en VR.

Parce qu’un objet flottant à moitié dans les doigts reste une tradition du développement XR, mais on peut essayer de faire mieux.

---

# Objectifs avancés

## Niveau Medior

Lorsque la télécommande est tenue en main :

* les boutons de la manette doivent envoyer des entiers via un singleton statique.

## Niveau Senior

Les entiers ne doivent être transmis :

* qu’aux objets :
  * visibles devant le joueur ;
  * situés dans un certain rayon d’action.
