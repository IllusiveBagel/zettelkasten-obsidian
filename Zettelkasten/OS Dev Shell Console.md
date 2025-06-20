20-06-2025 15-44
Status: #OSDev 
Up: [[OS Dev Shell Design]] [[OSDev]]

## 1. **Add a Simple Screen Console**

Let’s make a minimal “screen console” that:

- Tracks a cursor position (row, column)
- Prints characters and strings to the framebuffer using your font
- Handles newlines and scrolling

**console.h**
```c
#pragma once
#include <stdint.h>

void console_init(uint32_t fg, uint32_t bg);
void console_putc(char c);
void console_puts(const char* s);
void console_newline(void);
void console_clear(void);
```

**console.c**
```c
#include "console.h"
#include "framebuffer.h"
#include "font8x8.h"
#include <string.h>

#define CONSOLE_COLS (fb_width / 8)
#define CONSOLE_ROWS (fb_height / 8)

static uint32_t cursor_x = 0, cursor_y = 0;
static uint32_t fg_color = 0xFFFFFF, bg_color = 0x000000;

void console_init(uint32_t fg, uint32_t bg) {
    fg_color = fg;
    bg_color = bg;
    cursor_x = 0;
    cursor_y = 0;
    fb_clear(bg_color);
}

void console_newline(void) {
    cursor_x = 0;
    cursor_y++;
    if (cursor_y >= CONSOLE_ROWS) {
        // Scroll up
        for (uint32_t y = 1; y < CONSOLE_ROWS; y++) {
            for (uint32_t x = 0; x < CONSOLE_COLS; x++) {
                // Copy char from (x, y) to (x, y-1)
                // For simplicity, just clear the last line
            }
        }
        fb_clear(bg_color); // Simple: clear screen instead of scrolling
        cursor_y = 0;
    }
}

void console_putc(char c) {
    if (c == '\n') {
        console_newline();
        return;
    }
    fb_draw_char(cursor_x * 8, cursor_y * 8, c, fg_color, bg_color);
    cursor_x++;
    if (cursor_x >= CONSOLE_COLS) {
        console_newline();
    }
}

void console_puts(const char* s) {
    while (*s) {
        console_putc(*s++);
    }
}

void console_clear(void) {
    fb_clear(bg_color);
    cursor_x = 0;
    cursor_y = 0;
}
```

---

## 2. **Update Your Shell to Output to Both UART and Screen**

**shell.c**
```c
#include "uart.h"
#include "console.h"
#include <string.h>

static void shell_help(void) {
    uart_puts("Commands:\n");
    console_puts("Commands:\n");
    uart_puts("  help   - Show this help\n");
    console_puts("  help   - Show this help\n");
    uart_puts("  echo   - Echo input\n");
    console_puts("  echo   - Echo input\n");
    uart_puts("  clear  - Clear screen (if supported)\n");
    console_puts("  clear  - Clear screen (if supported)\n");
}

static void shell_clear(void) {
    uart_puts("\033[2J\033[H");
    console_clear();
}

void shell_run(void) {
    char buf[128];

    uart_puts("Welcome to BagelOS shell!\nType 'help' for commands.\n");
    console_puts("Welcome to BagelOS shell!\nType 'help' for commands.\n");

    while (1) {
        uart_puts("> ");
        console_puts("> ");
        uart_gets(buf, sizeof(buf));

        console_puts(buf);
        console_putc('\n');

        if (strcmp(buf, "help") == 0) {
            shell_help();
        } else if (strncmp(buf, "echo ", 5) == 0) {
            uart_puts(buf + 5);
            uart_puts("\n");
            console_puts(buf + 5);
            console_putc('\n');
        } else if (strcmp(buf, "clear") == 0) {
            shell_clear();
        } else if (buf[0] == 0) {
            // Ignore empty input
        } else {
            uart_puts("Unknown command. Type 'help'.\n");
            console_puts("Unknown command. Type 'help'.\n");
        }
    }
}
```

---

## 3. **Initialize the Framebuffer and Console in main.c**

**main.c**
```c
#include "uart.h"
#include "framebuffer.h"
#include "console.h"
#include "shell.h"

void main(void) {
    uart_init();
    fb_init(640, 480, 32);
    console_init(0xFFFFFF, 0x000000); // White on black
    shell_run();
}
```

---

## 4. **Result**

- Your shell prompt and output will appear on both UART and HDMI screen.
- Input is still via UART for now.

---
# References
