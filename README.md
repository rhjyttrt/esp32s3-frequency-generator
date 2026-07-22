# ESP32S3 Frequency Generator

A high performance 3 channel programmable frequency generator built for the **WeAct Studio ESP32S3** and **Si5351** clock generator.

[![Simulate on Cirkit Designer](https://img.shields.io/badge/Cirkit%20Designer-Simulate%20Online-blue?style=for-the-badge)](https://app.cirkitdesigner.com/project/6a74417e-ec1c-4245-867d-878b34f4edb9)

![ESP32S3 Frequency Generator Preview](image.png)

>  **Note:** The interface text on the screen looks scrunched up in this preview because the UI layout was designed specifically for a dual color SSD1306 OLED display with a yellow top section. You can view the full schematic(note its just the UI nothing else) and interactive layout directly on [Cirkit Designer](https://app.cirkitdesigner.com/project/6a74417e-ec1c-4245-867d-878b34f4edb9).

---

## Hardware Requirements

* **MCU:** WeAct Studio ESP32S3 (**16MB Flash memory** required for the precompiled binary otherwise use the ino provided)
* **Clock Generator:** Si5351 Module (I2C)
* **Display:** SSD1306 OLED (128x64, I2C)
* **Inputs:** Rotary Encoder (EC11) & 3x Tactile Push Buttons or substitute one button with the encoders button
* **Indicator:** Onboard WS2812 NeoPixel RGB LED (`GPIO 45`)

---

## Features

1. **60 FPS Fluid UI:** Non blocking rendering loop prevents display draw times from lagging or dropping encoder pulses.
2. **Dynamic Range Tuning:** Fixed point 64 bit math tracking from **8 kHz up to 160 MHz** in steps down to **0.1 Hz**.
3. **Independent Channel Memories:** 3 active channel configurations (`CLK0`, `CLK1`, `CLK2`) stored in working RAM with manual save overrides.

---

## Hardware Connections

| ESP32S3 Pin | Function / Component | Description |
| :--- | :--- | :--- |
| **GPIO 1** | Step Button | Tuning increment adjust (`0.1 Hz` to `1 MHz`) |
| **GPIO 2** | Page Button | Channel cycle (`Ch 1` → `Ch 2` → `Ch 3`) |
| **GPIO 4** | I2C SDA | Shared I2C Data Bus (SSD1306 & Si5351) |
| **GPIO 5** | I2C SCL | Shared I2C Clock Bus (SSD1306 & Si5351) |
| **GPIO 6** | Save Button | Commit working RAM |
| **GPIO 7** | Encoder A | Quadrature Phase A |
| **GPIO 15** | Encoder B | Quadrature Phase B |
| **GPIO 45** | Status LED | Onboard WS2812 RGB NeoPixel |

---

## Software Dependencies

If compiling from source, install the following libraries via your IDE Library Manager:

* **Adafruit SSD1306** (`Adafruit_SSD1306`)
* **Adafruit GFX Library** (`Adafruit_GFX`)
* **Etherkit Si5351** (`si5351` by Jason Milldrum)
* **Adafruit NeoPixel** (`Adafruit_NeoPixel`)

---

## Flashing & Deployment

>  **Important:** Your ESP32S3 **must have 16MB of Flash memory** to use the precompiled `.bin` file due to its custom partition layout. If your board has a smaller flash size (such as 4MB or 8MB), **you must compile and upload using the `.ino` source code directly**.

### Option A: Precompiled Binary (16MB Flash Only)

Download the merged `.bin` file and deploy using `esptool`:

```bash
esptool.py --chip esp32s3 write_flash 0x0 si5351_onS3.ino.merged.bin
