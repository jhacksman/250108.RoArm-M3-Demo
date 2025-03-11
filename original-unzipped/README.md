# Original Unzipped Files

This folder contains the original unzipped files from RoArm-M3_example-250108.zip without any modifications.

## Issue Description
The original code has InfoPrint set to 2, which causes the RoArmM3_infoFeedback() function to run in every loop iteration (approximately 100Hz). This creates significant Serial port congestion and affects the ESP-NOW leader-follower mode functionality.

See the main repository README for a detailed analysis of the issue.
