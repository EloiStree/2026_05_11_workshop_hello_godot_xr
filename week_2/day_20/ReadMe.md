<img width="598" height="120" alt="image" src="https://github.com/user-attachments/assets/3fa16c8b-a180-4ede-88fb-b24d759cefc5" />

# Journée

J’espère que vous avez bien dormi 😅🔥

Aujourd’hui, on va explorer les rotations… et surtout les pratiquer sérieusement.

## Matin : Comprendre les rotations (Tilt, Roll, Yaw)

Le *tilt*, le *roll* et le *yaw* sont des notions clés en aviation pour décrire les rotations.
À l’époque de l’Oculus DK1, ces trois axes représentaient l’essentiel du suivi de la tête.

Prenons le temps de comprendre leur signification et de les comparer aux angles d’Euler.

On en profitera aussi pour aborder la relocalisation dans un plan cartésien ainsi que les quaternions.

## Avant-midi : Construire un compas

Objectif : déterminer une direction de référence (le “Nord”, ou ici le centre du niveau / un *node*).

* Récupérez la position et la direction de votre SSD1306
* Supprimez la composante verticale pour travailler uniquement sur le plan XZ
* Comparez la direction *forward* avec la direction vers votre cible
* Calculez l’angle entre ces deux directions

Utilisez cet angle pour dessiner une ligne sur le SSD1306 et créer un compas fonctionnel.

## Après-midi : Simulation de volant (Roll)

Mettons ça en pratique avec un volant.

* Fixez un SSD1306 sur une *graybox* représentant un volant
* Cherchez comment calculer l’inclinaison autour de son axe frontal (axe Z)

N’hésitez pas à passer par Open Brush pour visualiser votre approche avec des croquis. Votre cerveau vous remerciera.

## Fin de journée : Niveau à bulle

Placez le SSD1306 face vers le bas.

* Calculez son inclinaison en *roll* et en *tilt*
* Utilisez ces valeurs pour afficher un cercle mobile à l’écran

L’objectif est de reproduire le comportement d’un niveau à bulle.

## Conclusion

Allez aussi loin que possible selon votre niveau.
Si ça devient difficile, concentrez-vous sur la compréhension plutôt que sur la complétion.

Le vrai objectif, c’est de maîtriser les rotations, les angles d’Euler,
avec un petit rappel de trigonométrie pour faire bonne mesure.
