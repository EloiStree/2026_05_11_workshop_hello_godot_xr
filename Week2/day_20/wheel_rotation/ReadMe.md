








------------------


Example of script unclean:    
``` gdscript

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

```
