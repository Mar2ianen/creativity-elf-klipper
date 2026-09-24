# Addendum — 2026-09-24: read-only hotend sensor and fan startup

The printer connected through the CH340 USB serial adapter as `/dev/ttyUSB0`. Klipper was started manually on this PC using the phase-one configuration; no Klipper host service was installed or enabled. No heater target was set during this work.

## Hotend sensor

The changed print head's hotend thermistor is configured as a read-only `[temperature_sensor hotend]` on `PC1`. The pin and provisional `ATC Semitec 104GT-2` curve follow Klipper's generic MKS Robin Nano V1.2 example; the thermistor in the changed head has not been identified independently.

In a 30-second poll at room temperature, the hotend reported 18.39–22.16°C while the bed sensor reported 19.11–20.22°C. Klipper remained `Ready`. The variation on the hotend input is larger than expected for an idle sensor; check the thermistor connector, wiring, and actual sensor type before configuring or enabling the hotend heater.

The hotend heater output was not added to the config. The object is a monitor only and cannot set a hotend target.

## Heatsink fan

The Robin Nano `FAN` output is mapped to `PB1` in Klipper's generic board config. It is exposed as `[fan_generic heatsink_fan]` with `shutdown_speed: 0`. Klipper reported `speed: 0.0` after startup and no command was sent to run the fan. Its physical wiring and spin have not been verified.

The fan is manually controlled and has no heater coupling yet. It stays off after startup; automatic hotend-temperature behavior should be added together with a verified hotend heater configuration.

At the latest poll, the bed was 20.79°C with target 0°C and power 0; the hotend sensor read 22.47°C and the fan speed was 0.0. All axes remain unhomed after starting a fresh Klipper host session.

References: [Klipper generic MKS Robin Nano V1.2 config](https://github.com/Klipper3d/klipper/blob/master/config/generic-mks-robin-nano-v1.cfg), [Klipper temperature_sensor reference](https://www.klipper3d.org/Config_Reference.html#temperature_sensor), [Klipper fan_generic reference](https://www.klipper3d.org/Config_Reference.html#fan_generic).
