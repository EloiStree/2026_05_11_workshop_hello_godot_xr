``` gdscript
class_name SSD1306MouseEventForDebug
extends Node

signal on_scroll_up()
signal on_scroll_down()
signal on_scroll_left()
signal on_scroll_right()

signal on_left_click()
signal on_right_click()
signal on_middle_click()
signal on_left_release()
signal on_right_release()
signal on_middle_release()

signal on_left_click_changed(is_pressing: bool)
signal on_right_click_changed(is_pressing: bool)
signal on_middle_click_changed(is_pressing: bool)

@export var is_clicking_left: bool = false
@export var is_clicking_right: bool = false
@export var is_clicking_middle: bool = false

func _input(event: InputEvent) -> void:
	if event is InputEventMouseButton:
		# --- Scroll handling ---
		if event.pressed:
			match event.button_index:
				MOUSE_BUTTON_WHEEL_UP:
					on_scroll_up.emit()
				MOUSE_BUTTON_WHEEL_DOWN:
					on_scroll_down.emit()
				MOUSE_BUTTON_WHEEL_LEFT:
					on_scroll_left.emit()
				MOUSE_BUTTON_WHEEL_RIGHT:
					on_scroll_right.emit()

		# --- Click handling ---
		match event.button_index:
			MOUSE_BUTTON_LEFT:
				_handle_click(event.pressed, "left")
			MOUSE_BUTTON_RIGHT:
				_handle_click(event.pressed, "right")
			MOUSE_BUTTON_MIDDLE:
				_handle_click(event.pressed, "middle")


func _handle_click(is_pressing: bool, button: String) -> void:
	match button:
		"left":
			var changed := is_pressing != is_clicking_left
			is_clicking_left = is_pressing
			if changed:
				if is_pressing:
					on_left_click.emit()
				else:
					on_left_release.emit()
				on_left_click_changed.emit(is_pressing)

		"right":
			var changed := is_pressing != is_clicking_right
			is_clicking_right = is_pressing
			if changed:
				if is_pressing:
					on_right_click.emit()
				else:
					on_right_release.emit()
				on_right_click_changed.emit(is_pressing)

		"middle":
			var changed := is_pressing != is_clicking_middle
			is_clicking_middle = is_pressing
			if changed:
				if is_pressing:
					on_middle_click.emit()
				else:
					on_middle_release.emit()
				on_middle_click_changed.emit(is_pressing)



```



