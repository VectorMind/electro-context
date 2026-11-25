# NFC reader device specification

## Scope and hardware

The primary reader combines the diymore ESP32-C3 with a 0.42-inch OLED, a PN532 V3 breakout and **Jewel/Topaz** tags. The owner selected Jewel after confirming detection of a WhizTags Topaz 512. The original kit's S50 belongs to the earlier Type A comparison. [Hardware references and wiring](README.md#hardware)

## Selected interface

| Interface | Assignment |
| --- | --- |
| PN532 hardware I²C | GPIO0 SDA, GPIO1 SCL, 100 kHz, 7-bit address `0x24` |
| Onboard OLED software I²C | GPIO5 SDA, GPIO6 SCL, 7-bit address `0x3C`. The board schematic shows 4.7 kΩ pull-ups to 3.3 V on this bus. |
| PN532 mode and harness | Switch 1 ON, switch 2 OFF (1/0), sampled at power-up. Four wires for GND, VCC, SDA and SCL. The firmware polls readiness over I²C. |
| Serial console | C3 native USB at 115200 baud |

Use separate 3.3 V-compatible pull-ups on the PN532 bus. Establish the reader supply and idle signal levels on the actual breakout before connecting the C3. Use a bidirectional level shifter if the reader side pulls SDA/SCL to 5 V. The C3 5V header comes from USB VBUS and is a supply option after those checks. [Bench procedure](README.md#bench-checks) · [C3 OLED schematic](https://homesmartmesh.github.io/microcontrollers/esp32/c3-oled42/)

## Tag and display behavior

- The firmware defaults to `Jewel/Topaz only` and starts the next scan as soon as the previous one completes. Actual scan cadence depends on the response and recovery path.
- A valid Jewel response yields a four-byte identifier. Report it on first detection in byte order with leading zeros. The OLED shows eight uppercase hex digits split into two rows of four. Show `SCAN` / `TAG` at startup and after confirmed removal.
- A valid empty scan starts the **250 ms removal debounce**. Confirm removal on a later valid empty scan at or after the threshold and rearm the same ID. A valid tag read or transport error clears the pending removal. An I²C error does not count as an empty field.
- Firmware also offers Type A and mixed scan modes through its built-in settings. Jewel only is the selected device default.

## Enclosure and harness concept

Keep both PCBs face up, with the ESP32 on the left, USB-C toward the near wall, and the PN532 on the right with its four-pin header facing inward. Solder the harness with both boards out of the enclosure. Route all four insulated reader leads from the facing C3 edge through the gap. Keep power and signal leads identifiable at both ends. Leave a small service loop and strain-relieve the wires to the base.

| Feature | First prototype allowance |
| --- | --- |
| Gap between PCB edges, direct solder | Start at 8–10 mm. Try 6 mm only after fitting the insulated bends on a mockup. Solder the boards outside the case. |
| Gap with opposing right-angle headers and female plugs | Measure each plug installed, from PCB edge to the start of the flexible wire. Add both projections and 5–8 mm for the bends. Begin the plug-in test with the boards loose. |
| Space under boards | Measure from PCB underside to the lowest solder tail, plug or wire bend and add 1–2 mm. Give the harness its own underside channel. |
| ESP32 retention | Use two short edge ledges or clips and an end stop. Support the unsoldered edge at clear spots and relieve the clip around the soldered edge and USB port. Leave room for the actual USB plug and its overmold. |
| PN532 retention | Use the two diagonal mounting holes on plastic posts. Start with nylon fasteners and route the harness away from the coil perimeter. Keep nearby metal and the ESP32 PCB out of the area directly above or below the coil. |
| NFC lid | Put a roughly 1.0–1.2 mm unfilled plastic test panel over the coil. Set the PN532 post height from the tallest component under the cover, plus 0.5–1 mm air clearance. Test the actual tag through this panel before fixing the lid thickness or module orientation. |
| Display and buttons | Give the OLED its own lid cutout and keep the lid off the glass. Start with open access to BOOT and RESET. Reserve room for a later flexible insert or printed plungers. |

Measure PCB outlines, hole centers and diameters, maximum component and underside heights, USB-C plug clearance, and the OLED and button centers on the actual hardware. The allowances above are mockup dimensions pending fit and read tests.

## Bench verification

The owner reports working Jewel detection. The firmware implementation record confirms builds for the current C3 OLED and shared back-to-back Jewel configuration. Complete the C3 power and bus-level checks, compare the on-panel digits with the serial UID, and record hold, removal, re-presentation and scan latency on the current image. Measure the module's mounting points and read range through the intended face material before setting enclosure dimensions. Supply measurements, final connector and PCB layout remain open.

## ESP32-S2 Cucumber reference

The optional `cucumber_s2_i2c` target uses the same reader logic with GPIO16 SDA and GPIO17 SCL at 100 kHz and address `0x24`. Output is on DEBUG UART0 at 115200 baud. The earlier HSU comparison used GPIO17 TX, GPIO16 RX and switch setting 0/0. Its capture showed an outbound version query and no observed reader ACK. See the [Cucumber reference](README.md#esp32-s2-cucumber-v11-reference).
