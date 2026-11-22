# Handoff to the NFC reader firmware workspace

**Target** `C:\dev\ESP32Home\nfc-reader`. The local repository scaffold already exists. Open that checkout and use `C:\dev\ESP32Home\nfc_audio` as the firmware reference and `C:\dev\ESP32Home\robot_serial_display` as the workflow reference. Inspect the existing target files and adopt the relevant ESP32-C3 PlatformIO and Arduino settings in the firmware workspace when implementation is authorized.

The [as-built harness](../../../../projects/nfc-reader/README.md#as-built-wiring) uses GPIO5 for SDA and GPIO6 for SCL, shared by the onboard OLED and the PN532. Start at 100 kHz and use 7-bit addresses `0x3C` (OLED) and `0x24` (PN532). The PN532 is set to 1 ON and 2 OFF for I²C. The four-wire harness provides no IRQ or reset lead. Check the chosen driver's polling support before reusing the prototype's NFC code. Keep the first milestone scoped to the bus scan, a firmware-version query and UID display on the OLED.

Build work can proceed independently of the hardware checks. Before powered testing, record continuity, PN532 VCC suitability, and 3.3 V-compatible SDA/SCL levels per the [bench checklist](../../../../projects/nfc-reader/README.md#bench-checks). Return the build target, dependency versions, and any observed scan, firmware version, and repeatable tag-read results. Carry forward any unverified checks.

## Prepared firmware documentation

The [firmware-docs](firmware-docs/) directory contains the requested documentation in target-relative layout. Apply its four Markdown files to the target checkout from a session with write access. Their cross-repository links resolve from the installed locations. The [bring-up plan](firmware-docs/.plans/2026/11/22/reader-bringup/plan.md) is ready for review. PlatformIO configuration and firmware generation await a separate implementation request.
