# NFC reader firmware specification

**Delivery state** Planned. Build and bench verification are pending.

The first milestone reads the supplied card with the PN532 and shows its complete UID on the ESP32-C3 board's 0.42-inch OLED. Audio playback is outside this milestone.

| Interface | Firmware contract |
| --- | --- |
| Shared hardware I²C | Initialize SDA on GPIO5 and SCL on GPIO6 at 100 kHz. Both drivers must preserve this configuration. |
| Address scan | Report responding 7-bit addresses over serial so the operator can check for the OLED at 0x3C and PN532 at 0x24 independently. |
| Reader startup | Query and report the PN532 firmware version. Distinguish an address acknowledgement from a successful command response. |
| Reader transport | Poll readiness over I²C. The harness has no IRQ or reset lead. The physical switches are set to 1 ON and 2 OFF. |
| Tag presentation | Show all returned UID bytes in read order, preserving leading zeros. After removal and re-presentation, acquire and display the UID again without rebooting. |

Use the [device README](../../../VectorMind/electro-context/projects/nfc-reader/README.md#bench-checks) for the powered-test prerequisites. A successful build establishes compilation only. The expected addresses and version query must be checked on the assembled hardware.
