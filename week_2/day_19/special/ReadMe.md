Profitons de la vivaciter du matin pour refaire un peu de code de rotation d hier.
Puis on continuera sur Godot XR.

Vous touverez dans ce  zip un servo moteur, la voiture d'hier et un telecommande IR pour l exerice suivant.
[car_servor_ir.zip](https://github.com/user-attachments/files/27984166/car_servor_ir.zip)


### La carcase

Prenez le model 3D du KS4036 et placer la dans votre cube d hier.
Garder le cube pour si vous oubliez les dimensions, juste cacher le.

Ajouter y deux mini-moteur.

Dans les mini moteur"
- ajouter un point vide d'ancrage
- ajouter dans l ancrage un point vide de pivot
- ajouter ans le pivot le model 3D de la roue

### Faire tourner une roue.

Si vous vous souvenez pour tourner sur le Y vertical hier avec notre voiture on a utiliser `rotate_y` et `deg_to_rad` 🤔

Ici on doit proposer au game designer:
- une vitesse maximal de rotation
- une vitesse actuel de la  roue

Pour le developpeur deux methodes:
- set_speed_with_percentage
- set_speed_with_degree

L angle de la roue ne peut jamais depasser 720 degree. 
Prenez cette logique en compote pour eviter un depassement du float sur une longue session.


### Servo Moteur

Continuons sur le rotation

On aimerai utiliser des servos moteur SG90.
Ceux-ci vont de 0 a 180 degree

Prenez le model 3D creer un anchor et un pivot.

Creer un script qui recoit 
= set_with_angle_0_180(angle)
- set_with_percent(percent)








