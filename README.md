# MADs useful Godot 4.2 Utility Functions

This repository contains a set of utility functions for the Godot Engine. These functions are designed to assist with common tasks in game development, such as formatting times and numbers, and printing node paths in the scene tree. I use them across various Godot v4.2 projects.

Formats a race time given in milliseconds into a readable string.
```gdscript
format_race_time(3723000, true)
# Input: milliseconds = 3723000, full = true
# Output: "01:02:03.000"

format_race_time(61010, true)
# Input: milliseconds = 61010, full = true
# Output: "00:01:01.010"

format_race_time(3723000, false)
# Input: milliseconds = 3723000, full = false
# Output: "1:02:03.000"

format_race_time(61010, false)
# Input: milliseconds = 61010, full = false
# Output: "1:01.010"

func format_race_time(milliseconds: int, full: bool) -> String:
	var total_seconds: int = milliseconds / 1000
	var hours: int = total_seconds / 3600
	var minutes: int = (total_seconds % 3600) / 60
	var seconds: int = total_seconds % 60
	var ms: int = milliseconds % 1000

	if full:
		return str(hours).pad_zeros(2) + ":" + str(minutes).pad_zeros(2) + ":" + str(seconds).pad_zeros(2) + "." + str(ms).pad_zeros(3)

	if hours > 0:
		return str(hours) + ":" + str(minutes).pad_zeros(2) + ":" + str(seconds).pad_zeros(2) + "." + str(ms).pad_zeros(3)
	elif minutes > 0:
		return str(minutes) + ":" + str(seconds).pad_zeros(2) + "." + str(ms).pad_zeros(3)
	elif seconds > 0:
		return str(seconds) + "." + str(ms).pad_zeros(3)
	else:
		return str(ms)
```

Formats a given time in milliseconds into hours, minutes, seconds, and milliseconds.
```gdscript
format_ms_to_hours_minutes_seconds(3723000, true)
# Input: milliseconds = 3723000, full = true
# Output: "01h 02m 03s 000ms"

format_ms_to_hours_minutes_seconds(3723000, false)
# Input: milliseconds = 3723000, full = false
# Output: "1h 02m 03s 000ms"

func format_ms_to_hours_minutes_seconds(milliseconds: int, full: bool) -> String:
	var total_seconds: int = milliseconds / 1000
	var hours: int = total_seconds / 3600
	var minutes: int = (total_seconds % 3600) / 60
	var seconds: int = total_seconds % 60
	var ms: int = milliseconds % 1000

	if full:
		return str(hours).pad_zeros(2) + "h " + str(minutes).pad_zeros(2) + "m " + str(seconds).pad_zeros(2) + "s " + str(ms).pad_zeros(3) + "ms"

	if hours > 0:
		return str(hours) + "h " + str(minutes).pad_zeros(2) + "m " + str(seconds).pad_zeros(2) + "s " + str(ms).pad_zeros(3) + "ms"
	elif minutes > 0:
		return str(minutes) + "m " + str(seconds).pad_zeros(2) + "s " + str(ms).pad_zeros(3) + "ms"
	elif seconds > 0:
		return str(seconds) + "s " + str(ms).pad_zeros(3) + "ms"
	else:
		return str(ms) + "ms"
```

Formats a number by inserting commas at every thousandth place.
```gdscript
format_number_comma(1234567)
# Input: number = 1234567
# Output: "1,234,567"

func format_number_comma(number: int) -> String:
	var number_str: String = str(number)
	var formatted_number: String = ""
	var count: int = 0

	# Iterate over the number string in reverse
	for i: int in range(number_str.length() - 1, -1, -1):
		formatted_number = number_str[i] + formatted_number
		count += 1
		# Insert a comma every three digits, except at the start
		if count % 3 == 0 and i != 0:
			formatted_number = "," + formatted_number
	return formatted_number
```

Prints the node path from one node to another node in the scene tree.
More useful when its in a global autoload
```gdscript
print_node_path_to(self, "scoreboard_handler")
# Input: from_node_name = self, to_node_name = "scoreboard_handler"
# Output: Prints "The path from [starting node] to [target node] is [path]" or "Node not found: scoreboard_handler"
# global_autoload.print_node_path_to(self, "scoreboard_handler")

func print_node_path_to(from_node_name: Node, to_node_name: String) -> void:
	var target_node: Node = get_tree().get_root().find_child(to_node_name, true, false)
	if target_node:
		var path: NodePath = from_node_name.get_path_to(target_node)
		prints("The path from", from_node_name, "to", target_node, "is", path)
	else:
		prints("Node not found:", to_node_name)
```
