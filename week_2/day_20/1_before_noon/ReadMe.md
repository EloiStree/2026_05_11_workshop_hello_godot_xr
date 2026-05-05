**Objectif :** Faire un compas

Exercice en soi assez simple. On veut savoir combien de degrés à gauche ou à droite se trouve l’objet.

En bref :
On a donc besoin d’un point à viser (`Vector3`).
De la direction *forward* vue du dessus (plan XZ sans le Y).
On calcule la direction du point avec : destination - origine.
On compare les deux directions.
Soit avec les outils de Godot pour calculer un angle,
Soit avec la trigonométrie.

---

**Une solution à cet exercice :**


```gdscript
class_name SSD1306RotationCompassFaceUp
extends Node



@export var direction_node: Node3D
@export var target_north_node:Node3D
@export var rotation_left_right_north_node_degrees: float = 0.0
@export var rotation_left_right_game_center_degrees: float = 0.0

# @export var use_debug_print: bool = false



func get_compass_rotation_to_godot_center_in_degrees_left_right() -> float:
	return rotation_left_right_game_center_degrees



func _process(_delta: float) -> void:

	var from_position: Vector3 = Vector3.ZERO
	var from_rotation: Quaternion = Quaternion.IDENTITY
	var to_position: Vector3 = Vector3.ZERO
	var angle: float = 0.0
	from_position = direction_node.global_position
	from_rotation = Quaternion.from_euler(direction_node.global_rotation)
	
	angle= compute_compass_angle_of_of_raw_value(from_position, from_rotation, to_position)
	rotation_left_right_game_center_degrees = angle

	# if use_debug_print:
	# 	print ("Compass Degree: ", rotation_left_right_game_center_degrees)

	if not target_north_node:
		return

	to_position = target_north_node.global_position
	angle = compute_compass_angle_of_of_raw_value(from_position, from_rotation, to_position)
	rotation_left_right_north_node_degrees = angle
	
	# if use_debug_print:
	# 	print ("Compass Degree to North Node: ", rotation_left_right_north_node_degrees)


func set_north_node(north_node: Node3D) -> void:
	target_north_node = north_node


func compute_compass_angle_of_given_north_node(north_node:Node3D) -> float:
	var position_compass = direction_node.global_position
	var rotation_compass = Quaternion.from_euler(direction_node.global_rotation)
	var position_north = north_node.global_position
	return compute_compass_angle_of_of_raw_value(position_compass, rotation_compass, position_north)


func compute_compass_angle_of_of_raw_value(position_compass: Vector3, rotation_compass:Quaternion, position_north: Vector3) -> float:
	var north_vector = position_north - position_compass
	var north_vector_2d = Vector2(north_vector.x, north_vector.z).normalized()
	var foward_vector = convert_quaternion_to_forward_vector(rotation_compass)
	var foward_vector_2d = Vector2(foward_vector.x, foward_vector.z).normalized()
	var center = Vector2.ZERO
	var angle = get_angle_beget_angle_between_three_point_in_degrees(center, foward_vector_2d, north_vector_2d)
	angle = fmod(angle + 180.0, 360.0) - 180.0
	if angle < -180.0:
		angle += 360.0
	return angle

func convert_quaternion_to_forward_vector(quaternion: Quaternion) -> Vector3:
	var forward = Vector3(0, 0, -1)
	return quaternion * forward	

func get_angle_between_three_point(center: Vector2, point_a: Vector2, point_b: Vector2) -> float:
	var vector_a = point_a - center
	var vector_b = point_b - center
	var angle = atan2(vector_b.y, vector_b.x) - atan2(vector_a.y, vector_a.x)
	return angle

func get_angle_beget_angle_between_three_point_in_degrees(center: Vector2, point_a: Vector2, point_b: Vector2) -> float:
	var angle = get_angle_between_three_point(center, point_a, point_b)
	return rad_to_deg(angle)


```
