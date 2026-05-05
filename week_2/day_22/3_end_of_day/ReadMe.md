Objectif : Calculer le tilt et le raw

Si vous deviez créer un niveau à bulle en VR, vous devriez calculer son tilt et son raw à partir d’un quaternion.

On va encore utiliser un peu de trigonométrie 😋.


**Solution**:
``` gdscript
class_name SSD1306RotationTiltRawFaceUp
extends Node

@export var direction_node: Node3D
@export var wheel_rotation_in_degrees: float = 0.0

@export var use_debug_print: bool = false

@export var tilt_rotation_front_in_degrees_0_360_clockwise: float = 0.0
@export var raw_rotation_right_in_degrees_0_360_clockwise: float = 0.0

@export var percent_raw_right_01 : float = 0.0
@export var percent_tilt_front_01 : float = 0.0
@export var percent_raw_right_11 : float = 0.0
@export var percent_tilt_front_11 : float = 0.0

@export var tilt_degrees_180 :float = 0.0
@export var raw_degrees_180 :float = 0.0

@export var tilt_clamp_to_90_degrees: float = 0.0
@export var raw_clamp_to_90_degrees: float = 0.0



#region TILT GET
func get_tilt_rotation_in_degrees_percent_11() -> float:
	return percent_tilt_front_11

func get_tilt_rotation_in_degrees_angle_left_rigth_180() -> float:
	return tilt_degrees_180

func get_tilt_rotation_in_degrees_angle_left_rigth_clamp_90() -> float:
	return tilt_clamp_to_90_degrees

func get_tilt_rotation_in_degrees_0_360() -> float:
	return tilt_rotation_front_in_degrees_0_360_clockwise
#endregion

#region RAW GET
func get_raw_rotation_in_degrees_percent_11() -> float:
	return percent_raw_right_11

func get_raw_rotation_in_degrees_angle_left_rigth_180() -> float:
	return raw_degrees_180

func get_raw_rotation_in_degrees_angle_left_rigth_clamp_90() -> float:
	return raw_clamp_to_90_degrees

func get_raw_rotation_in_degrees_0_360() -> float:
	return raw_rotation_right_in_degrees_0_360_clockwise
#endregion







class TiltRawValues:
	var tilt_rotation_front_in_degrees_0_360_clockwise: float = 0.0
	var raw_rotation_right_in_degrees_0_360_clockwise: float = 0.0


static func get_tilt_raw_values_from_node(given_node: Node3D) -> TiltRawValues:
	return get_tilt_raw_values_from_global(Quaternion.from_euler(given_node.global_rotation))


static func get_tilt_raw_values_from_global( world_rotation_forward: Quaternion) -> TiltRawValues:
	
	var center:Vector3 = Vector3.ZERO
	var world_up = Vector3.UP
	
	var q_node_forward = world_rotation_forward
	var q_node_up = q_node_forward * Quaternion.from_euler(Vector3(deg_to_rad(90),0,0))
	var q_node_right = q_node_forward * Quaternion.from_euler(Vector3(0,deg_to_rad(-90),0))
	var v_forward = q_node_forward*Vector3(0,0,-1)
	var v_node_right = q_node_right*Vector3(0,0,-1)
	var v_node_up = q_node_up*Vector3(0,0,-1)
	var v_node_flat_forward = Vector3(v_forward.x,0,v_forward.z)
	var v_node_flat_right = Vector3(v_node_right.x,0,v_node_right.z)

	# https://godotengine.org/asset-library/asset/1766
	# DebugDraw3D.draw_line(center,world_up, Color.RED)
	# DebugDraw3D.draw_line(center,v_forward, Color.BLUE)
	# DebugDraw3D.draw_line(center,v_node_right, Color.RED)
	# DebugDraw3D.draw_line(center,v_node_up, Color.GREEN)
	# DebugDraw3D.draw_line(center,v_node_flat_right, Color.RED*0.5)
	# DebugDraw3D.draw_line(center,v_node_flat_forward, Color.BLUE*0.5)

		
	var is_facing_up = v_node_up.dot(world_up) > 0
	var is_facing_down = not is_facing_up

 ## COMPUTE THE RAW RIGHT CLOCKWISE ANGLE
	var opposed = distance(v_node_flat_right, v_node_right)
	var hypothenuse= distance(center, v_node_right)
	var adjacent = distance(center, v_node_flat_right)
	var clock_wise_angle = acos(clamp(adjacent / hypothenuse, -1.0, 1.0))
	var clock_wise_angle_degrees = rad_to_deg(clock_wise_angle)

	var trigo_angle_raw =clock_wise_angle_degrees
	var is_downward = sign(v_node_right.y- v_node_flat_right.y) < 0
	var is_upward = sign(v_node_right.y- v_node_flat_right.y) >= 0

	if is_facing_up and not is_downward:
		trigo_angle_raw = clock_wise_angle_degrees
	if is_facing_down and is_downward:
		trigo_angle_raw = 180.0-clock_wise_angle_degrees
	if is_facing_down and is_upward:
		trigo_angle_raw = 180.0+clock_wise_angle_degrees
	if is_facing_up and is_upward:
		trigo_angle_raw = 360.0-clock_wise_angle_degrees

## COMPUTE 	THE TILT CLOCKWISE ANGLE
	opposed = distance(v_node_flat_forward, v_forward)
	hypothenuse= distance(center, v_forward)
	adjacent = distance(center, v_node_flat_forward)
	var tilt_clock_wise_angle = acos(clamp(adjacent / hypothenuse, -1.0, 1.0))
	var tilt_clock_wise_angle_degrees = rad_to_deg(tilt_clock_wise_angle)

	var trigo_angle_tilt = tilt_clock_wise_angle_degrees
	is_downward = sign(v_forward.y- v_node_flat_forward.y) < 0
	is_upward = sign(v_forward.y- v_node_flat_forward.y) >= 0
	if is_facing_up and not is_downward:
		trigo_angle_tilt = tilt_clock_wise_angle_degrees
	if is_facing_down and is_downward:
		trigo_angle_tilt = 180.0-tilt_clock_wise_angle_degrees
	if is_facing_down and is_upward:
		trigo_angle_tilt = 180.0+tilt_clock_wise_angle_degrees
	if is_facing_up and is_upward:
		trigo_angle_tilt = 360.0-tilt_clock_wise_angle_degrees

	var result: TiltRawValues = TiltRawValues.new()
	result.tilt_rotation_front_in_degrees_0_360_clockwise = trigo_angle_tilt
	result.raw_rotation_right_in_degrees_0_360_clockwise = trigo_angle_raw
	return result



func _process(_delta: float) -> void:

	var tilt_raw_values = get_tilt_raw_values_from_node(direction_node)
	tilt_rotation_front_in_degrees_0_360_clockwise = tilt_raw_values.tilt_rotation_front_in_degrees_0_360_clockwise
	raw_rotation_right_in_degrees_0_360_clockwise = tilt_raw_values.raw_rotation_right_in_degrees_0_360_clockwise

	# from 0-360 to -180 to 180
	tilt_degrees_180 = fmod(tilt_rotation_front_in_degrees_0_360_clockwise + 180.0, 360.0) - 180.0
	raw_degrees_180 = fmod(raw_rotation_right_in_degrees_0_360_clockwise + 180.0, 360.0) - 180.0 

	# print ("Tilt Degrees: ", tilt_rotation_front_in_degrees_0_360_clockwise, " Raw: ", raw_rotation_right_in_degrees_0_360_clockwise)
	# print("180 Tilt Degrees: ", tilt_degrees_180, " 180 Raw Degrees: ", raw_degrees_180)

	percent_tilt_front_01 = (tilt_rotation_front_in_degrees_0_360_clockwise) / 360.0 
	percent_raw_right_01 = (raw_rotation_right_in_degrees_0_360_clockwise) / 360.0 
	percent_raw_right_11 = raw_degrees_180 /180.0
	percent_tilt_front_11 = tilt_degrees_180 /180.0

	tilt_clamp_to_90_degrees = clamp(tilt_degrees_180, -90.0, 90.0)
	raw_clamp_to_90_degrees = clamp(raw_degrees_180, -90.0, 90.0)
	
	

static func distance(point_a: Vector3, point_b: Vector3) -> float:
	return (point_b - point_a).length()


func inverse_rotation_from_direction_to_forward_vector(direction: Vector3) -> Quaternion:
	var forward = Vector3(0, 0, -1)
	return quaternion_from_two_directions(forward, direction)


func quaternion_from_two_directions(from_direction: Vector3, to_direction: Vector3) -> Quaternion:
	var from_normalized = from_direction.normalized()
	var to_normalized = to_direction.normalized()

	var dot_product = from_normalized.dot(to_normalized)

	if dot_product > 0.999999:
		return Quaternion.IDENTITY

	elif dot_product < -0.999999:
		var orthogonal_vector = Vector3(1, 0, 0).cross(from_normalized)
		if orthogonal_vector.length() < 0.000001:
			orthogonal_vector = Vector3(0, 1, 0).cross(from_normalized)

		orthogonal_vector = orthogonal_vector.normalized()
		return Quaternion(orthogonal_vector.x, orthogonal_vector.y, orthogonal_vector.z, 0)

	else:
		var cross_product = from_normalized.cross(to_normalized)
		return Quaternion(
			cross_product.x,
			cross_product.y,
			cross_product.z,
			1.0 + dot_product
		).normalized()


func convert_quaternion_to_forward_vector(quaternion: Quaternion) -> Vector3:
	var forward = Vector3(0, 0, -1)
	return quaternion * forward


```
