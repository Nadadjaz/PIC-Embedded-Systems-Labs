# PIC-Embedded-Systems-Labs
# PIC Microcontroller Labs — Embedded Systems & RTOS

A collection of practical labs completed during my Electronics of Embedded Systems degree at the University of Jijel. All programs are written in CCS C for PIC microcontrollers.

---

## Labs Overview

### TP1 — LED Blink with TMR0 (`tp1_embedded_systems.c`)
Getting started with PIC timers. Two approaches: software delay loop first, then hardware TMR0 with interrupts. Calculated overflow times and verified on oscilloscope.

**Key concepts:** Timer0, prescaler, ISR, delay calculation
**Hardware:** PIC16F84A, LED, oscilloscope

---

### TP2 — Code Composer Studio (`tp2_code_composer.c`)
Learned CCS C environment and built several small programs:
- Sequential LED patterns (pairs: LED1+LED8, LED2+LED7...)
- 7-segment display counter 00→99
- Counter controlled by two buttons (increment/decrement)
- TMR0 interrupt-driven LED blink at exactly 500ms

**Key concepts:** 7-segment encoding, debounce, TMR0 interrupt, shift register
**Hardware:** PIC16F84A, 7-seg display, push buttons

---

## TP3 — Watchdog Timer (WDT)

This lab was about system reliability — what happens when your program crashes, and how the hardware can recover automatically without human intervention.

The Watchdog Timer is an independent hardware counter that runs in the background. If the program doesn't reset it periodically (by calling `restart_wdt()`), it overflows and forces a full system reset. It's a safety net for embedded systems that can't afford to stay frozen.

The lab program works like this:

- On startup (or after a WDT reset): LED on **RB7** lights for 2 seconds — visible proof that a reset occurred
- Then LED on **RB6** starts blinking — normal operation
- If the button on **RB0** is pressed: the program enters an intentional infinite loop (simulating a crash)
- The WDT is no longer reset → it overflows → PIC resets → RB7 lights again

The default WDT overflow is ~18ms. I modified it to **576ms** using a prescaler of 32:

```
576ms = 18ms × 32
```

One thing I found useful in CCS C is the `RESTART_WDT` option in `#use delay()`, which automatically resets the watchdog inside every `delay_ms()` call. This means normal delays don't accidentally trigger a reset, but a true infinite loop still will.

**What I used:** PIC16F84A, CCS C, Proteus simulation, push button

---
### TP4 — RTOS Multitasking (`tp4_rtos.c`)
First experience with a Real-Time Operating System on a microcontroller. Built two systems:

**Part 1:** Four LEDs blinking independently at different rates (250ms, 500ms, 1s, 2s) — all running concurrently using RTOS tasks.

**Part 2:** Voltage measurement system with three concurrent tasks:
- `Live` → blinks indicator LED every 200ms
- `Get_voltage` → reads ADC every 2s, converts to millivolts
- `To_RS232` → sends voltage to PC every 1s

Used semaphores to synchronize ADC reading and RS232 transmission.

**Key concepts:** RTOS tasks, semaphores, ADC, RS232, concurrent programming
**Hardware:** PIC16F877A, LM35 sensor, PC terminal

---

## Tools Used

- **Compiler:** CCS C Compiler
- **Simulator:** Proteus ISIS
- **Hardware:** PIC16F84A, PIC16F877A
- **IDE:** Code Composer Studio / MPLAB

---

## Author

**Nada Djazari** — Electronics of Embedded Systems Engineer
📧 nada.djazari@gmail.com | 🎓 University of Jijel, Algeria
