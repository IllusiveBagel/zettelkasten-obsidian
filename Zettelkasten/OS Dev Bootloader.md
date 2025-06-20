20-06-2025 09-59
Status: #OSDev 
Up: [[BagelOS]] [[OSDev]]

## 1. **Raspberry Pi Boot Process (Bare Metal)**

- The Pi’s GPU loads [bootcode.bin](vscode-file://vscode-app/c:/Users/LoganDeeley/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html), then [start.elf](vscode-file://vscode-app/c:/Users/LoganDeeley/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html), then looks for `kernel8.img` (for 64-bit, Pi 3/4) on the SD card.
- `kernel8.img` is loaded at address `0x80000` and executed in EL2 (hypervisor mode) or EL1 (kernel mode), depending on config.

**You do NOT need to write a traditional bootloader**—just provide a `kernel8.img` binary.

---

## 2. **Minimal Bootloader/Startup Code**

You need a minimal assembly file to:

- Set up the stack pointer
- Jump to your C kernel entry point

### **Example: `start.S`**

```asm
.section .text

.global _start

_start:

    // Set up stack pointer (e.g., at 0x80000 + 0x8000 = 0x88000)

    ldr x0, =_stack_top

    mov sp, x0

    // Call main()

    bl main

    // Infinite loop if main returns

1:  wfe

    b 1b

// Define stack (32KB)

.section .bss

.align 12

_stack:

    .skip 0x8000

_stack_top:
```

---

## 3. **Minimal C Kernel Entry**

### **Example: `main.c`**

```c
void main(void) {

    // For now, just loop forever

    while (1) { }

}
```

---

## 4. **Linker Script**

### **Example: linker.ld**

```
ENTRY(_start)

SECTIONS

{

    . = 0x80000;

    .text : { *(.text*) }

    .rodata : { *(.rodata*) }

    .data : { *(.data*) }

    .bss : { *(.bss*) *(COMMON) }

}
```

---

## 5. **Build Instructions**

- Assemble and link using your Makefile (see previous answer).
- Output must be named `kernel8.img` and placed on the SD card’s boot partition.

---

## 6. **Booting**

- Copy `kernel8.img` to the FAT32 boot partition of your SD card.
- Insert into Pi and power on.

---
# References
