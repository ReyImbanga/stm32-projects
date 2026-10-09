# STM32 PWM LED Controller with Interrupt-Driven Mode Switching

## Overview

This project demonstrates an interrupt-driven embedded firmware architecture using the STM32F103C8T6 microcontroller and the STM32 HAL framework.

The application generates PWM-based LED brightness control while handling user interaction through external interrupts (EXTI). Multiple operating modes are implemented using a lightweight embedded state machine architecture.

The project focuses on practical real-time embedded systems concepts including:

- PWM signal generation
- Hardware timer configuration
- Interrupt handling
- Software debouncing
- Embedded state machines
- Non-blocking firmware design
- STM32 HAL peripheral management

This project was developed as part of hands-on STM32 embedded systems training.

---

## Features

- PWM LED brightness control
- Interrupt-driven firmware architecture
- Push-button mode switching using EXTI
- Software debounce implementation
- Smooth LED fade animation
- Hardware timer scheduling
- Lightweight embedded state machine
- Non-blocking main loop design

---

## Operating Modes

| Mode | Description |
|---|---|
| OFF | LED completely OFF |
| FADE | LED brightness smoothly increases and decreases |
| FULL | LED operates at maximum brightness |

---

## Hardware Used

### Microcontroller

- STM32F103C8T6 ("Blue Pill")

### Components

- LED
- Push button
- Current limiting resistor
- ST-Link programmer/debugger

---

## Pin Configuration

| Peripheral | Pin | Function |
|---|---|---|
| TIM4 CH1 | PB6 | PWM Output |
| EXTI Button | PA0 | User Input Interrupt |

---

## Clock Configuration

The system clock is configured using:

- External HSE oscillator
- PLL enabled
- System clock frequency: 72 MHz

---

## Timer Configuration

### TIM2 — Scheduler Timer

TIM2 is configured as a periodic interrupt source used for firmware scheduling and LED animation timing.

| Parameter | Value |
|---|---|
| Prescaler | 7199 |
| Period | 99 |
| Interrupt Frequency | 100 Hz |

### TIM4 — PWM Generator

TIM4 is configured in PWM mode to generate LED brightness control signals.

| Parameter | Value |
|---|---|
| Prescaler | 71 |
| Period | 999 |
| PWM Frequency | 1 kHz |
| Resolution | 1000 steps |

---

## Software Architecture

### Main Loop

The application uses a lightweight non-blocking superloop architecture.

```c
while (1)
{
}
```

All runtime logic is handled through interrupts.

---

### Interrupt Handlers

#### Timer Interrupt Callback

```c
HAL_TIM_PeriodElapsedCallback()
```

Responsible for:

- Updating PWM duty cycle
- Managing fade animation
- Applying current operating mode

---

#### GPIO External Interrupt Callback

```c
HAL_GPIO_EXTI_Callback()
```

Responsible for:

- Detecting button presses
- Applying software debounce filtering
- Switching operating modes

---

### Embedded State Machine

```text
0 = OFF
1 = FADE
2 = FULL
```

The firmware behavior is controlled using a lightweight state machine implementation.

---

## Fade Algorithm

The LED fade effect is implemented using incremental PWM duty cycle updates combined with automatic direction reversal.

```c
duty += direction;
```

The direction changes automatically when reaching minimum or maximum duty cycle values, creating a smooth breathing LED effect.

---

## Debounce Strategy

Mechanical push-button bouncing is filtered using a software debounce mechanism based on the system tick timer.

```c
if ((now - lastPress) < 30)
    return;
```

Debounce time:
- 30 ms

---

## Project Structure

```text
STM32-PWM-LED/
│
├── Core/
│   ├── Inc/
│   └── Src/
│       └── main.c
│
├── Drivers/
│
├── README.md
│
└── STM32CubeIDE project files
```

---

## Skills Demonstrated

This project demonstrates practical experience with:

- Embedded C programming
- STM32 HAL drivers
- PWM signal generation
- Hardware timers
- External interrupts (EXTI)
- Interrupt-driven firmware architecture
- Embedded state machines
- Real-time embedded systems
- Software debouncing
- Non-blocking firmware design

---

## Development Environment

| Tool | Description |
|---|---|
| STM32CubeIDE | Firmware development |
| STM32 HAL | Peripheral abstraction framework |
| ARM GCC | Embedded compiler |
| ST-Link | Flashing and debugging |

---

## Future Improvements

Possible future enhancements include:

- UART debugging interface
- DMA-driven PWM updates
- ADC-controlled brightness adjustment
- RTOS integration (FreeRTOS)
- Multi-channel RGB LED control
- EEPROM configuration storage
- Advanced animation engine

---

## Learning Objectives

This project was created to strengthen understanding of:

- STM32 timer peripherals
- PWM signal generation
- Interrupt-based embedded systems
- Embedded firmware architecture
- Real-time event handling
- Peripheral configuration using STM32 HAL

---

## Author
Rey Imbanga
- Embedded Systems and Electronics Engineering

Focused on:

- STM32 development
- Embedded C programming
- Real-time systems
- Low-level microcontroller programming

---

## License

This project is provided for educational and portfolio purposes.
