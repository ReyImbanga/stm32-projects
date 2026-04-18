# 📡 STM32 Real-Time ADC Acquisition (DMA + Timer)

## Overview

This project implements a real-time analog signal acquisition system using the STM32F103 (Blue Pill).

The system uses:
- Timer-triggered ADC sampling (deterministic timing)
- DMA in circular mode (CPU-free data transfer)
- Double-buffer processing (half/full callbacks)
- Moving average filtering
- UART output for real-time monitoring

A potentiometer is used as an analog input signal.

---

## Architecture
TIM3 (100 Hz) → ADC → DMA(Circular Mode) → Buffer(Half Buffer Ready and Full Buffer Ready) → Processing(Averaging) → UART

---

## Hardware Setup

- STM32F103C8T6 (Blue Pill)
- ST-Link V2
- USB-UART (CH340)
- 10kΩ Potentiometer
- LED (Blue) + resistor(330Ω)

### ADC Input
- PA0 → Potentiometer middle pin
- Other pins → 3.3V / GND

### UART
- PA9 (TX) → RX (USB-UART)
- PA10 (RX) → TX (USB-UART)
- GND → GND

### LED
- PB6 → LED → GND

---

## Features

- 12-bit ADC resolution (0–4095)
- Timer-based sampling (deterministic)
- DMA circular buffering
- Double-buffer processing (half/full interrupts)
- Moving average filtering
- Real-time UART output
- Threshold-based LED control

---

## Output
V: 0.78
V: 1.25
V: 2.03

---

## Voltage Conversion
voltage = (adc_value * 3.3f) / 4095.0f;

---

## Limitations

- UART transmission is blocking (HAL_UART_Transmit)
- No synchronization protection between DMA write and CPU read
- No ADC calibration (Vref assumed 3.3V)
- No RTOS or task scheduling

---

## Improvements

- Non-blocking UART using DMA
- Double-buffer protection with mutex/RTOS
- ADC calibration using internal Vref
- Higher sampling frequency support
- Real-time visualization (Python / GUI)
