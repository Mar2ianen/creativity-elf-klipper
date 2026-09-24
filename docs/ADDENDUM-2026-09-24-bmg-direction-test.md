# Addendum — 2026-09-24: BMG feeder direction test

The user detached the BMG feeder from the hotend and asked for a 20 mm feed to check direction. The printer was `Ready`; the bed and hotend targets and heater powers were all 0°C/0. No normal `[extruder]` section is configured yet, so a temporary `[manual_stepper bmg_test]` was used without changing the hotend temperature interlock.

The test uses the Robin Nano E0 stepper outputs `PD6` (step), `!PD3` (direction), and `!PB3` (enable), matching the upstream Klipper generic config for Robin Nano V1.2. The initial BMG value is `rotation_distance: 7.71` with `microsteps: 16`, equivalent to about 415 steps/mm for a 1.8° motor. This is the manufacturer baseline; the actual motor step angle is not yet confirmed, and feed length has not been calibrated.

The manual stepper is limited to a 20 mm position and set to 2 mm/s with 5 mm/s² acceleration. Two 20 mm moves were accepted by Klipper, with `SET_POSITION=0` before each move and the motor disabled afterward. The user observed the positive move feeding filament out from the feeder's hotend side, confirming the current `!PD3` direction sends filament toward the hotend. The physical amount moved was not measured.

References: [Klipper generic Robin Nano V1.2 config](https://github.com/Klipper3d/klipper/blob/master/config/generic-mks-robin-nano-v1.cfg), [Klipper rotation distance and BMG gear ratio](https://www.klipper3d.org/Rotation_Distance.html), [Bondtech BMG E-step baseline](https://www.bondtech.se/product/bmg-extruder/).
