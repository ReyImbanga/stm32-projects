# Setup Guide

How to open, build and flash the projects in this repository.

## Tools

| Tool | Purpose |
|---|---|
| STM32CubeIDE | Editor, GCC toolchain, CubeMX configurator, debugger |
| ST-Link V2 | SWD programmer and debugger |
| STM32CubeProgrammer (optional) | Standalone flashing and chip inspection. It supersedes the older ST-LINK Utility |

## Wiring the Blue Pill to an ST-Link V2 (SWD)

| ST-Link V2 | Blue Pill |
|---|---|
| SWCLK | SWCLK (PA14) |
| SWDIO | SWDIO (PA13) |
| GND | GND |
| 3.3V | 3.3V |

Notes:

- Leave `BOOT0` at 0 (jumper on the 0 side) for normal operation from Flash.
- The on-board LED is on **PC13** and is **active low** (the LED lights when the pin is at 0).
- Many Blue Pill boards carry clone chips (for example CKS32 or GD32). They usually work, but the device ID or Flash size reported may differ from a genuine STM32F103C8T6, and CubeIDE can show an ID warning.
- In every CubeMX configuration, set `System Core → SYS → Debug` to **Serial Wire**. Otherwise the debugger loses access to the chip after the first flash.

## Opening a project

1. In CubeIDE: `File → Open Projects from File System…`.
2. Click `Directory…` and select one project folder, for example `projects/adc-dma-uart-monitor`.
3. Finish. The project builds as is, because `Drivers/` is committed.

Open one project at a time per folder. Do not import the repository root.

## Building and flashing

1. Build with the hammer icon (Debug configuration).
2. Run or debug with the ST-Link connected. `Run → Debug As → STM32 C/C++ Application`.

## Renaming a project safely

Rename it from inside CubeIDE (`Refactor → Rename`) and keep the `.ioc` name consistent. Renaming files from the operating system can desynchronize the `.ioc`, `.project` and `.cproject`.

## Regenerating code

CubeMX regenerates `main.c` and peripheral files whenever the `.ioc` changes. Only code written between `USER CODE BEGIN` and `USER CODE END` markers survives.
