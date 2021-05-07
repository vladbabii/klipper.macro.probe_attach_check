# Probe Attach Check Klipper Macro

Runs one of the two specified macros depending if the probe is mounted or not.
This is intended to be used with a detachable probe (magnetic or manual) that is active (high) when disconnected or it hits something

Usage:
```
PROBE_ATTACH_CHECK ATTACHED="MACRO_1_NAME" DETACHED="MACRO_2_NAME"
```

