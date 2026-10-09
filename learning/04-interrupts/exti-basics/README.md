# EXTI Basics

Starting point for external interrupts on a push button.

**Status:** In progress

## Current state

- The generated project configures `PB6` as an LED output. The main loop toggles it every second with `HAL_Delay(1000)`.
- A `HAL_GPIO_EXTI_Callback()` with a 25 ms debounce on `GPIO_PIN_0` is written in the source but **commented out**.
- The `.ioc` does not yet configure an EXTI input pin, so no interrupt is active.

## Next steps

1. Configure an input pin in CubeMX as `GPIO_EXTI` (falling edge, pull-up) and enable its NVIC line.
2. Uncomment and adapt the callback to the chosen pin.
3. Remove the blocking `HAL_Delay` from the main loop.

## Complete version

A finished EXTI implementation (interrupt-driven mode switching, debounce, state machine) is in [`projects/pwm-led-mode-controller`](../../../projects/pwm-led-mode-controller).

## Run

Open the folder in STM32CubeIDE, build, flash. See [`docs/setup.md`](../../../docs/setup.md).
