# ESP32-S2 Cucumber v1.1 PN532 HSU test

**Target workspace** `C:/dev/ESP32Home/nfc-reader`. Work in `.plans/2026/11/22/reader-bringup/` and follow the firmware repo's AGENTS.md and WORKFLOW.md. Read the “ESP32-S2 Cucumber v1.1 serial bench test” section in `C:/dev/VectorMind/electro-context/projects/nfc-reader/README.md`. The matching interface is in the adjacent `spec.md`.

The owner's C3 HSU diagnostic repeatedly timed out waiting for the PN532 command ACK. Build a focused ESP32-S2 test for the same breakout and card. Keep the working-tree C3 changes and its build target intact.

## Build and serial contract

- Add an isolated Arduino PlatformIO environment, suggested name `cucumber_s2_hsu`. Verify the selected board definition's ESP32-S2 target, flash size and settings against the WROVER module. Reuse the existing ESP32 platform and pinned Seeed Studio PN532 HSU dependency where compatible. Keep the S2 build free of OLED dependencies.
- Set the PN532 to HSU with both switches OFF (0/0) and power-cycle it. Use hardware UART1 at 115200 baud, 8N1, no flow control. Assign TX GPIO17 to PN532 RXD and RX GPIO16 to PN532 TXD, with common ground. The existing `PN532_HSU` constructor takes RX before TX, so pass `Serial1, 16, 17`. Reuse the existing version query and its separate ACK and response checks.
- Send startup and runtime logs to hardware UART0 through the DEBUG USB-C FTDI bridge at 115200 baud, 8N1, no flow control. Select UART0 explicitly or build with USB CDC on boot disabled. The DEBUG COM port serves both upload and monitoring. Keep OTG unplugged during the test.
- Print a startup banner identifying the ESP32-S2 target, HSU settings and GPIO mapping. Query `GetFirmwareVersion` at startup and retry at a bounded interval if it fails. Report missing or invalid ACK separately from missing or invalid response, and show the IC and firmware version on success. Once the PN532 responds, configure reader mode and poll for ISO/IEC 14443A tags with bounded timeouts. Print all UID bytes with leading zeros, and report a fresh read after removal and re-presentation. Keep routine no-tag polls quiet or rate-limited, with a periodic status line to show the sketch is active.

## Acceptance and evidence

1. Build the S2 test and verify that the C3 environment still builds. Record the target, dependency versions, exact commands, build result and binary path.
2. Before attaching the UART wires, verify continuity, shared ground, PN532 supply suitability and 3.3 V-compatible signal levels per the Electro README. Give the owner the exact upload and monitor commands using the DEBUG COM port discovered on their PC. Flashing requires an explicit request.
3. Capture the S2 boot banner and two successful PN532 firmware-version responses, across a reset if needed. Present the S50 card, remove it, and present it again without rebooting. Capture the same full UID both times. If a step fails, return the serial output identifying the failed stage.
4. Record build and bench results separately in the firmware packet and return the serial evidence and any pending checks to the Electro owner.
