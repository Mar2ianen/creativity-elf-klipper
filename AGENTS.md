# AGENTS.md

This repository controls a real 3D printer. Treat every configuration change as a hardware change.

## Non-negotiable rules

1. **Read the repository state before changing anything.**
   At minimum read:
   - `README.md`
   - `docs/STATUS.md`
   - `firmware/BUILD.md`
   - every config file you intend to touch

   Do not work from assumptions, chat memory, generic Creativity Elf configs, or an upstream example when the repository already contains a measured or confirmed value.

2. **The repository is the source of truth. Keep it current.**
   Any confirmed hardware fact, successful test, failed test, wiring change, pin change, axis-direction result, endstop result, firmware change, toolhead change, or calibration result that materially changes the printer state **must be documented in the same task/commit**.

   In particular, update `docs/STATUS.md` whenever the real machine state changes.

   If a change makes `README.md`, comments, or another document stale, update those too. Leaving known-stale documentation behind is a bug.

3. **Never claim hardware validation that did not happen.**
   Distinguish clearly between:
   - inferred / copied from upstream,
   - configured,
   - electrically verified,
   - mechanically verified,
   - tested on the real printer.

   A command being accepted by Klipper does not mean the mechanism was verified. A pin existing in an upstream config does not prove this particular machine is wired that way.

4. **Do not guess pins, polarity, dimensions, thermistor types, driver modes, or mechanics.**
   Prefer, in order:
   1. measurements or tests on this printer;
   2. schematics / official board documentation;
   3. upstream Klipper board config;
   4. well-supported external references.

   If a value is provisional, mark it explicitly as provisional in both config comments and `docs/STATUS.md`.

5. **Hardware safety beats convenience.**
   Before enabling motion, homing, heaters, fans, probes, or drivers, verify the relevant prerequisites.

   Examples:
   - Do not home an axis whose endstop has not been verified.
   - Do not enable a heater before its temperature sensor reports sane values and the heater output is mapped correctly.
   - Do not raise velocity/acceleration merely because the config parses.
   - Do not remove conservative bring-up limits until the corresponding subsystem has been tested.

6. **Make small, reviewable changes.**
   Do not mix unrelated pin remaps, motion tuning, heater setup, macros, and documentation cleanup into one opaque edit.

   Prefer one logical change per commit when practical.

7. **Preserve reproducibility.**
   Firmware changes must record:
   - upstream/source revision,
   - target MCU/board,
   - bootloader offset,
   - clock source,
   - communication interface,
   - any post-processing step,
   - checksum of the produced binary.

   Update `firmware/BUILD.md` when any of these change.

8. **Do not silently replace known-good configuration.**
   If an existing value was physically verified, preserve it unless there is evidence it is wrong. Explain why a verified value is being changed.

9. **Prefer modular configuration.**
   As this repository grows, split stable hardware, motion, toolhead, heaters, drivers, macros, and experimental configuration into separate included files instead of growing one monolithic `printer.cfg`.

   Keep the top-level config easy to audit.

10. **No fake cleanup.**
    Do not delete comments that encode why a value exists.
    Do not rewrite working config just to make it prettier.
    Do not turn a bring-up repository into an abstract template detached from the actual printer.

## Required workflow

For every task:

1. Read current docs and relevant config.
2. State what is known versus what still needs physical verification.
3. Make the smallest change that advances the machine.
4. Validate syntax/configuration where possible.
5. If real-hardware validation is required, stop and give the exact test to run.
6. After the user reports the result, update configuration **and** documentation to match reality.
7. Ensure there are no stale statements contradicting the new state.

## Documentation discipline

`docs/STATUS.md` must answer, at a glance:

- What hardware is currently installed?
- Which axes move correctly?
- Which axes have been homed successfully?
- Which endstops are electrically and physically verified?
- Which pins/sections are still provisional?
- Is the toolhead installed?
- Are thermistors sane?
- Have heaters been tested?
- Has extrusion been tested?
- What was the last successful real-machine operation?
- What is the next blocked or risky step?

Do not let this file become a historical diary full of obsolete warnings. Keep current state prominent; move useful history into dated notes only when it is worth preserving.

## External sources

When consulting Klipper or board references:

- Prefer upstream Klipper documentation/configs and official board documentation.
- Record important external references in repo docs when they materially justify a hardware mapping or procedure.
- Never copy a full config from another Creativity Elf and assume this printer matches it.

This machine is being modified over time and may diverge substantially from stock Creativity Elf hardware.

## Agent handoff quality

Assume another agent will continue the task with no chat context.

Before finishing, leave the repository in a state where that agent can determine:
- what is true,
- what is inferred,
- what was tested,
- what remains unsafe or unknown,
- and what should happen next,

without reconstructing the project from conversation history.

If you know the docs are stale when you finish, **the task is not finished**.
