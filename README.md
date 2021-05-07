# Probe Attach Check Klipper Macro

Runs one of the two specified macros depending if the probe is mounted or not.
This is intended to be used with a detachable probe (magnetic or manual) that is active (high) when disconnected or it hits something

Usage:
```
PROBE_ATTACH_CHECK ATTACHED="MACRO_1_NAME" DETACHED="MACRO_2_NAME"
```

How it works
1. check endstop status
2. if endstop is not triggered then probe is mounted and runs macro from parameter (then stops)
3. (if endstop is triggered) move up probe.sample_retract_dist
4. check endstops status again
5. if endstop is triggered then probe is not mounted and run macro (then stop)
6. if endstop is not triggered then probe is mounted and run macro (then stop)


Example usage for checking probe is picked up corectly 
```
[probe]
## ....
activate_gcode:
  PROBE_PICKUP ## macro to pickup probe
  PROBE_ATTACH_CHECK DETACHED="PROBE_ERROR_NOT_PICKED_UP"

[gcode_macro PROBE_ERROR_NOT_PICKED_UP]
gcode:
  RESPOND PREFIX="error" MSG="Probe > ERROR > Probe is not picked up"
  M112 # Emergency stop
```
This would allow, with a manually attached probe to check if the probe is attached before doing any actual probing with it
