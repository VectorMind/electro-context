# NFC reader firmware bring-up

**Owner** ESP32 firmware workspace. **State** Open.

The local `ESP32Home/nfc-reader` repository has a README, license and ignore file. Bring up the soldered ESP32-C3 OLED and PN532 combination on the selected shared I²C bus and display the supplied tag's UID. The [device specification](../../../../projects/nfc-reader/spec.md#selected-interface) owns the wiring and electrical constraints.

**Exit checks** The firmware builds for ESP32-C3. After the electrical checks, verify that the OLED and PN532 respond on the shared bus, read the PN532 firmware version, and read and display the tag UID again after removing and re-presenting the tag. Report build and hardware results separately.

[Receiving agent instructions](handoff.md)
