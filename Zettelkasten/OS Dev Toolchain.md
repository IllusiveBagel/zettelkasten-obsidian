20-06-2025 09-46
Status: #OSDev
Up: [[BagelOS]] [[OSDev]]

## 1. **Choose a Cross-Compiler**

For ARM bare-metal (AArch64 for Pi 3B), use the GNU Arm Embedded Toolchain:

- **AArch64 (64-bit, Pi 3B):**  
    `aarch64-none-elf-gcc` (recommended for Pi 3B+)
- **ARM32 (older Pis):**  
    `arm-none-eabi-gcc`

---

## 2. **Install the Toolchain**

### **On Windows**

- Download the [Arm GNU Toolchain for AArch64](vscode-file://vscode-app/c:/Users/LoganDeeley/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) (choose the Windows installer for `aarch64-none-elf`).
- Install and add the `bin` directory to your PATH.

### **On Linux/macOS**

```bash
# Ubuntu/Debian (AArch64)

sudo apt update

sudo apt install gcc-aarch64-linux-gnu

# Or download prebuilt binaries from Arm's website:

# https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads
```
---

## 3. **Verify Installation**

Open a terminal or command prompt and run:

```aarch64-none-elf-gcc --version```

You should see version info for the cross-compiler.

---

## 4. **Set Up Your Build System**

- Create a `Makefile` or use CMake.
- Example `Makefile` snippet:

```makefile
CROSS_COMPILE = aarch64-none-elf-

CC = $(CROSS_COMPILE)gcc

LD = $(CROSS_COMPILE)ld

OBJCOPY = $(CROSS_COMPILE)objcopy

  

all: kernel8.img

  

kernel8.img: kernel.elf

    $(OBJCOPY) kernel.elf -O binary kernel8.img

  

kernel.elf: start.o main.o

    $(LD) -T linker.ld start.o main.o -o kernel.elf

  

%.o: %.c

    $(CC) -c $< -o $@

  

clean:

    rm -f *.o *.elf *.img
```

---
# References
