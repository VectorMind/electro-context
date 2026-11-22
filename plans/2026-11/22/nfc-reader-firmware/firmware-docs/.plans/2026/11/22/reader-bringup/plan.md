# Reader bring-up plan

## Problem and intended result

The assembled reader shares a two-wire bus with the onboard OLED. Establish in order that both devices acknowledge, the reader answers a command, and a newly presented card produces a complete visible UID. Follow the [firmware contract](../../../../../specification/reader.md) and the hardware references in [WORKFLOW.md](../../../../../WORKFLOW.md).

## Design decisions

| Choice | Proposal and rationale | Confidence | Status |
| --- | --- | --- | --- |
| Build baseline | Adopt the nfc_audio PlatformIO baseline: environment esp32c3, platform espressif32, board esp32-c3-devkitm-1, Arduino framework, serial monitor 115200. Confirm the build and board behavior separately. | High | closed |
| Bus and wiring | Use the pin map, bus speed and addresses in the firmware specification. The harness provides four conductors. | High | closed |

## Open points

| Choice | Proposal and verification | Confidence | Status |
| --- | --- | --- | --- |
| PN532 driver | Evaluate the polling transport linked from the Electro device spec. Verify that the chosen version works without IRQ/reset, uses the shared bus and bounds readiness waits. The prototype's 0xFF pin arguments are insufficient evidence. Record the selected dependency and version. | Medium | open |
| OLED driver and geometry | Check the controller, visible window and offsets against the Electro board reference. Select a shared-bus driver and confirm that a complete UID fits on the actual display, wrapping if needed. | Medium | open |

## Implementation and acceptance sequence

1. When implementation is requested, inspect the prototype configuration and pin definitions. Configure the C3 target, select the drivers, build, and record the exact environment and versions.
2. Before powered tests, obtain the Electro bench record for continuity, suitable PN532 VCC and 3.3 V-compatible SDA/SCL levels. Keep hardware validation pending until these checks are recorded.
3. Check the OLED alone as described in the device checklist. On the assembled bus capture a serial scan including 0x3C and 0x24. Capture the returned PN532 firmware version separately.
4. Present the supplied card, record its full UID and check the OLED against the returned bytes. Remove and present it again. Capture a fresh read and confirm the same UID appears without reset.
5. Return the build result, serial evidence, display observation and outstanding checks to the Electro packet. Mark each unrun check pending.
