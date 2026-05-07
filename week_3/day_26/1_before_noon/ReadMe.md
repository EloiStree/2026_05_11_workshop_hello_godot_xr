**Objectif : Raycast pour les capteurs de ligne**

Même idée, mais avec un peu de recherche et développement.

On va essayer de récupérer la couleur de l’objet touché et transformer cette donnée en vrai/faux. Cela permettra de créer des niveaux pour notre voiture.

**Challenge :**

* Junior : essayez de récupérer la couleur de l’objet touché.
* Senior : essayez de récupérer la texture UV à l’endroit où l’objet a été touché.








-------------
-------------


Une solution:

``` gdscript
class_name SensorToolNodeToMesh3DColor
extends Node 



signal on_color_albedo_found(color: Color)
signal on_color_emission_found( color: Color)

signal on_no_color_found()

@export var albedo_color_found: bool = false
@export var material_albedo_color: Color = Color(1,1,1)
@export var emission_color_found: bool = false
@export var material_emission_color: Color = Color(1,1,1)


@export var node_received: Node = null
@export var mesh_list_in_node:Array[MeshInstance3D] = []


func _clear_colors_info():
	albedo_color_found = false
	material_albedo_color = Color(0,0,0)
	emission_color_found = false
	material_emission_color = Color(0,0,0)

func clear_with_no_node():
	push_node_in(null)
	

func push_node_in(node:Node3D):
	_clear_colors_info() 
	if node == null:
		node_received = null
		mesh_list_in_node = []
		_clear_colors_info()
		on_no_color_found.emit()
		return
	node_received = node
	mesh_list_in_node = []

	var mesh_instance :MeshInstance3D = null

	if node is MeshInstance3D:
		mesh_instance = node as MeshInstance3D
		mesh_list_in_node = [mesh_instance]
		find_color_in_mesh_instance(mesh_instance)
	else:
		mesh_list_in_node = find_mesh_instances_in_node_recursively(node)
		if mesh_list_in_node.size() == 0:
			_clear_colors_info()
		else:
			for m in mesh_list_in_node  :
				if m != null:
					find_color_in_mesh_instance(m)

	if albedo_color_found:
		on_color_albedo_found.emit(material_albedo_color)
	if emission_color_found:
		on_color_emission_found.emit(material_emission_color)
	if not albedo_color_found and not emission_color_found:
		on_no_color_found.emit()



func find_mesh_instances_in_node_recursively(node: Node) -> Array[MeshInstance3D]:
	var mesh_instances: Array[MeshInstance3D] = []
	
	if not node:
		return mesh_instances
	
	# Check current node
	if node is MeshInstance3D:
		mesh_instances.append(node as MeshInstance3D)
	
	# Recurse on all children
	for child in node.get_children():
		mesh_instances.append_array(
			find_mesh_instances_in_node_recursively(child)
		)
	
	return mesh_instances


func find_color_in_mesh_instance(mesh_instance: MeshInstance3D) -> void:

	if mesh_instance == null:
		return
	if mesh_instance.material_override:
		_print_material_colors(mesh_instance.material_override)

	var mesh := mesh_instance.mesh
	if mesh == null:
		return

	for surface in mesh.get_surface_count():
		var override_mat = mesh_instance.get_surface_override_material(surface)
		if override_mat:
			_print_material_colors(override_mat)

	for surface in mesh.get_surface_count():
		var mat = mesh.surface_get_material(surface)
		if mat:
			_print_material_colors(mat)


func _print_material_colors(mat: Material) -> void:
	if mat is StandardMaterial3D:
		var std_mat := mat as StandardMaterial3D
		material_albedo_color = std_mat.albedo_color
		albedo_color_found = true
		if std_mat.emission_enabled:
			material_emission_color = std_mat.emission
			emission_color_found = true
		else :
			material_emission_color = Color(0,0,0)
			emission_color_found = false

```



Make a laser to debug the ray
``` gdscript
class_name SensorViewLaser3D
extends Node


@export var use_raycast_to_update_laser: bool = true
@export var ray_cast_3d: RayCast3D

@export var sphere_point_start_node:Node3D
@export var sphere_point_end_node:Node3D
@export var cylinder_ray_node:Node3D
@export var laser_radius: float = 0.05



@export_group("Debug")
@export var from_point: Vector3 = Vector3.ZERO
@export var to_point: Vector3 = Vector3.ZERO

func get_ray_cast_start() -> Vector3:
	if not use_raycast_to_update_laser:
		return from_point

	if not ray_cast_3d:
		return Vector3.ZERO	
	return ray_cast_3d.global_position

func get_ray_cast_end() -> Vector3:

	if not use_raycast_to_update_laser:
		return to_point


	if not ray_cast_3d:
		return Vector3.ZERO
	var start_point: Vector3 = ray_cast_3d.global_position
	var q_forward: Vector3 = -ray_cast_3d.global_transform.basis.z.normalized()
	var end = ray_cast_3d.to_global(ray_cast_3d.target_position)
	var end_point: Vector3 = start_point + end
	var is_colliding = ray_cast_3d.is_colliding()
	if is_colliding:
		end_point = ray_cast_3d.get_collision_point()
	return end_point	

func _process(delta: float) -> void:
	if not ray_cast_3d:
		return
	ray_cast_3d.force_raycast_update()	
	var start_point: Vector3 = get_ray_cast_start()
	var end_point: Vector3 = get_ray_cast_end()

	#DebugDraw3D.draw_line(start_point, end_point, Color(1, 0, 0),0.05)

	var direction: Vector3 = (end_point - start_point).normalized()
	var distance: float = start_point.distance_to(end_point)
	var q_direction: Quaternion = quat_from_direction(direction)
	
	if sphere_point_start_node:
		sphere_point_start_node.global_position = start_point
		sphere_point_start_node.scale = Vector3(laser_radius, laser_radius, laser_radius)
		
		# set direction
		sphere_point_start_node.rotation = q_direction.get_euler()

	if sphere_point_end_node:
		sphere_point_end_node.global_position = end_point
		sphere_point_end_node.scale = Vector3(laser_radius, laser_radius, laser_radius)
		sphere_point_end_node.rotation = q_direction.get_euler()

	if cylinder_ray_node:
		cylinder_ray_node.global_transform.origin = start_point+direction * distance * 0.5
		cylinder_ray_node.look_at(end_point, Vector3.UP)
		cylinder_ray_node.scale = Vector3(laser_radius, laser_radius, distance)
	


func quat_from_direction(direction: Vector3) -> Quaternion:
	var forward = Vector3(0, 0, -1)
	var dir = direction.normalized()

	var dot = forward.dot(dir)

	# Same direction
	if dot > 0.9999:
		return Quaternion.IDENTITY

	# Opposite direction
	if dot < -0.9999:
		return Quaternion(Vector3.UP, PI)

	var axis = forward.cross(dir).normalized()
	var angle = acos(dot)

	return Quaternion(axis, angle)
```
