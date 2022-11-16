# Godot Math Extensions
Additional math helper functions and behaviors for manipulating angles and floats.

I'm hopeful that the methods in `MathEx` will be added to Godot 4, as the [interpolation example](https://docs.godotengine.org/en/latest/tutorials/math/interpolation.html#smoothing-motion) (`lerp` and `lerp_angle`) in the documentation is frame-rate dependent.

This extension also includes `FloatSmooth` (based on the same smoothing function as Unity's `SmoothDamp`). Though I find the spring function's timing imprecise, and am still searching for a more accurately controllable solution.

## Usage

### Helper functions (MathEx)
Helper functions are contained in the `MathEx` class:
```gdscript
var angle = MathEx.angle(deg_to_rad(0), deg_to_rad(90))
```
Currently this comprises:
```gdscript
angle(from: float, to: float) -> float
move_toward_angle(from: float, to: float, delta: float) -> float
```

### Float smoothing (FloatSmooth)
`FloatSmooth` is a small class for smoothly modifying a float value. 

Due to the spring model requiring two tracked properties (`value` and `velocity`), the options for implementing it in Godot were either to return an Array/Dictionary, or to track both in a dedicated class. This is the latter.

To use it, create a variable — this should be persistent, don't create a new one each time:
```gdscript
var my_value := FloatSmooth.new(1.0)
```
To smoothly modify the value, use `move_toward`. This returns the updated value for immediate use:
```gdscript
func _process(delta: float) -> void:
    var result := my_value.move_toward(10, delta)
    print(result)
```
For angles (in radians), use `move_toward_angle`. Which will correctly wrap around `TAU`:
```gdscript
var result := my_value.move_toward_angle(deg_to_rad(90), delta)
```
Use `set_instant` to change the value without smoothing. This clears the current velocity:
```gdscript
my_value.set_instant(0.0)
```

***FloatSmooth* is developed with thanks to:**<br />
Lowe, T. (2004). Critically damped ease-in/ease-out smoothing.<br />
In A, Kirmse (Ed.), *Game programming gems 4* (pp. 95-101). Charles River Media, Inc.