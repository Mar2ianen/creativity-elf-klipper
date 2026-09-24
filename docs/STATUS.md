# Bring-up status — updated 2026-09-24

## Hardware and host

- Printer: Creativity Elf.
- Mainboard: MKS Robin Nano V1.2 / STM32F103VET6.
- USB serial adapter: CH340, enumerated as /dev/ttyUSB0 on the test PC.
- Klipper host was manually started on this PC through the CH340 adapter for the sensor-only session; Orange Pi 3 LTS remains the planned permanent host.
- Print head has been changed. The hotend sensor is read-only; hotend heater, extruder, and probe are not configured.
- Bed thermistor PC0/TB and heater output PA0/H-BED are configured and were exercised on the printer. Staged heating to 80°C and PID calibration at 80°C succeeded. Coefficients: `Kp=64.814`, `Ki=1.583`, `Kd=663.529`. The sensor type follows the upstream Robin Nano config but is not independently cross-checked against stock firmware.
- The hotend sensor is configured read-only at PC1, with a provisional `ATC Semitec 104GT-2` curve from upstream Klipper. A 30-second poll ranged 18.39–22.16°C while the bed sensor ranged 19.11–20.22°C; the hotend sensor connection/type needs checking before heat is enabled.
- The manual heatsink fan is configured on PB1 as `fan_generic heatsink_fan`; Klipper reports speed 0 after startup. It is not automatically tied to a heater, and its electrical connection/spin has not been tested.
- Latest live status: bed 20.79°C, target 0°C, power 0; hotend sensor 22.47°C; fan speed 0. Klipper is `Ready`. All axes are unhomed after starting a fresh host session.

## Endstops

During the initial 15-second QUERY_ENDSTOPS poll on 2026-09-22:

- X changed between open and TRIGGERED.
- Y remained open throughout. At the time, the user reported the wire was broken and planned to repair it; that repair is now reported complete (see the 2026-09-23 addendum).
- Z and the configured z1 input changed between open and TRIGGERED.
- The z1 input was assigned to PC4 provisionally; its physical identity was confirmed in the 2026-09-23 Z-switch poll below.

## Addendum — 2026-09-23: repair and logical polarity

The user reports that all mechanical endstops are now repaired and working. In a 30-second poll at 4 queries per second (120 `QUERY_ENDSTOPS` requests), Y changed between `open` and `TRIGGERED` as it was actuated. X, Z, and Z1 remained `TRIGGERED`; the user confirmed those switches were not touched and were physically released. The user also confirmed all switches are currently released.

For X, Z, and Z1, the previous Klipper logic reported the released state backwards. The `endstop_pin` polarity was reversed in `config/printer.cfg` and the working phase-one config for X/PA15, Z/PA11, and Z1/PC4. Y toggled during actuation, so its original `!PA12` polarity was retained; the exact released/pressed mapping for Y still needs one deliberate check.

The updated X/Z/Z1 polarity had not yet been re-polled at the time of this first addendum. The Z1 input remains provisionally mapped to PC4, so verify which physical switch it represents.

### Follow-up X/Y poll and homing

In a second 30-second poll, X started `open` while released and changed to `TRIGGERED` when pressed. Y started `TRIGGERED` while released and changed to `open` when pressed, confirming Y's polarity was also reversed. The config was changed from `!PA12` to `PA12`.

The user located the X endstop at the right side and the Y endstop by the screen/front. The config now homes X+ to max=300 and Y− to min=0. After the polarity change, released X/Y both read `open`. `G28 X Y` completed: at X=300/Y=0 both read `TRIGGERED`; after a 50 mm inward move to X=250/Y=50, both returned to `open`. At that stage, XY were homed and Z was not yet homed; later restarts reset the homing state.

X/Y endstop response and homing directions are verified. At this point, Z and Z1 had not yet been pressed after the polarity update; the later Z-switch poll below resolved their input mapping.

### Right/left Z switch poll

On 2026-09-23, a 45-second poll ran at 4 `QUERY_ENDSTOPS` requests per second. All four inputs began `open`. Touching the physical right Z switch changed `stepper_z`/PA11 to `TRIGGERED`; releasing it returned to `open`. Touching the physical left Z switch changed `z1`/PC4 to `TRIGGERED`; releasing it returned to `open`. A final query showed all switches `open`.

The physical right/left Z endstop inputs and polarity are verified. At this point, no Z motor movement or Z homing had yet been performed.

### Z direction and homing

