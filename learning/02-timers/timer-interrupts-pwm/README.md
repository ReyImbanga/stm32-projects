# Timers: Periodic Interrupts and PWM Fade

Three timers in one exercise: two periodic interrupts toggling LEDs, and one PWM channel producing a fade.

**Status:** Exercise

## Overview

| Timer | Mode | Configuration | Effect |
|---|---|---|---|
| TIM3 | Update interrupt | prescaler 7200-1, period 3000 | Toggles `PA2` in the callback |
| TIM4 | Update interrupt | prescaler 7200-1, period 7000 | Toggles `PA3` in the callback |
| TIM2 CH1 | PWM | period 255 | Fade up and down on `PA0` |

With a 72 MHz timer clock, the prescaler gives a 10 kHz tick, so TIM3 fires every 300 ms and TIM4 every 700 ms. Check the clock tree in the `.ioc` to confirm the timer clock used.

## How it works

- `HAL_TIM_Base_Start_IT()` starts TIM3 and TIM4. Both share `HAL_TIM_PeriodElapsedCallback()`, which checks `htim->Instance` to know who fired.
- The main loop ramps the PWM compare value from 0 to 255 and back, one step every 10 ms.
- The two LEDs blink at unrelated periods, which shows that timers run independently of the CPU loop.

## Skills demonstrated

- Timer base configuration (prescaler and period)
- Interrupt callbacks shared across several timers
- PWM generation with `__HAL_TIM_SET_COMPARE`

## Run

Open the folder in STM32CubeIDE, build, flash. See [`docs/setup.md`](../../../docs/setup.md).
