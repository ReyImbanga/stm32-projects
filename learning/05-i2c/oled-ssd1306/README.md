# I²C: SSD1306 OLED Display

Text and a live counter on an SSD1306 OLED over I²C.

**Status:** Exercise

## Overview

The firmware initializes an SSD1306 display on I²C1 and renders text with two font sizes. The main loop displays a numeric counter, repositioning the cursor according to the number of digits so the value stays centered. Earlier demo code (text scrolling, a "SCORE" screen) remains in the source as commented-out blocks.

## Hardware

| Pin | Function |
|---|---|
| PB6 | I2C1 SCL |
| PB7 | I2C1 SDA |

The driver uses the 8-bit address `0x78`. Some modules answer at `0x7A`; change `SSD1306_I2C_ADDR` in `ssd1306.h` if the display stays blank.

## Skills demonstrated

- I²C master configuration with HAL
- Integrating a third-party display driver
- Cursor positioning for variable-width numeric output

## License note

`ssd1306.c`, `ssd1306.h` and `fonts.*` come from a library by Tilen Majerle and Alexander Lutsai and carry a **GPL-3.0** header. See [`THIRD_PARTY_NOTICES.md`](../../../THIRD_PARTY_NOTICES.md).

## Run

Open the folder in STM32CubeIDE, build, flash. See [`docs/setup.md`](../../../docs/setup.md).
