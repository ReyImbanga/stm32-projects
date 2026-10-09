# Third-Party Notices

The MIT license in this repository covers the original work only. The components below are included under their own terms.

| Component | Where it appears | Owner | License |
|---|---|---|---|
| STM32F1xx HAL driver | `Drivers/STM32F1xx_HAL_Driver/` in every project | STMicroelectronics | See the `LICENSE.txt` shipped in that folder |
| CMSIS | `Drivers/CMSIS/` in every project | Arm / STMicroelectronics | See the `LICENSE.txt` shipped in that folder |
| STM32CubeMX-generated files (`main.c`, `*_it.c`, `*_msp.c`, startup file, linker script) | Every project | STMicroelectronics | Terms stated in each file header |
| FreeRTOS kernel V10.3.1 | `projects/freertos-microweather-altimeter/Middlewares/Third_Party/FreeRTOS/` | Amazon Web Services | See `Source/LICENSE` in that folder |
| SSD1306 driver and fonts (`ssd1306.*`, `fonts.*`) | `learning/05-i2c/oled-ssd1306/` and `projects/adc-oled-pwm-controller/` | Tilen Majerle, Alexander Lutsai | **GPL-3.0** (stated in the file headers) |

## About the GPL-3.0 driver

The SSD1306 driver files carry a GNU General Public License v3 header. Two projects compile these files into their firmware:

- `learning/05-i2c/oled-ssd1306`
- `projects/adc-oled-pwm-controller`

Distributing firmware that combines GPL-3.0 code with other code is generally subject to the GPL's terms for the combined work. The MIT grant in the root `LICENSE` therefore should not be read as relicensing those two projects as a whole.

Two ways to resolve it, both left to the repository owner:

1. Keep those two projects under GPL-3.0 and state it in their READMEs.
2. Replace the driver with a permissively licensed SSD1306 library, or write one.

This notice is information, not legal advice.
