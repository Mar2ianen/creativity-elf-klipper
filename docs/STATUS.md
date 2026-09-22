# Bring-up status — 2026-09-22

## Hardware and host

- Printer: Creativity Elf.
- Mainboard: MKS Robin Nano V1.2 / STM32F103VET6.
- USB serial adapter: CH340, enumerated as /dev/ttyUSB0 on the test PC.
- Klipper MCU handshake succeeded from this PC; Orange Pi 3 LTS is the planned host later.
- Print head has been changed; heater, extruder, and probe setup is still pending.

## Endstops

During a 15-second QUERY_ENDSTOPS poll:

- X changed between open and TRIGGERED.
- Y remained open throughout. The user confirmed the Y endstop wire is broken and plans to repair it.
- Z and the configured z1 input changed between open and TRIGGERED.
- The z1 input is assigned to PC4 provisionally; its physical identity still needs confirmation.

Do not home Y until its wire is repaired and the switch is verified. Recheck all endstop states after the repair before homing.

## Direction checks and jogs

The user manually placed the head near the center before testing. Temporary software coordinates were set from that assumed center; no homing was performed.

- Y−10 mm, then Y−50 mm, at 2 mm/s; the user confirmed the Y− direction moved toward the printer front.
- X+50 mm at 2 mm/s was commanded from the assumed center. Klipper reported X=200, Y=90 afterward; physical X direction confirmation is pending.

These are relative direction checks, not a calibrated coordinate reference. On a fresh host connection, treat the axes as unhomed and re-establish a safe position before any further jog.
