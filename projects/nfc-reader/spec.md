# NFC reader device specification

## Stated scope

Bring up an NFC reader using an ESP32-C3 and a PN532 breakout. The first bench milestone is to communicate with the PN532 and read a tag UID. Audio belongs to the earlier prototype.

## Starting hardware

| Item | Evidence | Still to verify |
| --- | --- | --- |
| ESP32-C3 development board with 0.42-inch OLED | Owner identified the blue diymore board as the likely unit. The [retailer listing](https://www.amazon.de/-/en/dp/B0GQL8YVPF) is the purchase reference. The existing Home Smart Mesh board page has a photo and a schematic. | Match the actual unit to the board reference. |
| PN532 NFC Module V3 breakout | Owner supplied the [Binghe listing](https://www.amazon.de/-/en/dp/B0D86CPN5J) for the red module, sold with an S50 card. The [Elechouse V3 guide](https://www.elechouse.com/elechouse/images/product/PN532_module_V3/PN532_%20Manual_V3.pdf) is a reference for this layout. | Match the PCB markings, switch legend and power circuit to the actual module. |

## Candidate interface

The earlier `nfc_audio` prototype configures SDA on GPIO5 and SCL on GPIO6. The [ESP32-C3 OLED board schematic](https://homesmartmesh.github.io/microcontrollers/esp32/c3-oled42/) shows the OLED wired to those pins with 4.7 kΩ pull-ups to 3.3 V. The board photo and schematic place GPIO5 and GPIO6 on the opposite edge from 5V and GND. The schematic also shows that the 5V header comes from USB VBUS. Check the actual board against the schematic before using it.

For the first bench test, use the PN532's four-pin header. In the supplied photo, its labels read GND, VCC, SDA, SCL from top to bottom. ESP32 GND goes to GND, GPIO5 to SDA and GPIO6 to SCL. Use ESP32 5V for VCC only after checking that the actual breakout accepts that supply and that its header-side SDA/SCL idle at 3.3 V with suitable pull-ups. With the ESP32 signal wires disconnected, power the reader at the verified supply and measure both lines against GND. If either idles near 5 V, use a bidirectional I²C level shifter before connecting it. If the lines float, establish the pull-ups and recheck the idle voltage.

Expected 7-bit addresses are `0x3C` for the OLED and `0x24` for the PN532. An [I²C polling transport](https://github.com/elechouse/PN532/blob/PN532_HSU/PN532_I2C/PN532_I2C.cpp) reads the PN532's status byte and is a candidate for using the four-pin header alone. The prototype passed `0xFF` for IRQ and reset to the [Adafruit PN532 I²C constructor](https://github.com/adafruit/Adafruit-PN532/blob/master/Adafruit_PN532.cpp), which configures both supplied GPIOs. Verify the chosen driver with a firmware-version query followed by UID reads.

If a single ESP32 header edge becomes a mechanical requirement, GPIO0 for SDA and GPIO1 for SCL are candidates on the power edge. The onboard OLED stays wired to GPIO5/GPIO6. Keeping both devices active with this wiring requires using a separate software I²C bus alongside the ESP32-C3's single hardware I²C controller, and confirming library compatibility and pull-ups for the separate bus. Keep GPIO2 available for boot strapping and RX/TX available for UART0.

## Enclosure and harness concept

Keep both PCBs face up, with the ESP32 on the left, USB-C toward the near wall, and the PN532 on the right with its four-pin header facing inward. Solder the harness with both boards out of the enclosure. Route the SDA/SCL pair along the underside of the ESP32 to the facing edge. Route all four insulated wires through the gap, with independent power and signal leads to accommodate the header order. Leave a small service loop and strain-relieve the wires to the base.

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

## Hardware decisions and bench checks

1. Check the reader markings and verify the module supply requirements and where the I²C pull-ups are tied. Confirm 3.3 V bus levels and adequate supply headroom before attaching the module to the ESP32-C3.
2. Check the OLED by itself, then add the PN532 in I²C mode with power off during rewiring. Verify that the OLED and PN532 coexist and that a PN532 firmware-version command succeeds.
3. Read the supplied card UID repeatedly and check that the reader detects it again after removal and re-presentation.
4. Use the interface and power results to decide wiring and connectors. Measure the module’s mounting points and read range through the intended face material before setting enclosure dimensions.

The shared bus, final supply, supported tags, connector and PCB layout remain open. See the [project README](README.md) for the illustrated hardware overview and the [intake record](../../plans/2026-11/22/nfc-reader-intake/implementation.md) for the evidence.
