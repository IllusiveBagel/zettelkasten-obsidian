20-06-2025 15-48
Status: #OSDev 
Up: [[OS Dev Shell Console]] [[OSDev]]

To **interact with the console using the HDMI screen** (not just see output), you need to accept input from a device that the user can use with the Pi itself.  
There are two main options:

---

## 1. **USB Keyboard Input (Recommended for HDMI Console)**

- **Why:** Most users expect to use a USB keyboard with a Pi.
- **How:**
    - Implement a USB host stack (TinyUSB is a good choice, but you can write a minimal one for just HID keyboards).
    - Write a USB HID keyboard driver to receive key events.
    - Feed those key events into your shell’s input buffer, and echo them to the framebuffer console.

---

## 2. **Alternative: On-Screen Keyboard or GPIO Buttons**

- Not practical for a real shell, but possible for demos or embedded use.

---

## **What You Need to Do for USB Keyboard Input**

### **A. USB Host Stack**

- Integrate a USB host stack (TinyUSB or similar) into your OS.
- Initialize the USB controller (DWC OTG on Pi 3B).

### **B. HID Keyboard Driver**

- Parse USB HID reports from the keyboard.
- Convert keycodes to ASCII characters (handle modifiers like Shift).

### **C. Console Input Integration**

- When a key is pressed, add it to your shell’s input buffer.
- Echo the character to the framebuffer console (and optionally UART).
- Handle backspace, enter, etc.

---

## **Minimal Steps to Get Started**

1. **Integrate TinyUSB (or your own minimal USB host) into your project.**
2. **Implement a function like:**
  ```c
   char keyboard_getc(void); // Blocks until a key is pressed
 ```
3. **Replace `uart_getc()` in your shell with `keyboard_getc()` (or support both).**
4. **Echo input to the framebuffer console as the user types.**

---

## **Summary Table**

|Step|What to Do|
|---|---|
|USB Host Stack|Integrate TinyUSB or minimal host|
|HID Keyboard Driver|Parse key events, convert to ASCII|
|Console Input Integration|Feed keys into shell, echo to framebuffer|



---
# References
