



Un example:
``` gdscript
static func sent_ir_integer_to_receiver_in_cone(value: int, start_point:Vector3, distance_point:Vector3, angle_point:Vector3) -> Array[SensorIrReceiverSquare]:
	var distance_cone = (distance_point - start_point).length()
	var angle_cone = rad_to_deg((angle_point - start_point).angle_to((distance_point - start_point).normalized()))
	var receivers_in_cone:Array[SensorIrReceiverSquare] = []
	for receiver in SensorIrReceiverSquare.receiver_in_scene:
		var receiver_position:Vector3 = receiver.ir_forward_direction.global_transform.origin
		var start_to_receiver_direction = receiver_position - start_point
		var start_to_receiver_distance = start_to_receiver_direction.length()
		if start_to_receiver_distance > distance_cone:
			continue
		var angle_direction_to_receiver = rad_to_deg((distance_point - start_point).normalized().angle_to(start_to_receiver_direction.normalized()))
		if angle_direction_to_receiver > angle_cone:
			continue
		receivers_in_cone.append(receiver)
		receiver.notify_an_received_ir_integer_message(value)
	return receivers_in_cone
```
