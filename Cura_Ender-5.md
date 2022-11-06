# My Ender-5

These are my settings for the "Obnoxious" Ender-5
 
## Settings

### Hotend PID

Results after PID tuning:

```
Send: M303 E0 S240 C10
...cut...
Recv:  Classic PID
Recv:  Kp: 18.20 Ki: 1.27 Kd: 65.17
Recv: PID Autotune finished! Put the last Kp, Ki and Kd constants from below into Configuration.h
Recv: #define DEFAULT_Kp 18.20
Recv: #define DEFAULT_Ki 1.27
Recv: #define DEFAULT_Kd 65.17
Recv: //action:notification Obnoxious E5 Ready.
```

Commands to save new values:

```
M301 P18.20 I1.27 D65.17
M500
```

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

; Concurrent preheating 
M140 S{material_bed_temperature_layer_0}  ;Set bed temp
M104 S160 T0                              ;Set no-ooze hotend temp
M190 S{material_bed_temperature_layer_0}  ;Wait for bed to warm up 
M109 S160 T0                              ;Wait for hotend to warm up
M82                                       ;absolute extrusion mode (from orginal Cura G-code)

; Mostly Cura default for Ender-5
M201 X500.00 Y500.00 Z100.00 E5000.00  ;Setup machine max acceleration
M203 X500.00 Y500.00 Z10.00 E50.00     ;Setup machine max feedrate
M204 P500.00 R1000.00 T500.00          ;Setup Print/Retract/Travel acceleration
M205 X8.00 Y8.00 Z0.40 E5.00           ;Setup Jerk
M220 S100                              ;Reset Feedrate
M221 S100                              ;Reset Flowrate

; Homing and auto-levelling
G28                                    ;Home
G29                                    ;Auto-levelling

; Final warm up
M140 S{material_bed_temperature_layer_0}       ;Set bed temp
M190 S{material_bed_temperature_layer_0}       ;Wait for bed to warm up 
M104 S{material_print_temperature_layer_0} T0  ;Set final hotend temp
M109 S{material_print_temperature_layer_0} T0  ;Wait for hotend to warm up

; Print prime line
G92 E0                                 ;Reset Extruder
G0 Z2.0 F3000                          ;Move Z Axis up
G1 X-0.7 Y20 Z0.28 F5000.0             ;Move to start position
G1 X-0.7 Y200.0 Z0.28 F1500.0 E15      ;Draw the first line
G1 X-0.4 Y200.0 Z0.28 F5000.0          ;Move to side a little
G1 X-0.4 Y20 Z0.28 F1500.0 E30         ;Draw the second line
G92 E0                                 ;Reset Extruder
G1 Z2.0 F3000                          ;Move Z Axis up
```

### End G-code

```
G91                                  ;Relative positioning
G1 E-2 F2700                         ;Retract a bit
G1 E-2 Z0.2 F2400                    ;Retract and raise Z
G1 X5 Y5 F3000                       ;Wipe out
G1 Z10                               ;Raise Z more
G90                                  ;Absolute positioning

G1 X{machine_width} Y{machine_depth} ;Present print
M106 S0                              ;Turn-off fan
M104 S0                              ;Turn-off hotend
M140 S0                              ;Turn-off bed

M18 X Y Z E                          ;Disable all steppers
```




## References:

### Cura 5.2.1 Stock Profile

### Stock Start G-Code

```
M201 X500.00 Y500.00 Z100.00 E5000.00 ;Setup machine max acceleration
M203 X500.00 Y500.00 Z10.00 E50.00 ;Setup machine max feedrate
M204 P500.00 R1000.00 T500.00 ;Setup Print/Retract/Travel acceleration
M205 X8.00 Y8.00 Z0.40 E5.00 ;Setup Jerk
M220 S100 ;Reset Feedrate
M221 S100 ;Reset Flowrate

G28 ;Home

G92 E0 ;Reset Extruder
G1 Z2.0 F3000 ;Move Z Axis up
G1 X10.1 Y20 Z0.28 F5000.0 ;Move to start position
G1 X10.1 Y200.0 Z0.28 F1500.0 E15 ;Draw the first line
G1 X10.4 Y200.0 Z0.28 F5000.0 ;Move to side a little
G1 X10.4 Y20 Z0.28 F1500.0 E30 ;Draw the second line
G92 E0 ;Reset Extruder
G1 Z2.0 F3000 ;Move Z Axis up
```

### Stock End G-Code

```
G91 ;Relative positioning
G1 E-2 F2700 ;Retract a bit
G1 E-2 Z0.2 F2400 ;Retract and raise Z
G1 X5 Y5 F3000 ;Wipe out
G1 Z10 ;Raise Z more
G90 ;Absolute positioning

G28 X0 Y0 ;Present print
M106 S0 ;Turn-off fan
M104 S0 ;Turn-off hotend
M140 S0 ;Turn-off bed

M84 X Y E ;Disable all steppers but Z
```