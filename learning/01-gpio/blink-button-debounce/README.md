# Blink with Button Debounce

LED blink whose speed is selected by a push button, using non-blocking timing and a software debounce.

**Status:** Exercise

## Overview

The LED on `PB9` toggles continuously. While the button on `PB6` is held, the blink period drops from 500 ms to 100 ms. The button is read through a debounce filter, and all timing is non-blocking.

## Hardware

| Pin | Function |
|---|---|
| PB6 | Push button input, active low (pressed = 0) |
| PB9 | LED output |

## How it works

- `BTN_ReadStable()` samples the raw pin and only accepts a new state after it stays unchanged for `DEBOUNCE_MS` (25 ms), measured with `HAL_GetTick()`.
- The main loop sets `period_ms` to 100 ms when the stable state is pressed, otherwise 500 ms.
- The LED toggles when `HAL_GetTick() - lastToggle >= period_ms`. No `HAL_Delay`, so the button stays responsive.

## Skills demonstrated

- GPIO input and output with HAL
- Software debounce
- Non-blocking timing with the SysTick counter

## Run

Open the folder in STM32CubeIDE (`File → Open Projects from File System…`), build, flash. See [`docs/setup.md`](../../../docs/setup.md).
