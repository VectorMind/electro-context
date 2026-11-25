# NFC reader firmware bring-up

**Owner** ESP32 firmware workspace. **State** Open for final C3 bench verification.

The owner selected WhizTags Topaz 512 and confirmed Jewel detection. The default `esp32c3` build uses the PN532 on hardware I²C with separate software I²C for the OLED. The current Jewel-only configuration starts scans back-to-back and debounces removal for 250 ms. The [device specification](../../../../projects/nfc-reader/spec.md#selected-interface) owns the electrical interface. The [implementation record](implementation.md) separates build results and physical observations.

## Remaining bench checks

1. Verify the C3's GPIO0/GPIO1 reader harness, common ground, PN532 supply, and 3.3 V pull-ups under power. Set switch 1 ON and switch 2 OFF before powering the breakout.
2. Confirm the C3 starts in Jewel-only mode and reports the PN532 firmware version. Present the Topaz 512 and compare all eight OLED digits with the reported UID.
3. Verify one arrival event while the tag is held, removal after the debounce, and re-presentation without reset. Measure scan cadence or tap-to-UID latency on the current image.
4. Record the observed hardware results and build identity before closing the packet.

Use the [firmware handoff](handoff.md) to resolve the checkout and continue the bench work.

## ESP32-S2 Cucumber reference

The Cucumber was used to isolate the earlier C3 transport issues. Its HSU query produced no observed reader ACK. I²C then returned the reader firmware version and supported the full-frame response investigation. The shared firmware retains the serial-only `cucumber_s2_i2c` build for comparison. See the [Cucumber wiring](../../../../projects/nfc-reader/README.md#esp32-s2-cucumber-v11-reference) and [historical troubleshooting handoff](firmware-docs/.plans/2026/11/22/reader-bringup/handoff-cucumber-i2c-troubleshooting.md).
