# Addendum — 2026-09-23: bed heater test and PID calibration

The Robin Nano V1.2 bed thermistor is connected to `PC0` (`TB`) and the bed heater output to `PA0` (`H-BED`), following the Makerbase pinout/schematic. The configured thermistor curve is `EPCOS 100K B57560G104F`, copied from upstream Klipper's generic Robin Nano V1.2 configuration; it has not been independently checked against the printer's factory firmware.

The user reported feeling the bed warm normally. With Klipper watermark control active, the printer was heated progressively from an ambient reading of about 19.6°C:

| Target | Highest observed temperature | Heater response |
| --- | ---: | --- |
| 30°C | 34.8°C | Heater switched; bed warmed |
| 50°C | 52.8°C | Output switched off near target |
| 65°C | 67.0°C | Output switched off near target |
| 80°C | 82.5°C | Output switched off near target |

Klipper stayed `Ready` during the staged test. PID calibration was then run with `PID_CALIBRATE HEATER=heater_bed TARGET=80`; it cycled between 75°C and 80°C and completed with:

```ini
control: pid
pid_Kp: 64.814
pid_Ki: 1.583
pid_Kd: 663.529
```

`SAVE_CONFIG` persisted these values and restarted Klipper. Post-restart, Klipper was `Ready`, heater target was 0°C, power was 0, and the bed was cooling from 74.01°C. All axes are unhomed after the restart; no motion was issued during heater setup or calibration. The bed `max_temp` remains 100°C, and the hotend, extruder, and probe remain unconfigured.

References: [Makerbase Robin Nano V1.2 pinout](https://github.com/makerbase-mks/MKS-Robin-Nano-V1.X/blob/master/hardware/MKS%20Robin%20Nano%20V1.2_003/MKS%20Robin%20Nano%20V1.2_003%20PIN.pdf), [Makerbase Robin Nano V1.2 schematic](https://github.com/makerbase-mks/MKS-Robin-Nano-V1.X/blob/master/hardware/MKS%20Robin%20Nano%20V1.2_004/MKS%20Robin%20Nano%20V1.2_004%20SCH.pdf), [Klipper generic Robin Nano config](https://github.com/Klipper3d/klipper/blob/master/config/generic-mks-robin-nano-v1.cfg), [Klipper PID calibration procedure](https://www.klipper3d.org/Config_checks.html#calibrate-pid-settings).
