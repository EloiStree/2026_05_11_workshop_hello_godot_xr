```gdscript
class_name SSD1306MouseClickToNodeMove
extends Node3D

@export var main_camera: Camera3D
@export var what_to_move: Node3D

# === Configuration ===
@export var use_collision_mask: bool = false
@export_flags_3d_physics var collision_mask: int = 1

@export var use_black_list: bool = false
@export var black_list_group: Array[StringName] = []

@export var use_white_list: bool = false
@export var white_list_group: Array[StringName] = []

# New: Choose what to collide with
@export var collide_with_bodies: bool = true
@export var collide_with_areas: bool = true

@export var use_on_click_move: bool = true
@export var use_on_pressing_move: bool = true


func _input(event: InputEvent) -> void:
	if use_on_click_move and event is InputEventMouseButton and event.pressed and event.button_index == MOUSE_BUTTON_LEFT:
		move_to_click(event.position)
	elif use_on_pressing_move and event is InputEventMouseMotion and Input.is_mouse_button_pressed(MOUSE_BUTTON_LEFT):
		move_to_click(event.position)



func move_to_click(mouse_pos: Vector2) -> void:
	if not main_camera or not what_to_move:
		return

	var from: Vector3 = main_camera.project_ray_origin(mouse_pos)
	var direction: Vector3 = main_camera.project_ray_normal(mouse_pos)
	var to: Vector3 = from + direction * 2000.0  # Increased range

	var space_state := get_world_3d().direct_space_state

	var query := PhysicsRayQueryParameters3D.create(from, to)
	query.collide_with_bodies = collide_with_bodies
	query.collide_with_areas = collide_with_areas
	query.collision_mask = collision_mask if use_collision_mask else 0xFFFFFFF

	var result: Dictionary = space_state.intersect_ray(query)

	if not result or not result.has("collider"):
		return

	var collider = result.collider

	# Filtering
	if use_black_list and not black_list_group.is_empty():
		for group in black_list_group:
			if collider.is_in_group(group):
				return

	if use_white_list and not white_list_group.is_empty():
		var in_white_list := false
		for group in white_list_group:
			if collider.is_in_group(group):
				in_white_list = true
				break
		if not in_white_list:
			return

	# Optional: Extra safety for Area3D
	if collider is Area3D and not collider.monitoring:
		return

	what_to_move.global_position = result.position

```
