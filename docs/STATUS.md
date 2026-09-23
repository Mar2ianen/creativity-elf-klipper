# Bring-up status — updated 2026-09-23

## Hardware and host

- Printer: Creativity Elf.
- Mainboard: MKS Robin Nano V1.2 / STM32F103VET6.
- USB serial adapter: CH340, enumerated as /dev/ttyUSB0 on the test PC.
- Klipper MCU handshake succeeded from this PC; Orange Pi 3 LTS is the planned host later.
- Print head has been changed; heater, extruder, and probe setup is still pending.

## Endstops

During the initial 15-second QUERY_ENDSTOPS poll on 2026-09-22:

- X changed between open and TRIGGERED.
- Y remained open throughout. At the time, the user reported the wire was broken and planned to repair it; that repair is now reported complete (see the 2026-09-23 addendum).
- Z and the configured z1 input changed between open and TRIGGERED.
- The z1 input is assigned to PC4 provisionally; its physical identity still needs confirmation.

## Addendum — 2026-09-23: repair and logical polarity

The user reports that all mechanical endstops are now repaired and working. In a 30-second poll at 4 queries per second (120 `QUERY_ENDSTOPS` requests), Y changed between `open` and `TRIGGERED` as it was actuated. X, Z, and Z1 remained `TRIGGERED`; the user confirmed those switches were not touched and were physically released. The user also confirmed all switches are currently released.

For X, Z, and Z1, the previous Klipper logic reported the released state backwards. The `endstop_pin` polarity was reversed in `config/printer.cfg` and the working phase-one config for X/PA15, Z/PA11, and Z1/PC4. Y toggled during actuation, so its original `!PA12` polarity was retained; the exact released/pressed mapping for Y still needs one deliberate check.

The updated X/Z/Z1 polarity had not yet been re-polled at the time of this first addendum. The Z1 input remains provisionally mapped to PC4, so verify which physical switch it represents.

### Follow-up X/Y poll and homing

In a second 30-second poll, X started `open` while released and changed to `TRIGGERED` when pressed. Y started `TRIGGERED` while released and changed to `open` when pressed, confirming Y's polarity was also reversed. The config was changed from `!PA12` to `PA12`.

The user located the X endstop at the right side and the Y endstop by the screen/front. The config now homes X+ to max=300 and Y− to min=0. After the polarity change, released X/Y both read `open`. `G28 X Y` completed: at X=300/Y=0 both read `TRIGGERED`; after a 50 mm inward move to X=250/Y=50, both returned to `open`. The current Klipper session reports XY homed; Z remains unhomed. A new host session will start unhomed.

X/Y endstop response and homing directions are verified. Z and Z1 have not been individually pressed after the polarity update; verify each with `QUERY_ENDSTOPS` before Z homing. The Z1-to-PC4 mapping remains provisional.

## Earlier direction checks and jogs

During the initial jogs, the user manually placed the head near the center and temporary software coordinates were set from that estimate. No homing was performed at that stage.

- Y−10 mm, then Y−50 mm, at 2 mm/s; the user confirmed the Y− direction moved toward the printer front.
- X+50 mm at 2 mm/s was commanded from the assumed center. Klipper reported X=200, Y=90 afterward. The later X homing confirmed X+ moves toward the right-side endstop.

These are relative direction checks, not a calibrated coordinate reference. On a fresh host connection, treat the axes as unhomed and re-establish a safe position before any further jog.
