# Bare-Metal GPIO (Skeleton)

Workspace for register-level GPIO exercises.

**Status:** Skeleton

## Current state

This is a CubeMX-generated project (`baremetal_gpio.ioc`) with only the RCC and NVIC blocks enabled. The application section of `main.c` is empty and no pin is configured.

## Purpose

Use it to practice driving the GPIO peripheral **without HAL**: enable the port clock in `RCC->APB2ENR`, configure the pin mode in `GPIOx->CRL/CRH`, and write `ODR`/`BSRR`. The goal is to understand what HAL does underneath.

## Planned exercises

1. Blink the on-board LED on PC13 using direct register access.
2. Repeat with the LL (low-layer) API and compare code size.
3. Read a button through `IDR` and combine it with the LED.

## Run

Open the folder in STM32CubeIDE, build, flash. See [`docs/setup.md`](../../../docs/setup.md).
