# My Ender-5

These are my settings for the "Obnoxious" Ender-5
 
## Settings

### Printer

* X (Width): 220.0 mm
* Y (Depth): 220.0 mm
* Z (Height): 300.0 mm
* Origina at center: No
* Heated bed: Yes
* Heated build volume: No
* G-code flavor: Marlin

### Printhead

* X min: -26 mm
* Y min: -32 mm
* X max: 32 mm
* Y max: 34 mm
* Gantry Height: 25.0 mm
* Number of extruders: 1
* Apply Extruder offsets to GCode: Yes

### Start G-code

```
; Adi Ender-5 Custom Start G-code

; Concurrent preheating (overrride Cura steam engine auto generated)
M140 S{material_bed_temperature_layer_0} ; start preheating the bed
M104 S{material_print_temperature_layer_0} ﻿T0 ; start preheating hotend
M190 S{material_bed_temperature_layer_0} ; heat to Cura Bed setting 
M109 S{material_print_temperature_layer_0} ﻿T0 ; heat to Cura Hotend
M82 ;absolute extrusion mode (from orginal Cura G-code)

; Mostly Cura default for Ender-5
M201 X500.00 Y500.00 Z100.00 E5000.00 ;Setup machine max acceleration
M203 X500.00 Y500.00 Z10.00 E50.00 ;Setup machine max feedrate
M204 P500.00 R1000.00 T500.00 ;Setup Print/Retract/Travel acceleration
M205 X8.00 Y8.00 Z0.40 E5.00 ;Setup Jerk
M220 S100 ;Reset Feedrate
M221 S100 ;Reset Flowrate

G28 ;Home
G29 ;Auto-levelling

G92 E0 ;Reset Extruder
G1 Z2.0 F3000 ;Move Z Axis up
G1 X-0.7 Y20 Z0.28 F5000.0 ;Move to start position
G1 X-0.7 Y200.0 Z0.28 F1500.0 E15 ;Draw the first line
G1 X-0.4 Y200.0 Z0.28 F5000.0 ;Move to side a little
G1 X-0.4 Y20 Z0.28 F1500.0 E30 ;Draw the second line
G92 E0 ;Reset Extruder
G1 Z2.0 F3000 ;Move Z Axis up
```

### End G-code

```
G91 ;Relative positioning
G1 E-2 F2700 ;Retract a bit
G1 E-2 Z0.2 F2400 ;Retract and raise Z
G1 X5 Y5 F3000 ;Wipe out
G1 Z10 ;Raise Z more
G90 ;Absolute positioning

G1 X{machine_width} Y{machine_depth} ;Present print
M106 S0 ;Turn-off fan
M104 S0 ;Turn-off hotend
M140 S0 ;Turn-off bed

M18 X Y Z E ;Disable all steppers
```
