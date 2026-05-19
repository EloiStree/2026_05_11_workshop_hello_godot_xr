## Matin : Comprendre les rotations (Tilt, Roll, Yaw)

Le *tilt*, le *roll* et le *yaw* sont des notions clés en aviation pour décrire les rotations.
À l’époque de l’Oculus DK1, ces trois axes représentaient l’essentiel du suivi de la tête.

Prenons le temps de comprendre leur signification et de les comparer aux angles d’Euler.

On en profitera aussi pour aborder la relocalisation dans un plan cartésien ainsi que les quaternions.

En Euler :

* Tilt serait pour nous le X
* Roll serait pour nous le Z
* Yaw serait pour nous le Y

## Exercice :

* Recréons un Gimbal Lock dans Godot en 2D.
* Relions des sliders UI de Godot pour jouer avec tout ça.

[<img width="1209" height="600" alt="image" src="https://github.com/user-attachments/assets/a8a320d6-8003-47a3-a48b-1f24261ca4fd" />](https://www.thingiverse.com/thing:73634)

[https://www.thingiverse.com/thing:73634](https://www.thingiverse.com/thing:73634)

[<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/9108e47d-18fe-4fc1-b57d-a901d50d5f0e" />](https://sketchfab.com/search?features=downloadable&q=gimbal&type=models)

Trouver des Gimbals :

* [https://www.thingiverse.com/search?q=gimbal+sg90&page=1](https://www.thingiverse.com/search?q=gimbal+sg90&page=1)
* [https://sketchfab.com/search?features=downloadable&q=gimbal&type=models](https://sketchfab.com/search?features=downloadable&q=gimbal&type=models)


-------------

# C'est toujours plus simple en Zero

<img width="834" height="537" alt="image" src="https://github.com/user-attachments/assets/8f0102c9-2995-4ded-ad78-ea442f76127d" />    

_"C'est toujours plus simple les maths au centre du plan"_   


Ramenons le point en sur le zero vers l'avant de Godot.
``` gdscript
@tool
extends Node3D


@export var le_point_observer:Node3D
@export var le_plan:Node3D

@export var localized_point_observer:Vector3
@export var le_point_observer_relocalised:Node3D

func _ready():
    refresh()


func _process(_delta:float):
    refresh()


func refresh():
    var v3_le_point_observer:Vector3= le_point_observer.global_transform.origin
    var v3_le_plan:Vector3 = le_plan.global_transform.origin
    var q_le_plan:Quaternion = Quaternion.from_euler(le_plan.global_rotation)

    var v3_le_point_observer_at_zero:Vector3 = v3_le_point_observer - v3_le_plan
    var q_inverse_le_plan:Quaternion = q_le_plan.inverse()
    var v3_le_point_observer_at_zero_and_rotated:Vector3 = q_inverse_le_plan * v3_le_point_observer_at_zero
    localized_point_observer = v3_le_point_observer_at_zero_and_rotated
    le_point_observer_relocalised.global_transform.origin = localized_point_observer
```
