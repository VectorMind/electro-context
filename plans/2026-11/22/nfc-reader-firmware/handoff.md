# Handoff to the NFC reader firmware workspace

Resolve `firmware`, `prototype_reference`, and `firmware_sdd_reference` in the [device manifest](../../../../projects/nfc-reader/project.yaml) using the [host configuration](../../../../config/README.md). If the firmware checkout is missing, clone its `clone_url` into the resolved `firmware` path. Inspect the prototype's PlatformIO configuration and the layout of the reference checkout. Use the prepared firmware workflow below for the bring-up process. Adopt the relevant ESP32-C3 PlatformIO and Arduino settings when implementation is authorized.

The [as-built harness](../../../../projects/nfc-reader/README.md#as-built-wiring) uses GPIO5 for SDA and GPIO6 for SCL, shared by the onboard OLED and the PN532. Start at 100 kHz and use 7-bit addresses `0x3C` (OLED) and `0x24` (PN532). The PN532 is set to 1 ON and 2 OFF for I²C. The four-wire harness provides no IRQ or reset lead. Check the chosen driver's polling support before reusing the prototype's NFC code. Keep the first milestone scoped to the bus scan, a firmware-version query and UID display on the OLED.

Build work can proceed independently of the hardware checks. Before powered testing, record continuity, PN532 VCC suitability, and 3.3 V-compatible SDA/SCL levels per the [bench checklist](../../../../projects/nfc-reader/README.md#bench-checks). Return the build target, dependency versions, and any observed scan, firmware version, and repeatable tag-read results. Carry forward any unverified checks.

## Prepared firmware documentation

The [firmware-docs](firmware-docs/) directory contains the requested documentation in target-relative layout. Apply its four Markdown files to the target checkout from a session with write access. The cross-repository links assume the standard layout under the configured `dev` root. The [bring-up plan](firmware-docs/.plans/2026/11/22/reader-bringup/plan.md) is ready for review. PlatformIO configuration and firmware generation await a separate implementation request.
