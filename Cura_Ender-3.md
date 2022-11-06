# My Ender-3

These are my settings for the "Hilarious" Ender-3

## Versions

### Binder Clips

Cloning Cura's stock **Ender-3** profile yields no go areas for a build plate attached with binder clips, such as a glass plate.

### Magnetic Build Plate

Cloning Cura's stock **Ender-3 Pro** profile yields no go areas for a build plate attached with magnetic
## Settings

### Printer

* X (Width): 220.0 mm
* Y (Depth): 220.0 mm
* Z (Height): 250.0 mm
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
; Adi Ender-3 Custom Start G-code

; Concurrent preheating 
M140 S{material_bed_temperature_layer_0}  ;Set bed temp
M104 S160 T0                              ;Set no-ooze hotend temp
M190 S{material_bed_temperature_layer_0}  ;Wait for bed to warm up 
M109 S160 T0                              ;Wait for hotend to warm up
M82                                       ;absolute extrusion mode (from orginal Cura G-code)

; Homing and auto-levelling
G28                                ;Home all axes
G29                                ;Auto-levelling

; Final warm up
M140 S{material_bed_temperature_layer_0}       ;Set bed temp
M190 S{material_bed_temperature_layer_0}       ;Wait for bed to warm up 
M104 S{material_print_temperature_layer_0} T0  ;Set final hotend temp
M109 S{material_print_temperature_layer_0} T0  ;Wait for hotend to warm up

; Print prime line
G92 E0                             ;Reset Extruder
G1 Z2.0 F3000                      ;Move Z Axis up little to prevent scratching of Heat Bed
G1 X-0.7 Y6 Z0.3 F5000.0           ;Move to start position
G1 X-0.7 Y200.0 Z0.3 F1500.0 E15   ;Draw the first line
G1 X-0.4 Y200.0 Z0.3 F5000.0       ;Move to side a little
G1 X-0.4 Y6 Z0.3 F1500.0 E30       ;Draw the second line
G92 E0                             ;Reset Extruder
G1 Z2.0 F3000                      ;Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0             ;Move over to prevent blob squish
```

### End G-code

```
; Ender 3 Custom End G-code
G91                                ;Relative positioning
G1 E-2 F2700                       ;Retract a bit
G1 E-2 Z0.2 F2400                  ;Retract and raise Z
G1 X5 Y5 F3000                     ;Wipe out
G1 Z10                             ;Raise Z more
G90                                ;Absolute positioning

;G1 X0 Y{machine_depth}            ;Present print
G1 X{machine_width} Y110           ;Present print

M106 S0                            ;Turn-off fan
M104 S0                            ;Turn-off hotend
M140 S0                            ;Turn-off bed

M18 X Y Z E                        ;Disable all steppers
```



## References:

### Cura 5.2.1 Stock Profile

### Stock Start G-Code

```
; Ender 3 Custom Start G-code
G92 E0 ; Reset Extruder
G28 ; Home all axes
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position
G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line
G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little
G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0 ; Move over to prevent blob squish
```

### Stock End G-Code

```
G91 ;Relative positioning
G1 E-2 F2700 ;Retract a bit
G1 E-2 Z0.2 F2400 ;Retract and raise Z
G1 X5 Y5 F3000 ;Wipe out
G1 Z10 ;Raise Z more
G90 ;Absolute positioning

G1 X0 Y{machine_depth} ;Present print
M106 S0 ;Turn-off fan
M104 S0 ;Turn-off hotend
M140 S0 ;Turn-off bed

M84 X Y E ;Disable all steppers but Z
```