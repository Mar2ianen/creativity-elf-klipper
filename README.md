# Creativity Elf Klipper setup

Klipper bring-up notes and configuration for a Creativity Elf with an MKS Robin Nano V1.2 board.

## Current status

- Klipper MCU firmware is installed and the host connected successfully from this Linux PC.
- The user reports that all mechanical endstops are repaired and working. X/Y polarity and homing were checked: X homes right to 300, Y homes toward the screen/front to 0. Both reported `TRIGGERED` at the switches and returned to `open` after moving 50 mm inward.
- Z/Z1 polarity was adjusted so released switches should report `open`, but those inputs have not yet been pressed and checked after the change. The Z1 input mapping to PC4 is still provisional; verify Z/Z1 before Z homing.
- The print head has been changed. This phase-one config intentionally has no heater, extruder, or probe sections.
- The Y− direction toward the front and X+ direction toward the right were confirmed during homing. See the status notes before continuing.

This is an in-progress bring-up config, not a complete print-ready printer configuration.

## Files

- config/printer.cfg — conservative endstop and motion bring-up config.
- firmware/Robin_nano.bin — Klipper MCU firmware built for the Robin Nano V1.2.
- firmware/BUILD.md — build settings, source revision, and checksum.
- docs/STATUS.md — hardware state, endstop observations, and last manual jogs.
- docs/ADDENDUM-2026-09-23.md — repaired endstops and polarity update.

## Important

The current Klipper session has X/Y homed and is at X=250, Y=50. Z is not homed. A fresh Klipper host session starts unhomed; recheck endstop states before homing.
