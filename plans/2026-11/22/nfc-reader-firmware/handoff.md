# Handoff to the NFC reader firmware workspace

Resolve the `firmware` entry in the [device manifest](../../../../projects/nfc-reader/project.yaml) using the [host configuration](../../../../config/README.md). If the checkout is missing, use its `clone_url` to restore it at the resolved path. Follow the firmware checkout's AGENTS.md and WORKFLOW.md and continue in `.plans/2026/11/22/reader-bringup/`. The manifest also resolves the prototype and historical workflow references.

The selected configuration is ESP32-C3 with a dedicated PN532 hardware I²C bus on GPIO0/GPIO1, the onboard OLED on GPIO5/GPIO6 software I²C, and Jewel/Topaz-only polling. The owner confirmed Topaz detection in the earlier Jewel-only bench build. Both current targets compiled with back-to-back Jewel polling and 250 ms removal debounce. See the [device interface](../../../../projects/nfc-reader/spec.md#selected-interface) and the [C3 bench procedure](../../../../projects/nfc-reader/README.md#bench-checks).

## Firmware owner follow-up

- Update the firmware README's reader pins from the old GPIO5/GPIO6 wiring to GPIO0 SDA and GPIO1 SCL. Put the C3 and Jewel bench flow first. Park the Cucumber target at the end.
- Refresh the firmware return handoff with the startup text and `Jewel tag UID:` and `Jewel tag removed` events. Keep the older Type A logs as dated evidence.
- Follow the [remaining bench checks](plan.md#remaining-bench-checks) on the C3 and return the exact image, serial events, OLED observation and measured scan timing. The latest build record has the compiled behavior, while the full on-board sequence is pending.

The [firmware-docs](firmware-docs/) directory holds the Electro-side reference copies in target-relative layout. Their cross-repository links assume the standard layout under the configured `dev` root.

## ESP32-S2 Cucumber reference

Historical handoffs cover [HSU](firmware-docs/.plans/2026/11/22/reader-bringup/handoff-cucumber-hsu.md), [I²C comparison](firmware-docs/.plans/2026/11/22/reader-bringup/handoff-cucumber-i2c.md) and [the earlier poll timeout](firmware-docs/.plans/2026/11/22/reader-bringup/handoff-cucumber-i2c-troubleshooting.md). See the [Cucumber reference](../../../../projects/nfc-reader/README.md#esp32-s2-cucumber-v11-reference) for the current serial-only comparison wiring.
