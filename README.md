# STM32 Projects — Embedded Firmware Portfolio

Hands-on firmware for the **STM32F103C8T6 ("Blue Pill", ARM Cortex-M3)**, built with **STM32CubeIDE** and the **STM32 HAL**. The repository follows a structured learning path from GPIO and timers up to ADC + DMA acquisition chains, I²C displays and FreeRTOS.

> **Reading guide.** `projects/` holds complete, documented builds. `learning/` holds the guided exercises that led to them, in the order they were practiced.

## Hardware and toolchain

| Item | Details |
|---|---|
| MCU board | STM32F103C8T6 "Blue Pill" (72 MHz max, Cortex-M3) |
| Programmer / debugger | ST-Link V2 over SWD |
| IDE | STM32CubeIDE (GCC `arm-none-eabi`, CubeMX integrated) |
| Libraries | STM32Cube HAL, CMSIS, FreeRTOS (CMSIS-RTOS v2) |
| Peripherals used | Potentiometers, LDR, LEDs, push buttons, SSD1306 OLED (I²C), NPN driver stage |

## Repository layout

```
stm32-projects/
├── projects/      Complete builds, each with its own README
├── learning/      Guided exercises, ordered by topic
│   ├── 00-course/       Study plan, lessons and journal (French)
│   ├── 01-gpio/  02-timers/  03-uart/  04-interrupts/  05-i2c/  06-registers/
├── docs/          Setup guide, conventions, README template
├── LICENSE
└── THIRD_PARTY_NOTICES.md
```

## Featured projects

| Project | What it demonstrates | Status |
|---|---|---|
| [`adc-dma-uart-monitor`](projects/adc-dma-uart-monitor) | Timer-triggered ADC sampling, circular DMA, half/full-buffer processing, moving-average filter, UART telemetry | Documented |
| [`ldr-pwm-light-controller`](projects/ldr-pwm-light-controller) | Light sensing with an LDR, DMA acquisition, filtering, PWM LED brightness control | Documented |
| [`adc-oled-pwm-controller`](projects/adc-oled-pwm-controller) | ADC + DMA, PWM output, SSD1306 OLED over I²C, transistor driver stage, KiCad schematic | Documented |
| [`pwm-led-mode-controller`](projects/pwm-led-mode-controller) | EXTI-driven mode switching, software debounce, embedded state machine, PWM fade | Documented |
| [`freertos-microweather-altimeter`](projects/freertos-microweather-altimeter) | FreeRTOS (CMSIS-RTOS v2) project foundation, sensor features planned | In progress |

## Learning path

| Topic | Exercise | Status |
|---|---|---|
| GPIO | [`blink-button-debounce`](learning/01-gpio/blink-button-debounce) | Exercise |
| GPIO | [`gpio-practice-debounce`](learning/01-gpio/gpio-practice-debounce) | Exercise (contains experiments) |
| Timers | [`timer-interrupts-pwm`](learning/02-timers/timer-interrupts-pwm) | Exercise |
| UART | [`uart-command`](learning/03-uart/uart-command) | Exercise |
| Interrupts | [`exti-basics`](learning/04-interrupts/exti-basics) | In progress |
| I²C | [`oled-ssd1306`](learning/05-i2c/oled-ssd1306) | Exercise |
| Registers | [`baremetal-gpio`](learning/06-registers/baremetal-gpio) | Skeleton |

The study plan follows ST's free MOOC *Moving from 8 to 32 bits workshop – first steps in STM32*, adapted from the Nucleo-F072RB/Keil setup to the Blue Pill/STM32CubeIDE, then extended with deeper modules. See [`learning/00-course`](learning/00-course).

## Getting started

1. Install **STM32CubeIDE**.
2. In the IDE: `File → Open Projects from File System…`, choose a project folder (for example `projects/adc-dma-uart-monitor`).
3. Build, then flash with an ST-Link V2 wired over SWD. Wiring and notes are in [`docs/setup.md`](docs/setup.md).

Each project is self-contained: the `Drivers/` folder is committed, so a fresh clone builds without regenerating code.

## Conventions

Folder naming, commit style and the README template are described in [`docs/conventions.md`](docs/conventions.md).

## License

Original code in this repository is released under the [MIT License](LICENSE).

Vendor and third-party code keeps its own license and is not covered by the MIT grant. In particular, the SSD1306 driver used by two projects is **GPL-3.0**. See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for the full list.

## Author

Rey Imbanga — electronics and embedded systems engineer. GitHub: [@ReyImbanga](https://github.com/ReyImbanga)
