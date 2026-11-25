# ESP32-S2 Cucumber v1.1 PN532 I²C test

**Target workspace** `C:/dev/ESP32Home/nfc-reader`. Work in `.plans/2026/11/22/reader-bringup/` and follow the firmware repo's AGENTS.md and WORKFLOW.md. The hardware contract and electrical checks are in `C:/dev/VectorMind/electro-context/projects/nfc-reader/spec.md` and `README.md`, under the Cucumber I²C comparison.

The owner captured a valid outgoing HSU version query on the Cucumber. The reader TXD trace showed no visible full-swing ACK in the capture. Try I²C on the same PN532 to check whether the breakout responds over a different interface.

## Build and serial behavior

- Add `cucumber_s2_i2c` as an isolated PlatformIO Arduino environment using the existing Cucumber board settings and UART0 DEBUG console at 115200 baud. Preserve the C3 and Cucumber HSU targets.
- Use GPIO16 for SDA and GPIO17 for SCL at 100 kHz with the PN532 at 7-bit address `0x24`. Use the existing four-wire harness and status-byte polling. The reader must be power-cycled with switch 1 ON and switch 2 OFF. Verify reader VCC and 3.3 V pull-ups on the Cucumber side before joining the bus.
- Evaluate the existing pinned Seeed Studio PN532-Arduino v1.1.0 `PN532_I2C` transport with `NFC_INTERFACE_I2C`. Its `begin()` calls `TwoWire::begin()` without arguments. Ensure the selected pins, clock and finite transaction timeout survive initialization. The stock transport also narrows the signed response-length result before checking for errors. Verify how it handles timeouts and truncated frames before relying on its diagnostics.
- Log startup settings, the address-probe result, PN532 command ACK and firmware-version response as distinct stages. Use bounded retries. Once the version succeeds, configure reader mode and poll for ISO/IEC 14443A. Report the complete S50 UID in hex with leading zeros, then report a fresh read after removal and re-presentation.

## Acceptance and evidence

Build the I²C environment and check the two existing targets. Record exact build, upload and monitor commands using the Cucumber DEBUG COM port. Flashing requires an explicit request.

On the bench, capture an address ACK, a valid firmware-version response, and two reads of the same card separated by removal without rebooting. If it fails, record the first failed stage and any measured SDA/SCL idle voltages. Return build and bench results separately.
