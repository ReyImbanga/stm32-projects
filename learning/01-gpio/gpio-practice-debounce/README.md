# GPIO Practice: Debounce

Practice exercise on button debouncing and timed LED toggling.

**Status:** Exercise (contains commented-out experiments)

## Overview

A debounce routine (`StableBTN()`, 20 ms window) reads a button on `PB15`, and a variable `periode` (default 1000 ms) sets the LED toggle interval on `PB9`. The source keeps several earlier variants as commented-out blocks, so it reads as a lab notebook rather than a finished program.

## Hardware

| Pin | Function |
|---|---|
| PB15 | Push button input, active low |
| PB9 | LED output |

## Skills demonstrated

- Debounce with a timestamp and a stable-state variable
- Timed toggling with `HAL_GetTick()`

## Notes

- The main loop contains a blocking wait while the button is held. The finished version of this idea, with non-blocking behavior, is in [`blink-button-debounce`](../blink-button-debounce).
- Candidate cleanup: remove the commented-out blocks once the experiments are no longer needed.

## Run

Open the folder in STM32CubeIDE, build, flash. See [`docs/setup.md`](../../../docs/setup.md).
