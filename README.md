# Intern-Task-
# STM32F407G ADC with DMA Triggered by TIM2 (Bare Metal)

A **professional bare-metal embedded project** demonstrating **ADC + DMA + Timer hardware triggering** on the **STM32F407G Discovery board**, with real-time ADC values sent over **USART2**.

🚫 No HAL
🚫 No CMSIS drivers
✅ 100% Register-level programming

---

## 📌 Project Overview

This project configures:

* **TIM2** to generate periodic trigger events (TRGO)
* **ADC1** to start conversion on TIM2 trigger
* **DMA2 Stream0** to move ADC data to memory automatically
* **USART2** to print ADC values to a serial terminal

The CPU never polls the ADC — everything is **hardware-driven**.

---

## 🎯 Key Learning Outcomes

* Understanding STM32 memory-mapped registers
* Hardware trigger based ADC sampling
* DMA circular mode operation
* Peripheral clock tree usage
* Register-level UART communication
* Embedded timing and synchronization

---

## 🔧 Hardware Requirements

* STM32F407G-DISC1 board
* USB cable (ST-Link)
* Optional USB-to-TTL converter
* Potentiometer / analog voltage source
* Jumper wires

---

## 🔌 Pin Configuration

| Peripheral | Pin  | Purpose       |
| ---------- | ---- | ------------- |
| ADC1_CH0   | PA0  | Analog input  |
| USART2_TX  | PA2  | UART output   |
| GND        | GND  | Ground        |
| VREF       | 3.3V | ADC reference |

---

## ⏱ Timer Configuration (TIM2)

| Parameter    | Value        |
| ------------ | ------------ |
| Clock source | 16 MHz (HSI) |
| Prescaler    | 15999        |
| Auto-reload  | 9            |
| Trigger rate | 100 Hz       |

```
16 MHz / 16000 = 1 kHz
1 kHz / 10     = 100 Hz ADC trigger
```

---

## 📤 UART Configuration

* Baud rate: **115200**
* Data bits: 8
* Parity: None
* Stop bits: 1

Compatible with:

* PuTTY
* Tera Term
* Arduino Serial Monitor

---

## 🔁 System Data Flow

```
Analog Signal
     │
     ▼
   PA0 (ADC1_CH0)
     │
     ▼
   ADC1
     │  (Triggered by TIM2 TRGO)
     ▼
   DMA2 Stream0
     │
     ▼
  adc_buffer (RAM)
     │
     ▼
  USART2 (PA2)
     │
     ▼
 Serial Terminal
```

---

## 🧠 Code Execution Flow

1. Enable clocks (RCC)
2. Configure GPIO pins
3. Initialize USART2
4. Configure TIM2 for TRGO
5. Configure DMA2 Stream0
6. Configure ADC1
7. Arm ADC with dummy SWSTART
8. ADC conversions run automatically
9. DMA updates memory continuously
10. CPU prints ADC values via UART

---

## 🧪 Example Serial Output

```
1240
1238
1235
1229
1215
```

(12-bit ADC → Range: **0–4095**)

---

## ⚠ Important Design Notes

### 🔹 Why dummy `SWSTART` is required

* ADC external trigger will **not start** unless ADC is first armed
* `SWSTART` is used **once only**

### 🔹 Why ADC delay is needed

* ADC internal circuits require stabilization after enabling
* Prevents incorrect first conversion

### 🔹 Why UART delay is added

* ADC runs at 100 Hz
* UART is slower
* Delay improves readability

---

## 🐞 Troubleshooting Guide

### ❌ ADC value always 0

* PA0 not in analog mode
* No analog input connected
* ADC clock not enabled

### ❌ No UART output

* Wrong COM port
* Baud rate mismatch
* PA2 not configured as AF7

### ❌ ADC not triggering

* TIM2 not enabled
* Wrong EXTSEL value
* Missing dummy SWSTART

### ❌ DMA not working

* DMA2 clock not enabled
* Stream not disabled before configuration
* Wrong channel selection

---

## 🗂 Repository Structure

```
STM32F407_ADC_DMA_TIM2/
│
├── main.c
└── README.md
```

---

## 🧩 Why This Is a Strong Portfolio Project

✔ Uses hardware triggers
✔ Demonstrates DMA mastery
✔ No polling / no busy-wait ADC
✔ Real-time embedded design
✔ Interview-relevant STM32 knowledge

---

## 🚀 Possible Extensions

* Multiple ADC channels (scan mode)
* Interrupt-driven UART
* PLL @ 84 MHz
* FreeRTOS task-based version
* Low-power ADC sampling

---

## 📚 References

* STM32F407 Reference Manual (RM0090)
* STM32F407 Datasheet
* ARM Cortex-M4 TRM

---

## 👤 Author

**Dhinushree M**
Embedded Systems | Bare-Metal STM32 | Embedded C

---
