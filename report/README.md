# CAN-Driven Digital Instrument Cluster on STM32F469

**Sana Gholipour Bashiri**

An embedded digital instrument-cluster interface for STM32F469I-DISCO, receiving CAN speed and clock messages and presenting them through TouchGFX.

## Features

- CAN2 at 125 kbit/s; PB13/TX and PB12/RX.
- Identifier-list filtering and an RTOS polling receive task.
- 480 x 800 pixel TouchGFX layout with a 0-200 speed gauge and 24-hour clock.
- STM32CubeIDE, MDK-ARM, EWARM, and GCC project definitions.

## Build and reproduce

1. Read [build dependencies](../simulation/DEPENDENCIES.md).
2. Restore the matching ST SDK: saved versions are STM32CubeMX 6.15.0, STM32Cube FW_F4 V1.28.2, and TouchGFX 4.25.0.
3. Open `simulation/TouchGFX/can_touch.touchgfx`, generate code, and import `simulation/STM32CubeIDE/.project` or use a supplied alternate IDE project.
4. Preserve user-code sections when regenerating from `simulation/STM32F469I-DISCO.ioc`.
5. Build with the matching toolchain, connect a suitable external CAN transceiver, then flash and validate using a known CAN source at 125 kbit/s.

The platform TouchGFX SDK is intentionally obtained from ST rather than republished here. Third-party source notices remain in place; see [notices](../THIRD_PARTY_NOTICES.md). A desktop simulator needs a host data provider for the firmware globals.

## Results and implementation boundaries

Read the [report](../report/STM32_CAN_Instrument_Cluster.pdf). [Calculated decoding vectors](../results/decoding_vectors.csv) and the [architecture figure](../results/figures/firmware_architecture.png) are derived from source, not measured board performance. No hardware result recording or as-built transceiver circuit was supplied.

The current source does not mask the upper unused speed/hour bits, truncates fractional speed, and compares raw/scaled gauge backup values inconsistently. It shares globals across tasks without a coherent snapshot, and its error handler is empty. Fuel ID 0x220 passes the filter but is not decoded/displayed. This is the supplied TouchGFX implementation; no LVGL source is included. These limits are analyzed in the report, and no untested functional repairs are concealed in this upload package.


## Repository layout

- `report/`: six-page academic report, editable report text, and this README.
- `simulation/`: native project/model sources and environment requirements.
- `results/`: project-specific evidence and clearly labeled analytical results.
- `MANIFEST.csv`: file sizes and SHA-256 checksums for the upload package.

Upload the **contents of this GitHub folder** as the root of one repository. This project is independent and does not use either of the other project folders. The README at the repository root is mirrored inside `report/` as requested. Relative links in each copy are adjusted for its location.
