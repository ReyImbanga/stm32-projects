# STM32 Light-Responsive LED System (ADC + DMA + Timer + PWM)

## Overview

This project implements a **real-time embedded acquisition and control system** using an STM32F103C8T6.

It reads ambient light using a **photoresistor (LDR)**, processes the signal using **DMA-based ADC acquisition**, and controls LED brightness via **PWM**.

The system is designed with a **deterministic architecture**, separating acquisition, processing, and actuation.

---

## System Architecture

```
TIM3 (Trigger) → ADC1 → DMA (Circular Buffer) → CPU Processing → PWM (TIM2) → LED
```

### Key Design Principles

* **Deterministic sampling** using timer-triggered ADC
* **Non-blocking data acquisition** via DMA
* **Signal conditioning** using averaging filter
* **Decoupled architecture** (sensor ≠ actuator)

---

## Hardware Setup

### Components

* STM32F103C8T6 (Blue Pill)
* Photoresistor (LDR)
* Fixed resistor (10kΩ)
* LED + current-limiting resistor
* Breadboard + wiring

### Analog Front-End (Critical)

```
3.3V
 |
[10kΩ]
 |
 +-------> PA0 (ADC1_IN0)
 |
[LDR]
 |
GND
```

⚠️ The ADC input is **isolated from the LED circuit** to ensure accurate measurement.

---

## Firmware Architecture

### 1. ADC Acquisition

* ADC triggered by **TIM3 TRGO**
* Sampling frequency: **~10 kHz**
* Continuous acquisition via **DMA (circular mode)**

### 2. DMA Strategy

* Buffer size: 64 samples
* Half-transfer and full-transfer callbacks used
* Processing done in main loop (not in interrupt)

### 3. Signal Processing

* Arithmetic mean applied to reduce noise:

```
avg = sum(samples) / N
```

* Acts as a simple low-pass filter

### 4. PWM Control

* TIM2 configured in PWM mode
* Duty cycle updated dynamically based on ADC average

---

## Data Flow

```
Light → LDR resistance → Voltage divider → ADC
→ DMA buffer → Averaging → PWM duty cycle → LED brightness
```

---

## 📊 Results

| Condition    | ADC Value | Voltage | LED Behavior |
| ------------ | --------- | ------- | ------------ |
| Bright light | Low       | ~0V     | LED dim/off  |
| Darkness     | High      | ~3.3V   | LED bright   |

---

## Challenges & Solutions

### Issue: Unstable ADC readings

**Cause:** No common ground / floating input
**Fix:** Proper grounding and use of STM32 3.3V reference

---

### Issue: ADC saturation (~0.6V max)

**Cause:** Transistor connected to ADC node
**Fix:** Separation of analog sensing and switching circuitry

---

### Issue: Noisy signal

**Cause:** High impedance + environmental noise
**Fix:** Averaging filter + proper resistor selection

---

## Improvements (Future Work)

* Implement **exponential moving average (EMA) filter**
* Add **hysteresis for threshold-based control**
* Use **calibration for lux estimation**
* Replace LDR with **photodiode for higher precision**

---

## Key Takeaways

* ADC accuracy depends heavily on **analog front-end design**
* DMA enables **efficient, non-blocking data acquisition**
* Timer-triggered sampling ensures **predictable timing**
* Clean architecture improves **scalability and reliability**

---

## Author: Rey Imbanga

Embedded Systems Engineer (in progress)

Focus areas:

* STM32 bare-metal & HAL
* Real-time systems
* Sensor interfacing
* Embedded architecture design
