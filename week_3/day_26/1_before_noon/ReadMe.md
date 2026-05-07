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
