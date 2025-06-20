20-06-2025 12-13
Status: #OSDev
Up: [[OS Dev Kernel Initialisation]] [[OSDev]]

For a small OS, a **bump allocator** (also called a linear allocator) is the easiest to implement.

---
## 1. **What is a Bump Allocator?**

- You define a region of memory for the heap.
- You keep a pointer to the "current end" of the heap.
- Each allocation returns the current pointer and moves it forward by the requested size (optionally aligned).
- **No free:** Memory is only reclaimed if you reset the pointer (e.g., on reboot).

---

## 2. **Minimal Example**

**heap.h**
```c
#pragma once

#include <stddef.h>

void heap_init(void* heap_start, size_t heap_size);

void* simple_malloc(size_t size);
```

**heap.c**
```c
#include "heap.h"

  

static uint8_t* heap_ptr = 0;

static uint8_t* heap_end = 0;

  

void heap_init(void* heap_start, size_t heap_size) {

    heap_ptr = (uint8_t*)heap_start;

    heap_end = heap_ptr + heap_size;

}

  

void* simple_malloc(size_t size) {

    // Align to 8 bytes

    size = (size + 7) & ~7;

    if (heap_ptr + size > heap_end) return 0; // Out of memory

    void* ptr = heap_ptr;

    heap_ptr += size;

    return ptr;

}
```

---

## 3. **How to Use**

**In your `main.c`:**
```c
#include "heap.h"

  

#define HEAP_START 0x90000

#define HEAP_SIZE  (128 * 1024) // 128 KB

  

void main(void) {

    heap_init((void*)HEAP_START, HEAP_SIZE);

  

    void* buf = simple_malloc(64);

    // Use buf...

  

    while (1) { }

}
```

- Make sure `HEAP_START` does not overlap your code, stack, or other data.
- You can define the heap region in your linker script or just pick a safe address above your kernel.

---

## 4. **Summary Table**

|Function|Purpose|
|---|---|
|`heap_init`|Set up heap region|
|`simple_malloc`|Allocate memory (no free)|



---
# References
