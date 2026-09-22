# Creativity Elf Klipper setup

Klipper bring-up notes and configuration for a Creativity Elf with an MKS Robin Nano V1.2 board.

## Current status

- Klipper MCU firmware is installed and the host connected successfully from this Linux PC.
- The printer's Y endstop wire is broken. Do not home Y until it is repaired and verified.
- The print head has been changed. This phase-one config intentionally has no heater, extruder, or probe sections.
- The Y− direction was confirmed toward the front with a slow relative move. X+ was commanded toward the right; physical direction confirmation is pending. See the status notes before continuing.

This is an in-progress bring-up config, not a complete print-ready printer configuration.

## Files

- config/printer.cfg — conservative endstop and motion bring-up config.
- firmware/Robin_nano.bin — Klipper MCU firmware built for the Robin Nano V1.2.
- firmware/BUILD.md — build settings, source revision, and checksum.
- docs/STATUS.md — hardware state, endstop observations, and last manual jogs.

## Important

The machine has not been homed. Coordinates used for the small jogs were set from the user's manual placement at the center and are not calibrated coordinates. After starting a fresh Klipper host, treat all axes as unhomed. Do not run G28 Y while the Y endstop is broken.
