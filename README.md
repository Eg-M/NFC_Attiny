# NFC_Attiny

A PlatformIO/Arduino prototype for interfacing an ATtiny85 with an ST M24LR NFC/RFID memory chip over I2C. The chip provides user memory accessible over both I2C and NFC, as well as configurable energy-harvesting features.

## Features

The `M24LR` class in `src/M24LR.h` and `src/M24LR.cpp` provides helpers to:

- Read and write bytes and four-byte pages in user memory.
- Write a text message as an NDEF-style payload.
- Enable or disable energy harvesting, set its configuration, and check for an RF field.

The library uses I2C address `0x53` for user memory and `0x57` for system-memory operations. Consult the M24LR datasheet before changing system-memory settings.

## Hardware

- ATtiny85 microcontroller.
- ST M24LR NFC/RFID memory chip or a compatible module exposing its I2C interface.
- An AVR programmer and an upload connection supported by your setup.
- I2C wiring and pull-up resistors as required by your board and module.

## Build and upload

This project uses PlatformIO with the Arduino framework. The project configuration targets the `attiny85` board and declares the `adafruit/TinyWireM` dependency.

1. Install [PlatformIO Core](https://docs.platformio.org/en/latest/core/installation/index.html) or the PlatformIO IDE extension.
2. Clone this repository and open its directory in PlatformIO.
3. In `platformio.ini`, change `upload_port = COM4` to the serial port for your programmer. Check that the configured `stk500v1` upload protocol and speed (`19200`) match your hardware.
4. Build and upload:

   ```sh
   pio run
   pio run -t upload
   ```

## Example behavior

`src/main.cpp` calls its example routine once from `setup()`. It reads analog input `A0`, writes the value as text to the tag, then writes a byte and four-byte page to user memory. The `loop()` is empty, so the example does not continuously write while running.

The M24LR stores these writes in EEPROM, which has limited write endurance. Avoid frequent writes in application code and check your tag's memory layout before changing addresses.

## Project layout

- `src/main.cpp` — example firmware.
- `src/M24LR.h`, `src/M24LR.cpp` — M24LR I2C and memory helpers.
- `platformio.ini` — target board, framework, dependency, and upload settings.
