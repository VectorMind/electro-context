# Reader bring-up plan

The C3 is the primary Jewel/Topaz reader. Use the [firmware specification](../../../../../specification/reader.md) for the separate PN532 and OLED I²C buses and the current scan defaults.

1. Build `esp32c3` and record the exact image. Verify reader power, common ground, continuity and the dedicated bus pull-ups before powered testing.
2. Confirm the version query and the Jewel-only startup banner. Present the Topaz 512 and compare the full serial UID with the two OLED rows.
3. Hold, remove and re-present the tag without rebooting. Record both UID events, the single removal event, and measured scan timing.

## ESP32-S2 Cucumber reference

The same firmware can build as `cucumber_s2_i2c` for DEBUG serial comparison. See the earlier Cucumber handoffs in this packet for its bench history.
