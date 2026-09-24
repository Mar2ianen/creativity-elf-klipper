# Creativity Elf Klipper setup

Klipper bring-up notes and configuration for a Creativity Elf with an MKS Robin Nano V1.2 board.

## Current status

- Klipper host was manually started on this Linux PC through the CH340 USB serial adapter for the sensor-only session; the Orange Pi 3 LTS is still the planned permanent host.
- The user reports that all mechanical endstops are repaired and working. X/Y polarity and homing were checked: X homes right to 300, Y homes toward the screen/front to 0. Both reported `TRIGGERED` at the switches and returned to `open` after moving 50 mm inward.
- Z switches were checked individually: right Z maps to PA11/`stepper_z`, left Z maps to PC4/`z1`; both switch between `open` and `TRIGGERED`. A low-speed `G28 Z` completed after reversing both Z motor directions, and both switches reported `TRIGGERED` at home. Independent motor movement and gantry alignment still need checking.
- The bed thermistor on PC0/TB and heater output on PA0/H-BED were tested on the printer. The bed reached 80°C in stages and Klipper PID calibration completed at 80°C (`Kp=64.814`, `Ki=1.583`, `Kd=663.529`). The bed thermistor type follows the upstream Robin Nano config but has not been independently matched to factory firmware.
- The changed toolhead hotend thermistor is configured read-only on PC1 using the provisional upstream `ATC Semitec 104GT-2` curve. It read 18.39–22.16°C over a 30-second room-temperature poll, so its type or connection needs checking before any hotend heating. No hotend heater or extruder is configured.
- The heatsink fan is exposed as a manual `fan_generic` on PB1 and reports speed 0 after Klipper startup. It is not yet temperature-coupled; fan wiring and spin have not been tested.
- The Y− direction toward the front and X+ direction toward the right were confirmed during homing. See the status notes before continuing.

This is an in-progress bring-up config, not a complete print-ready printer configuration.

## Files

- config/printer.cfg — conservative endstop and motion bring-up config.
- firmware/Robin_nano.bin — Klipper MCU firmware built for the Robin Nano V1.2.
- firmware/BUILD.md — build settings, source revision, and checksum.
- docs/STATUS.md — hardware state, endstop observations, and last manual jogs.
- docs/ADDENDUM-2026-09-23.md — repaired endstops and polarity update.
- docs/ADDENDUM-2026-09-23-bed-heater-pid.md — staged bed heater test and PID calibration.
- docs/ADDENDUM-2026-09-24-hotend-sensor-fan.md — read-only hotend sensor and fan startup state.

## Important

Latest live status: bed 20.79°C, target 0°C, power 0; hotend sensor 22.47°C using a provisional curve; heatsink fan speed 0. Klipper is `Ready`. All axes are unhomed after starting a fresh host session. No heater was activated during the hotend sensor setup.
