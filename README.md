# Creativity Elf Klipper setup

Klipper bring-up notes and configuration for a Creativity Elf with an MKS Robin Nano V1.2 board.

## Current status

- Klipper host is being tested manually on this Linux PC through the CH340 USB serial adapter; the Orange Pi 3 LTS is still the planned permanent host.
- The user reports that all mechanical endstops are repaired and working. X/Y polarity and homing were checked: X homes right to 300, Y homes toward the screen/front to 0. Both reported `TRIGGERED` at the switches and returned to `open` after moving 50 mm inward.
- Z switches were checked individually: right Z maps to PA11/`stepper_z`, left Z maps to PC4/`z1`; both switch between `open` and `TRIGGERED`. A low-speed `G28 Z` completed after reversing both Z motor directions, and both switches reported `TRIGGERED` at home. Independent motor movement and gantry alignment still need checking.
- The bed thermistor on PC0/TB and heater output on PA0/H-BED were tested on the printer. The bed reached 80°C in stages and Klipper PID calibration completed at 80°C (`Kp=64.814`, `Ki=1.583`, `Kd=663.529`). The bed thermistor type follows the upstream Robin Nano config but has not been independently matched to factory firmware.
- The changed toolhead hotend is staged on PC3 with a 50°C limit and 50% maximum power. A 40°C target test was stopped at target; Klipper peaked near 47.7°C after power dropped to zero. The user measured 44°C with a thermocouple at its placement point and considered the check okay. Hotend PID has not been calibrated.
- The BMG feeder is temporarily configured as `[manual_stepper bmg_test]` using the Robin Nano E0 pins. The initial baseline is 415 steps/mm (`rotation_distance: 7.71`, assuming a 1.8° motor and 16 microsteps). The user observed positive motion feeding filament toward the hotend. Actual feed length has not been measured or calibrated.
- PB1 is exposed neutrally as `fan_generic fan_pb1`. A brief 40% command was accepted and returned to 0; the user reports the part-cooling blower did not spin, while the separate heatsink fan is running. Physical wiring from PB1 is unknown. The Robin Nano's HE1/PB0 is a heater output and has not been tested as a fan channel.
- The Y− direction toward the front and X+ direction toward the right were confirmed during homing. See the status notes before continuing.

This is an in-progress bring-up config, not a complete print-ready printer configuration.

## Files

- config/printer.cfg — conservative endstop and motion bring-up config.
- firmware/Robin_nano.bin — Klipper MCU firmware built for the Robin Nano V1.2.
- firmware/BUILD.md — build settings, source revision, and checksum.
- docs/STATUS.md — hardware state, endstop observations, and last manual jogs.
- docs/ADDENDUM-2026-09-23.md — repaired endstops and polarity update.
- docs/ADDENDUM-2026-09-23-bed-heater-pid.md — staged bed heater test and PID calibration.
- docs/ADDENDUM-2026-09-24-hotend-heater-fan-test.md — low-temperature hotend check and fan-channel observations.
- docs/ADDENDUM-2026-09-24-bmg-direction-test.md — BMG step baseline and verified feed direction.

## Important

Latest observed status after the BMG test: hotend and bed targets 0°C, heater power 0, PB1 command 0, Klipper `Ready`. The BMG test motor was disabled after its 20 mm moves. All axes are unhomed; BMG feed length still needs calibration.
