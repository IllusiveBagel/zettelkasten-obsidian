20-06-2025 10-25
Status: #OSDev 
Up: [[BagelOS]] [[OSDev]]

## 1. **What Kernel Initialization Involves**

- Set up the stack pointer (already done in [start.S](vscode-file://vscode-app/c:/Users/LoganDeeley/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html))
- Set up exception vectors (for handling interrupts and faults)
- Initialize UART (for serial output/debugging)
- Optionally, initialize a simple memory allocator
- Prepare for screen/framebuffer output

---

## 2. **Exception Vector Table**

You need to define an exception vector table and set the `VBAR_EL1` register to its address.

**Example: `vectors.S`**

```asm
.section .vectors, "ax"
.align 11
.global vectors

vectors:
    // Synchronous EL1t
    b .
    // IRQ EL1t
    b .
    // FIQ EL1t
    b .
    // SError EL1t
    b .
    // Synchronous EL1h
    b .
    // IRQ EL1h
    b .
    // FIQ EL1h
    b .
    // SError EL1h
    b .
    // Synchronous EL0_64
    b .
    // IRQ EL0_64
    b .
    // FIQ EL0_64
    b .
    // SError EL0_64
    b .
    // Synchronous EL0_32
    b .
    // IRQ EL0_32
    b .
    // FIQ EL0_32
    b .
    // SError EL0_32
    b .
```

**In your `main` or right after `_start`:**

```c
extern void vectors(void);

  

static inline void set_vbar_el1(void* addr) {

    asm volatile ("msr VBAR_EL1, %0" :: "r"(addr));

}

  

void main(void) {

    set_vbar_el1((void*)vectors);

    // ...rest of your init...

    while (1) { }

}
```

---

## 3. **UART Initialization**

Set up the UART for serial output. This is invaluable for debugging.

- You’ll need to write to the Pi’s PL011 UART registers.
- There are many tutorials and code samples for this (see [Baking Pi](vscode-file://vscode-app/c:/Users/LoganDeeley/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) or [Circle](vscode-file://vscode-app/c:/Users/LoganDeeley/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html)).

---

## 4. **Optional: Simple Memory Allocator**

For now, you can skip this or use a simple bump allocator if you need dynamic memory.

---

## 5. **Framebuffer Initialization**

You’ll use the mailbox interface to set up a framebuffer for screen output.  
This can be done after UART is working.


---
# References
