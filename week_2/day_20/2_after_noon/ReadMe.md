**Objectif** : Wheel Roll

Essayons de créer un code qui détecte l’angle brut de notre volant, sans tenir compte de son orientation par rapport à un autre objet.

On utilise uniquement un quaternion en entrée.

Le but de l’exercice ici n’est pas de vous faire réussir cet exercice,
mais que vous preniez un papier et un bic et que vous réfléchissiez en termes de volume et d’espace.

Faites des hypothèses sur la façon dont vous vous y prendriez,
et une fois plus ou moins clair dans votre tête, essayez de coder comme vous pouvez.

Le but n’est pas d’y arriver, mais d’essayer d’y arriver.

The compass m’a pris 20 minutes, l’exercice suivant 40 minutes.
Celui-ci, je n’ai pas trouvé du premier coup et il m’a fallu une bonne après-midi.
Note que si j’avais bien mangé le matin et que j’étais dans un bon jour, ça aurait pu me prendre 20 minutes.

Petit astuce : allez dans [Open Brush](https://github.com/EloiStree/HelloPaintingJam/issues/9) (hors ligne).
[https://github.com/EloiStree/HelloPaintingJam/issues/9](https://github.com/EloiStree/HelloPaintingJam/issues/9)
Dessinez sur une feuille de papier, c’est important, mais il y a des choses qui en 3D se voient plus vite.


Si vous n’arrivez pas à faire cet exercice 😉, pas de panique.   
Revenez dessus après la formation.    

Le suivant est plus « facile ».     


------------------

<img width="500" height="279" alt="image" src="https://github.com/user-attachments/assets/aa860242-36e8-4c19-b517-ee542dd02b63" />


------------------

**Solution:**
``` gdscript
class_name SSD1306RotationWheelFaceForward
extends Node

@export var frontal_center_point: Node3D
@export var wheel_rotation_left_right_in_degrees: float = 0.0

func _process(delta: float) -> void:
	wheel_rotation_left_right_in_degrees = compute_wheel_rotation_in_degrees_from_frontal_double_point(frontal_center_point)
	#print("Wheel Rotation Left Right in Degrees: ", wheel_rotation_left_right_in_degrees)


func get_wheel_rotation_in_degrees_left_right() -> float:
	return wheel_rotation_left_right_in_degrees


static func compute_wheel_rotation_in_degrees_from_frontal_double_point(front_center_point_forward: Node3D) -> float:

	# Have a point offset in forward is useless
	# It was just easier for me to when thinking about this math algorithm.
	# Feel free to correct it.

	
	var wheel_rotation_left_right_in_degrees = 0.0

	var q_forward = Quaternion.from_euler(front_center_point_forward.global_rotation)
	var v_forward = q_forward*Vector3(0,0,-1)
	var v_global_up = Vector3.UP
	var angle_up_to_forward = rad_to_deg(acos(clamp(v_forward.dot(v_global_up) / (v_forward.length() * v_global_up.length()), -1.0, 1.0)))
	var is_facing_up = angle_up_to_forward < 90.0


	var q_right :Quaternion= q_forward * Quaternion.from_euler(Vector3(0,deg_to_rad(-90),0))
	var v_right :Vector3= q_right*Vector3(0,0,-1)
	var q_left :Quaternion= q_forward * Quaternion.from_euler(Vector3(0,deg_to_rad(90),0))
	var v_left :Vector3= q_left*Vector3(0,0,-1)



	var left_point :Vector3= v_forward + v_left
	var right_point :Vector3= v_forward + v_right

	var right_leveled_point_at_center_height = Vector3( right_point.x,left_point.y,right_point.z)
	var hypotenuse = (right_point - left_point).length()
	var height = (right_point- right_leveled_point_at_center_height ).y
	var adjacent = (right_leveled_point_at_center_height - left_point).length()
	var angle = rad_to_deg(acos(clamp(adjacent / hypotenuse, -1.0, 1.0)))
	var is_height_up = height >= 0.0
	var is_height_down = height < 0.0

	# print("Angle: ", angle, " Is Facing Up: ", is_facing_up, " Is Height Up: ", is_height_up)

	if is_height_down and is_facing_up:
		wheel_rotation_left_right_in_degrees =  angle
	if is_height_down and not is_facing_up:
		wheel_rotation_left_right_in_degrees =90 + 90.0-angle
	if is_height_up and is_facing_up:
		wheel_rotation_left_right_in_degrees = -angle
	if is_height_up and not is_facing_up:
		wheel_rotation_left_right_in_degrees = -90 - (90.0-angle)

	if angle_up_to_forward==90.0 and wheel_rotation_left_right_in_degrees ==-180.0:
		wheel_rotation_left_right_in_degrees = 0.0


	return wheel_rotation_left_right_in_degrees


# func rotate_point_on_y_axis(point: Vector3, angle_in_degrees: float) -> Vector3:
# 	var angle_in_radians = deg_to_rad(angle_in_degrees)
# 	var cos_angle = cos(angle_in_radians)
# 	var sin_angle = sin(angle_in_radians)

# 	var rotated_x = point.x * cos_angle - point.z * sin_angle
# 	var rotated_z = point.x * sin_angle + point.z * cos_angle

# 	return Vector3(rotated_x, point.y, rotated_z)

```


En bref, la méthode consiste à générer un deuxième point sur la droite, puis à comparer les hauteurs afin d’appliquer de la trigonométrie sur l’inclinaison.   
  
