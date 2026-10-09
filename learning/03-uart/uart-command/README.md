# UART Command Parser

Control an LED with text commands sent over a serial terminal, using interrupt-driven reception.

**Status:** Exercise

## Overview

The firmware receives one byte at a time on USART1 through an interrupt, builds a line until `\n`, then executes the command in the main loop.

## Hardware

| Pin | Function |
|---|---|
| PA9 | USART1 TX |
| PA10 | USART1 RX |
| PB6 | LED output |

Connect a USB-to-serial adapter (3.3 V logic) and open a terminal at the baud rate configured in the `.ioc`.

## Commands

End each command with a newline.

| Command | Action | Reply |
|---|---|---|
| `ON` | LED on | `LED ON` |
| `OFF` | LED off | `LED OFF` |
| `STATUS` | Read the LED pin | `STATUS: ON` or `STATUS: OFF` |
| `HELP` | List commands | `Commands: ON, OFF, STATUS, HELP` |

## How it works

- `HAL_UART_Receive_IT()` is re-armed for one byte after each reception inside `HAL_UART_RxCpltCallback()`.
- Characters accumulate in a 32-byte buffer. `\r` is ignored and `\n` marks the command as complete.
- The callback only sets a `volatile` flag. Parsing (`strcmp`) happens in the main loop, so the interrupt stays short.
- An overflow resets the buffer index.

## Skills demonstrated

- UART reception with interrupts
- Producer/consumer flag between ISR and main loop
- Simple text protocol design

## Run

Open the folder in STM32CubeIDE, build, flash. See [`docs/setup.md`](../../../docs/setup.md).
