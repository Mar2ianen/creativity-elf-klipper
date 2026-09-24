# Addendum — 2026-09-24: hotend low-temperature test and fan output check

Klipper was run manually on the test PC through the CH340 adapter at `/dev/ttyUSB0`. The host remains this PC; Orange Pi 3 LTS is the planned permanent host. No motion was issued.

## Hotend

The changed toolhead thermistor uses the provisional upstream `ATC Semitec 104GT-2` curve on PC1. For a low-temperature check, PC3 was configured as `[heater_generic hotend]` with `watermark` control, `max_power: 0.5`, and `max_temp: 50`. The bed remained at target 0°C.

A 40°C target was set. Klipper readings rose from about 22°C to 41.9°C in roughly 20 seconds; heater power then turned off. Thermal inertia carried the Klipper reading to about 47.7°C before cooling. The user reports a multimeter thermocouple read 44°C at its placement point and said the check was okay. The target was set to 0°C; the heater output remained at 0, and Klipper stayed `Ready`. The user asked not to run hotend PID calibration, and none was run. The hotend sensor curve remains provisional; no extrusion test was performed.

## Part-cooling blower and heatsink fan

Makerbase's Robin Nano V1.2 pinout and upstream Klipper config identify the dedicated `FAN` output as PB1. The config exposes that pin neutrally as `[fan_generic fan_pb1]` with `shutdown_speed: 0`.

With both heater targets and powers at 0, PB1 was commanded to 40% for about five seconds. Klipper accepted the command and reported `speed: 0.4`; PB1 was then returned to 0 and reported `speed: 0.0`. The user observed that the part-cooling blower at the nozzle did not spin. The fan that is running is the separate heatsink/radiator fan. The physical fan or harness connected to PB1 is therefore not confirmed by this command test.

The board also exposes `HE1` at PB0. Upstream Klipper maps this to an additional heater output, not a dedicated fan connector. It may be usable as a second PWM channel only after confirming that no heater is connected there and tracing the toolhead wiring. PB0 was not energized during this check. No independent second fan channel has been physically verified.

References: [Makerbase Robin Nano V1.2 pinout](https://github.com/makerbase-mks/MKS-Robin-Nano-V1.X/blob/master/hardware/MKS%20Robin%20Nano%20V1.2_003/MKS%20Robin%20Nano%20V1.2_003%20PIN.pdf), [Makerbase Robin Nano V1.2 schematic](https://github.com/makerbase-mks/MKS-Robin-Nano-V1.X/blob/master/hardware/MKS%20Robin%20Nano%20V1.2_004/MKS%20Robin%20Nano%20V1.2_004%20SCH.pdf), [Klipper generic Robin Nano config](https://github.com/Klipper3d/klipper/blob/master/config/generic-mks-robin-nano-v1.cfg), [Klipper fan_generic reference](https://www.klipper3d.org/Config_Reference.html#fan_generic).