The first low-speed `G28 Z` attempt moved the whole portal in the wrong direction. The user reported this immediately; an emergency stop was sent and Klipper shut down. Both Z direction pins were then inverted (`PB4` for `stepper_z`, `PA1` for `stepper_z1`) and Klipper was restarted. All endstops read `open` before retrying.

The second `G28 Z` completed at 1 mm/s. A follow-up `QUERY_ENDSTOPS` showed `stepper_z:TRIGGERED` and `z1:TRIGGERED`, while X/Y remained `open`. This verifies Z homing to both physical switches with the updated directions. Independent motor-only movement and gantry alignment remain unchecked. A later Klipper restart for the bed sensor reset all axes to unhomed.

## Bed heater test and PID calibration — 2026-09-23

Makerbase's Robin Nano V1.2 pinout maps bed thermistor input `TB` to MCU pin `PC0` and bed heater output `H-BED` to `PA0`. Upstream Klipper's generic Robin Nano V1.2 config uses `sensor_type: EPCOS 100K B57560G104F` on `PC0`. The stock thermistor type is not independently confirmed against Creativity's factory firmware.

The configured `[heater_bed]` was tested on the printer using watermark control for staged bring-up, then Klipper's `PID_CALIBRATE HEATER=heater_bed TARGET=80`:

- Initial bed reading was about 19.6°C. At a 30°C target, the heater switched and the bed warmed; watermark hysteresis and thermal inertia produced a peak of about 34.8°C.
- At a 50°C target, the bed reached 52.8°C and the output switched off.
- At a 65°C target, the bed reached 67.0°C and the output switched off.
- At an 80°C target, the bed reached 82.5°C and the output switched off. Klipper remained `Ready` throughout.
- PID auto-tuning cycled the bed between 75°C and 80°C and completed with `Kp=64.814`, `Ki=1.583`, `Kd=663.529`.
- `SAVE_CONFIG` persisted the coefficients and restarted Klipper. The post-restart status was `Ready`, target 0°C, power 0, and the bed was cooling at 74.01°C.

The heater output and temperature response were verified on this printer; the sensor curve remains an upstream-based provisional assumption. `max_temp` remains conservatively limited to 100°C. Axes are unhomed after the save/restart, and no motion was issued during this heater test.

References: [Makerbase Robin Nano V1.2 pinout](https://github.com/makerbase-mks/MKS-Robin-Nano-V1.X/blob/master/hardware/MKS%20Robin%20Nano%20V1.2_003/MKS%20Robin%20Nano%20V1.2_003%20PIN.pdf), [Makerbase Robin Nano V1.2 schematic](https://github.com/makerbase-mks/MKS-Robin-Nano-V1.X/blob/master/hardware/MKS%20Robin%20Nano%20V1.2_004/MKS%20Robin%20Nano%20V1.2_004%20SCH.pdf), [Klipper generic Robin Nano config](https://github.com/Klipper3d/klipper/blob/master/config/generic-mks-robin-nano-v1.cfg), [Klipper heater bed reference](https://www.klipper3d.org/Config_Reference.html#heater_bed), [Klipper PID calibration](https://www.klipper3d.org/Config_checks.html#calibrate-pid-settings).

## Initial sensor-only check — historical

The user confirmed the printer and Robin Nano board are stock. Makerbase's Robin Nano V1.2 pinout maps the bed thermistor input `TB` to MCU pin `PC0` and bed heater output `H-BED` to `PA0`. Upstream Klipper's generic Robin Nano V1.2 config uses `sensor_type: EPCOS 100K B57560G104F` on `PC0`, with heater output `PA0`. The bring-up config uses only the sensor side in `[temperature_sensor bed]`; it has no `[heater_bed]` section or heater output.

When the sensor-only config was first enabled, Klipper reported 18.39°C, 18.49°C, and later 19.54°C. No heater was configured during that earlier check. The stock thermistor type was not independently checked against Creativity's factory firmware. No motion was issued during that initial temperature-only check.

## Earlier direction checks and jogs

During the initial jogs, the user manually placed the head near the center and temporary software coordinates were set from that estimate. No homing was performed at that stage.

- Y−10 mm, then Y−50 mm, at 2 mm/s; the user confirmed the Y− direction moved toward the printer front.
- X+50 mm at 2 mm/s was commanded from the assumed center. Klipper reported X=200, Y=90 afterward. The later X homing confirmed X+ moves toward the right-side endstop.

These are relative direction checks, not a calibrated coordinate reference. On a fresh host connection, treat the axes as unhomed and re-establish a safe position before any further jog.
