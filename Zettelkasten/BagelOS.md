20-06-2025 09-41
Status: #OSDev
Tags: [[OSDev]]

## 1. **Toolchain & Environment**

- Set up a cross-compiler toolchain for ARM (e.g., `arm-none-eabi-gcc`).
- Prepare a build system (Makefile, CMake, etc.).
- Get a way to write images to SD cards (for Pi) and a UART/serial cable for debugging.

---

## 2. **Bootloader**

- Start with a minimal bootloader (for Pi, this can be just `kernel8.img` on the SD card).
- The bootloader should set up the CPU, stack, and jump to your kernel’s entry point.

---

## 3. **Kernel Initialization**

- Set up the stack pointer.
- Initialize basic exception vectors.
- Optionally, set up a simple memory allocator (bump allocator).
- Initialize the UART for serial output (for debugging).

---

## 4. **Screen Output**

- Initialize the framebuffer (for Pi: use mailbox interface to set up a framebuffer).
- Write simple routines to draw text to the screen (bitmap font, or use the Pi’s built-in font).
- Optionally, implement basic graphics primitives (clear screen, draw rectangle, etc.).

---

## 5. **Input Handling**

- For a shell, you’ll want keyboard input.
    - On Pi, this is usually via USB (so you’ll need a USB stack and HID driver), or you can start with UART serial input for simplicity.

---

## 6. **Shell Implementation**

- Implement a simple command-line shell:
    - Read input (from UART or keyboard).
    - Parse commands.
    - Execute built-in commands (e.g., `help`, `clear`, `echo`, etc.).
    - Display output to the screen.

---

## 7. **Main Loop**

- Your kernel should enter a main loop that:
    - Waits for input.
    - Processes commands.
    - Updates the display.

---

## 8. **Optional: File System**

- For more advanced shells, add a simple file system (e.g., read files from FAT32 on SD card).

---

## **Minimal Checklist**

| Step        | What to Do                     |
| ----------- | ------------------------------ |
| Toolchain   | Cross-compiler, build system   |
| Bootloader  | Minimal loader, jump to kernel |
| Kernel Init | Stack, UART, exception vectors |
| Framebuffer | Set up, draw text              |
| Input       | UART or USB keyboard           |
| Shell       | Read, parse, execute commands  |
| Main Loop   | Wait for input, update display |



---
# References