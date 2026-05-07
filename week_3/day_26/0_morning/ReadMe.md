**Objectif:** Raycast pour capteur ultrason   
  
On a déjà utilisé des Area3D avec des layers, des groupes et des tags de script.   
Les Area3D sont pratiques, mais parfois un simple rayon fait très bien le travail.   

Voyons comment utiliser le RayCast3D de Godot pour mesurer la distance à un objet.   

Cela permettra de donner des “yeux” à notre voiture, voire de créer un laser si vous en avez envie.    

**Challenge Raycast :**     
[https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors/issues/28](https://github.com/EloiStree/2026_04_11_gdp_learn_with_sensors/issues/28)    


https://github.com/DmitriySalnikov/godot_debug_draw_3d  
```DebugDraw3D.draw_line(line_begin, line_end, Color(1, 1, 0))```




----------------

----------------

Une solution

``` gdscript
class_name SensorToolRaycastToDistance
extends Node


signal on_detecting_collision(is_colliding: bool)
signal on_start_collision()
signal on_stop_collision()
signal on_colliding_point_updated(collision_point: Vector3)
signal on_distance_updated(distance: float)
signal on_distance_as_percent_updated(distance: float)
signal on_node_hit_changed(node: Node)

@export var ray_cast_to_use:RayCast3D

@export var value_to_convert_in_percent_distance: float = 0.5
@export var clamp_percent_distance_to_0_1: bool = true


@export_group("Debug")
@export var is_colliding: bool = false
@export var hit_point_distance: float = 0.0
@export var hit_node: Node = null


func _process(delta: float) -> void:
	if not ray_cast_to_use:
		return

	ray_cast_to_use.force_raycast_update()
	var was_colliding := is_colliding
	is_colliding = ray_cast_to_use.is_colliding()

	if is_colliding and not was_colliding:
		on_start_collision.emit()
	elif not is_colliding and was_colliding:
		on_stop_collision.emit()

	if is_colliding:
		var collision_point := ray_cast_to_use.get_collision_point()
		on_colliding_point_updated.emit(collision_point)
		hit_point_distance = collision_point.distance_to(ray_cast_to_use.global_position)
		on_distance_updated.emit(hit_point_distance)
		var percent_distance := hit_point_distance / value_to_convert_in_percent_distance
		if clamp_percent_distance_to_0_1:
			percent_distance = clamp(percent_distance, 0.0, 1.0)
		on_distance_as_percent_updated.emit(percent_distance)
	else :
		on_colliding_point_updated.emit(Vector3.ZERO)
		on_distance_updated.emit(0.0)
		on_distance_as_percent_updated.emit(0.0)

	on_detecting_collision.emit(is_colliding)

	var node_that_hit = ray_cast_to_use.get_collider() if is_colliding else null
	if node_that_hit != hit_node:
		hit_node = node_that_hit
		on_node_hit_changed.emit(hit_node)


	var start_point: Vector3 = ray_cast_to_use.global_position
	var q_forward: Vector3 = -ray_cast_to_use.global_transform.basis.z.normalized()
	var end = ray_cast_to_use.to_global(ray_cast_to_use.target_position)
	var end_point: Vector3 = start_point + end
	if is_colliding:
		end_point = ray_cast_to_use.get_collision_point()
	
	DebugDraw3D.draw_line(start_point, end_point, Color(1, 1, 0),0.05)

	


```
