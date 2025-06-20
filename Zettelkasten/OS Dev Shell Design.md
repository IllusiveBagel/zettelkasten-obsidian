20-06-2025 15-08
Status: #OSDev
Up: [[BagelOS]] [[OSDev]]

## 1. **Shell Design**

- Print a prompt (e.g., `>` )
- Read a line from UART
- Parse the command
- Execute built-in commands (like `help`, `echo`, `clear`)
- Loop

---

## 2. **Minimal Shell Implementation**

**shell.h**
```c
#pragma once

void shell_run(void);
```

**shell.c**
```c
#include "uart.h"
#include <string.h>

static void shell_help(void) {
    uart_puts("Commands:\n");
    uart_puts("  help   - Show this help\n");
    uart_puts("  echo   - Echo input\n");
    uart_puts("  clear  - Clear screen (if supported)\n");
}

static void shell_clear(void) {
    // ANSI escape code for clear screen (works in many terminals)
    uart_puts("\033[2J\033[H");
}

void shell_run(void) {
    char buf[128];

    uart_puts("Welcome to BagelOS shell!\nType 'help' for commands.\n");

    while (1) {
        uart_puts("> ");
        uart_gets(buf, sizeof(buf));

        if (strcmp(buf, "help") == 0) {
            shell_help();
        } else if (strncmp(buf, "echo ", 5) == 0) {
            uart_puts(buf + 5);
            uart_puts("\n");
        } else if (strcmp(buf, "clear") == 0) {
            shell_clear();
        } else if (buf[0] == 0) {
            // Ignore empty input
        } else {
            uart_puts("Unknown command. Type 'help'.\n");
        }
    }
}
```

---

## 3. **Update Your main.c**
```c
#include "uart.h"
#include "shell.h"

void main(void) {
    uart_init();
    shell_run();
}
```

---

## 4. **Summary**

- The shell uses UART for all input/output.
- You can add more commands as needed.
- This is a great base for expanding to framebuffer or USB input later.

---
# References
