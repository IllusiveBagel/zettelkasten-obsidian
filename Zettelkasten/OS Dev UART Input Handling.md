20-06-2025 14-16
Status: #OSDev 
Up: [[OS Dev UART Initialisation]] [[OSDev]]

### 1. **UART Input Handling**

### **Add a UART getc function**

In your `uart.c`:
```c
char uart_getc(void)
{
    // Wait until the UART has received something
    while (UART0_FR & (1 << 4));
    return (char)(UART0_DR & 0xFF);
}
```

Add the prototype to `uart.h`:
```c
char uart_getc(void);
```

---

### **Reading a Line from UART**

You can implement a simple line input function:
```c
#include "uart.h"

void uart_gets(char* buf, int maxlen)
{
    int i = 0;
    while (i < maxlen - 1) {
        char c = uart_getc();
        if (c == '\r' || c == '\n') {
            uart_putc('\n');
            break;
        }
        if (c == 0x7F || c == 0x08) { // Handle backspace
            if (i > 0) {
                i--;
                uart_puts("\b \b");
            }
            continue;
        }
        uart_putc(c); // Echo
        buf[i++] = c;
    }
    buf[i] = 0;
}
```

---

### **Using UART Input in Your Shell**
```c
#include "uart.h"

void shell(void)
{
    char buf[128];
    while (1) {
        uart_puts("> ");
        uart_gets(buf, sizeof(buf));
        // Now buf contains the user input
        // Parse and execute commands here
        uart_puts("You typed: ");
        uart_puts(buf);
        uart_puts("\n");
    }
}
```

---
# References
