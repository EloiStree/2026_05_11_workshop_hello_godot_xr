

``` gdscript

const SCREEN_WIDTH: int = 128
const SCREEN_HEIGHT: int = 64
const SCREEN_SIZE: int = 128 * 64  # 8192

static func xy_lrtd_to_index(x: int, y: int) -> int  # Left-Right, Top-Down
static func index_to_xy_lrtd(index: int) -> Vector2i


# Basic pixel operations
func set_value_at_index_1d(index_0_8191: int, is_on: bool)
func get_value_at_index_1d(index_0_8191: int) -> bool
func set_value_at_x_y_lrtd(x_left_right: int, y_top_down: int, is_on: bool)
func get_value_at_x_y_lrtd(x_left_right: int, y_top_down: int) -> bool
func toggle_1d_value(index: int)
func toggle_2d_lrtd_value(x: int, y: int)

# Array operations
func get_value_as_1d_array_reference() -> Array[bool]
func get_value_as_1d_array_copy() -> Array[bool]
func set_value_with_1d_array(array: Array[bool])
func override_array_with_boolean_array(source_array: Array[bool])

# Drawing primitives
func draw_bool_fill_rectangle_lrtd(x_left_right: int, y_top_down: int, width: int, height: int, is_on: bool = true)
func draw_bool_line_up_lrtd(x_left_right: int, y_down_top: int, pixel: int, is_on: bool = true)
func draw_bool_line_down_lrtd(x_left_right: int, y_down_top: int, pixel: int, is_on: bool = true)
func draw_bool_line_right_lrtd(x_left_right: int, y_down_top: int, pixel: int, is_on: bool = true)
func draw_bool_line_left_lrtd(x_left_right: int, y_down_top: int, pixel: int, is_on: bool = true)

# Screen operations
func set_boolean_array_to_clear()
func set_boolean_array_to_full()
func inverse_all_boolean_value()
func inverse_boolean_horizontally()
func inverse_boolean_vertically()

# Shift operations
func shift_2d_boolean_array_left(loop_border: bool = true)
func shift_2d_boolean_array_right(loop_border: bool = true)
func shift_2d_boolean_array_up(loop_border: bool = true)
func shift_2d_boolean_array_down(loop_border: bool = true)

# Shape drawing
func draw_bool_center_circle_v2i_lrdt(point: Vector2i, radius: int, is_on: bool = true, fill: bool = true)
func draw_bool_center_square_v2i_lrdt(point: Vector2i, half_size: int, is_on: bool = true)
func draw_bool_line_v2i_lrdt(start: Vector2i, end: Vector2i, is_on: bool = true)

# Text display
func draw_bool_line_characters_6x8_lrtd(x_left_right: int, y_down_top: int, char: String, is_on: bool = true)
func draw_from_text_image_lrtd(x_left_right: int, y_down_top: int, text_image: String)

# Utility
func flush()
func fill()
func emit()

```
