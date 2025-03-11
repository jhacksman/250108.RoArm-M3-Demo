# 01-InfoPrint0 Fix

This folder contains the fix that changes InfoPrint from 2 to 0 in the RoArm-M3_config.h file.

## Issue Description
When InfoPrint is set to 2, the system experiences significant lag in leader/follower mode and commands sent through the web interface (192.168.4.1) become corrupted by a continuous stream of telemetry data.

## What t 1051 is doing
The t 1051 code is part of the `RoArmM3_infoFeedback()` function in RoArm-M3_module.h. This function:
- Creates a JSON object with position data (x, y, z coordinates)
- Includes joint angles (b, s, e, t, r, g)
- Includes torque/load information for each servo
- Sets a type identifier "T" = 1051
- Serializes this data and sends it to the Serial output

## How often it's running
When InfoPrint=2, this function is called in **every loop iteration** of the main program loop. This means it's constantly sending position and torque data to the Serial port at the maximum rate the microcontroller can process the main loop.

## Impact on leader-follower mode
This is affecting the leader-follower mode functionality for these reasons:

1. **Serial port congestion**: The constant Serial.println output in every loop iteration creates significant overhead and can slow down the main loop execution.

2. **Timing interference**: The ESP-NOW leader-follower communication happens in the same main loop where the Serial output is occurring, potentially causing timing issues or delays.

3. **Resource competition**: Both the Serial output and ESP-NOW communication are competing for system resources, with the Serial output potentially taking priority or interrupting the ESP-NOW communication.

4. **No delay between outputs**: There's no delay between Serial outputs when InfoPrint=2, which means it's flooding the Serial port continuously.

## Fix Implementation
The fix changes InfoPrint from 2 to 0 in the RoArm-M3_config.h file to stop the continuous feedback and improve the performance of the leader-follower mode by removing this overhead.
