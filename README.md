# Probe Attach Check Klipper Macro

Runs one of the two specified macros depending if the probe is mounted or not.
This is intended to be used with a detachable probe (magnetic or manual) that is active (high) when disconnected or it hits something

Dependency: https://github.com/vladbabii/klipper.macro.g28_override

The probe can be
* configured only in probe section
* used as z endstop in stepper z section

Usage:
```
PROBE_ATTACH_CHECK ATTACHED="MACRO_1_NAME" DETACHED="MACRO_2_NAME"
```

How it works
1. check endstop status
2. if endstop is not triggered then probe is mounted and runs macro from parameter (then stops)
3. (if endstop is triggered) move up [probe].sample_retract_dist mm (to check if probe is actually hitting something
4. check endstops status again
5. if endstop is triggered then probe is not mounted and run macro (then stop)
6. if endstop is not triggered then probe is mounted and run macro (then stop)


Example usage for checking a manual probe is attached before probing...
```
[probe]
## ....
activate_gcode:
  PROBE_ATTACH_CHECK DETACHED="PROBE_ERROR_NOT_PICKED_UP"

[gcode_macro PROBE_ERROR_NOT_PICKED_UP]
gcode:
  RESPOND PREFIX="error" MSG="Probe > ERROR > Probe is not picked up"
  M112 # Emergency stop
  
[gcode_macro PRINT_START]
gcode:
   ...
   PROBE_ATTACH_CHECK ATTACHED="PROBE_ERROR_PICKED_UP"
   
   ... heating, etc goes here

[gcode_macro PROBE_ERROR_PICKED_UP]
gcode:
  RESPOND PREFIX="error" MSG="Probe > ERROR > Probe is still attached!"
  M112 # Emergency stop
```
