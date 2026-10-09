# FreeRTOS Micro Weather Station / Altimeter

FreeRTOS foundation for a small weather and altitude node on the STM32F103C8T6.

**Status:** In progress

## Current state

- FreeRTOS kernel V10.3.1 through the CMSIS-RTOS v2 wrapper.
- One default task that toggles a status LED (`LED_STATUS`) every 500 ms with `osDelay`.
- The HAL time base is moved to a hardware timer (`stm32f1xx_hal_timebase_tim.c`), which is the recommended setup when FreeRTOS owns SysTick.
- Enabled peripherals in the `.ioc`: GPIO, RCC, SYS, NVIC, FreeRTOS.

Sensor acquisition, altitude computation and display are **not implemented yet**. The project name states the intended goal.

## Proposed roadmap

- [ ] Add an I²C pressure and temperature sensor driver.
- [ ] Add a sensor task that publishes readings through a queue.
- [ ] Compute altitude from pressure with the barometric formula.
- [ ] Add a display or UART reporting task.
- [ ] Protect shared peripherals (I²C bus) with a mutex.
- [ ] Document the task design: priorities, stack sizes, timing.

## Skills demonstrated so far

- FreeRTOS integration with STM32CubeMX
- Timer-based HAL tick alongside the RTOS scheduler

## Run

Open the folder in STM32CubeIDE, build, flash. See [`docs/setup.md`](../../docs/setup.md).
