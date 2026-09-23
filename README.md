# Creativity Elf Klipper setup

Klipper bring-up notes and configuration for a Creativity Elf with an MKS Robin Nano V1.2 board.

## Current status

- Klipper MCU firmware is installed and the host connected successfully from this Linux PC.
- The user reports that all mechanical endstops are repaired and working, including Y.
- Endstop logic polarity for X, Z, and Z1 was reversed after a 30-second poll showed `TRIGGERED` while those physically released switches were untouched. Y toggled during the poll. Verify every released switch reports `open` and every pressed switch reports `TRIGGERED` before homing. The Z1 input mapping to PC4 is still provisional.
- The print head has been changed. This phase-one config intentionally has no heater, extruder, or probe sections.
- The Y− direction was confirmed toward the front with a slow relative move. X+ was commanded toward the right; physical direction confirmation is pending. See the status notes before continuing.

This is an in-progress bring-up config, not a complete print-ready printer configuration.

## Files

- config/printer.cfg — conservative endstop and motion bring-up config.
- firmware/Robin_nano.bin — Klipper MCU firmware built for the Robin Nano V1.2.
- firmware/BUILD.md — build settings, source revision, and checksum.
- docs/STATUS.md — hardware state, endstop observations, and last manual jogs.
- docs/ADDENDUM-2026-09-23.md — repaired endstops and polarity update.

## Important

The machine has not been homed. Coordinates used for the small jogs were set from the user's manual placement at the center and are not calibrated coordinates. After starting a fresh Klipper host, treat all axes as unhomed. Recheck endstop states with the updated config before any homing.
