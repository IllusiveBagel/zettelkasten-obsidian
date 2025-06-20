20-06-2025 11-04
Status: #OSDev
Up: [[OS Dev Kernel Initialisation]] [[OSDev]]

## 1. **PL011 UART Base Address**

For Pi 3B:

- **PL011 UART base:** `0x3F201000` (for Pi 2/3, BCM2837)
- For Pi 4, it’s `0xFE201000`

---

## 2. **Minimal UART Initialization Code**

**uart.h**
```c
#pragma once

#include <stdint.h>

  

void uart_init(void);

void uart_putc(char c);

void uart_puts(const char* s);
```

**uart.c**
```c
#include "uart.h"

  

#define UART0_BASE 0x3F201000UL

  

#define UART0_DR     (*(volatile uint32_t*)(UART0_BASE + 0x00))

#define UART0_FR     (*(volatile uint32_t*)(UART0_BASE + 0x18))

#define UART0_IBRD   (*(volatile uint32_t*)(UART0_BASE + 0x24))

#define UART0_FBRD   (*(volatile uint32_t*)(UART0_BASE + 0x28))

#define UART0_LCRH   (*(volatile uint32_t*)(UART0_BASE + 0x2C))

#define UART0_CR     (*(volatile uint32_t*)(UART0_BASE + 0x30))

#define UART0_IMSC   (*(volatile uint32_t*)(UART0_BASE + 0x38))

#define UART0_ICR    (*(volatile uint32_t*)(UART0_BASE + 0x44))

  

void uart_init(void)

{

    // Disable UART0.

    UART0_CR = 0x00000000;

  

    // Clear pending interrupts.

    UART0_ICR = 0x7FF;

  

    // Set integer & fractional part of baud rate.

    // Divider = UART_CLOCK/(16 * Baud)

    // UART_CLOCK = 48 MHz, Baud = 115200.

    // Divider = 26.041666...

    UART0_IBRD = 26;

    UART0_FBRD = 3;

  

    // Enable FIFO & 8 bit data transmission (1 stop bit, no parity).

    UART0_LCRH = (1 << 4) | (3 << 5);

  

    // Mask all interrupts.

    UART0_IMSC = 0;

  

    // Enable UART0, receive & transfer part of UART.

    UART0_CR = (1 << 0) | (1 << 8) | (1 << 9);

}

  

void uart_putc(char c)

{

    // Wait for UART to become ready to transmit.

    while (UART0_FR & (1 << 5));

    UART0_DR = c;

}

  

void uart_puts(const char* s)

{

    while (*s) {

        if (*s == '\n')

            uart_putc('\r');

        uart_putc(*s++);

    }

}
```

---

## 3. **Using UART in Your Kernel**

**[main.c](vscode-file://vscode-app/c:/Users/LoganDeeley/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html)**
```c
#include "uart.h"

void main(void)
{
    uart_init();
    uart_puts("Hello, UART!\n");

    while (1) { }

}
```

---

## 4. **Wiring for Serial Output**

- Connect a USB-to-serial adapter to the Pi’s GPIO pins:
    
    - GPIO14 (TXD) = Pin 8
    - GPIO15 (RXD) = Pin 10
    - GND = Pin 6
- Use a terminal program (PuTTY, minicom, etc.) at **115200 baud, 8N1**.
---
# References
