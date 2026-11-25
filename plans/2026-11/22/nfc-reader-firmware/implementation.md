# NFC reader integration record

## Current C3 I²C and Jewel configuration

- The owner confirmed Topaz detection and selected Jewel/Topaz for the current reader. The firmware packet identifies the reference tag as a WhizTags Topaz 512 via NXP TagInfo and records the successful earlier Jewel-only test at a 500 ms interval.
- The current default `esp32c3` environment uses GPIO0 SDA and GPIO1 SCL for the PN532 at 100 kHz and `0x24`. The OLED uses GPIO5/GPIO6 on its own software I²C bus at `0x3C`. The README, wiring sketch and device specification follow the current source. The original shared bus is recorded in the [intake packet](../nfc-reader-intake/implementation.md).
- The firmware implementation record confirms both target builds with Jewel-only back-to-back polling and 250 ms removal debounce. The C3 OLED build also passed. The most recent full C3 bench sequence, OLED visual confirmation, electrical measurements and measured scan timing remain pending.

## ESP32-S2 Cucumber reference

The Cucumber HSU capture showed a valid outbound version query and no observed reader ACK. The I²C comparison subsequently returned firmware 1.6 (IC `0x32`) with tag-poll timeouts in the first build. Later firmware experiments acquired a full, checksum-valid tag frame and moved polling responses to length-aware reads. The firmware owner's implementation record preserves the earlier Type A captures and the transition to Jewel. The [Cucumber reference](../../../../projects/nfc-reader/README.md#esp32-s2-cucumber-v11-reference) preserves its I²C and HSU wiring.
