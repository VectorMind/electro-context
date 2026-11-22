# NFC reader intake

## Evidence

| Source | What it supports |
| --- | --- |
| Owner’s product links and screenshots | Likely blue diymore OLED board and Binghe PN532 V3 kit with an S50 card. Amazon listing pages could not be retrieved independently. |
| Home Smart Mesh `content/microcontrollers/esp32/c3-oled42.md` and its schematic | Existing diymore board reference and assets for the public page. |
| ESP32Home `nfc_audio/README.md`, `src/pins.h`, `src/main.cpp` | Shared I²C candidate on GPIO5/GPIO6, driver setup and attempted ISO 14443A UID polling. Source review establishes how it was configured. Bench evidence is pending. |
| Espressif ESP32-C3 datasheet, NXP PN532 manual and Elechouse V3 module guide | Chip specifications, PN532 host modes and address notation, and the reference module’s switch settings. |
| Adafruit PN532 I²C constructor | Configures both IRQ and reset GPIOs. The prototype’s no-pin values need checking against the chosen firmware dependency. |
| Resolved Drive home `drive_projects/nfc-reader` | The folder was accessible. No files were present during intake. |

## Work completed

Updated the [device specification](../../../../projects/nfc-reader/spec.md). The prototype's `src/pins.h` and `Wire.begin` call support GPIO5 for SDA and GPIO6 for SCL as candidate wiring.

Moved the website draft into the [project README](../../../../projects/nfc-reader/README.md). It includes component-side reference photos from Home Smart Mesh and Cirkit Designer and a generated side-by-side concept sketch. The photos are reference images pending comparison with the actual boards.

Reviewed the owner's bench photo and the ESP32-C3 OLED schematic. The photo shows the PN532 four-pin header labels and the intended inward-facing headers. The schematic puts the OLED bus on GPIO5/GPIO6, on the opposite edge from USB 5V and GND. Recorded candidate wiring, a plug-in harness clearance method and a first enclosure layout in the [device specification](../../../../projects/nfc-reader/spec.md#enclosure-and-harness-concept). Supply, pull-ups, driver behavior, mechanical dimensions and tag read range remain bench checks.

The owner selected the shared hardware I²C bus on GPIO5/GPIO6 and set the PN532 switches to 1 ON and 2 OFF, matching the I²C row on the board. Front and rear photos show the soldered four-wire harness, with black for GND, red for 5V to VCC, blue for GPIO5 to SDA and yellow for GPIO6 to SCL. The [README wiring sketch](../../../../projects/nfc-reader/README.md#as-built-wiring) documents that routing. Continuity, signal levels and the first scan have yet to be recorded.

## Website handoff

Installation is pending. The session could read `C:\dev\HomeSmartMesh\homesmartmesh.github.io` but received access denied when creating the article directory. The ESP32 index, board page, board images and schematic matched Git HEAD and the index at the time of the check.

The validation copy used the earlier website draft at `content/microcontrollers/esp32/nfc-reader/readme.md` and added `- uid: microcontrollers.esp32.nfc-reader` immediately after `c3-oled42` in the ESP32 index's `cards` block. The first render paired several reference labels with the wrong URLs and left the pinout out of the static HTML. Replaced the hardware and pinout tables with lists and gave each reference its own list entry.

The revised draft passed a static build of the validation copy with the pinned `astro-huge-doc` revision `4005092c48eec8c8ad6ef8efc5ba2ff70a1ec988`, Node v24.11.1 and the existing pnpm dependencies copied into a writable staging directory. The site's image artifact check passed. The generated HTML placed the NFC Reader card after the OLED board card, reused its image, displayed GPIO5 and GPIO6, and paired each reference label with its intended URL. The OLED board page's photo and SVG schematic resolved to assets matching the source files. The external Elechouse reference schematic PDF opened.

If website work resumes, adapt the project README to the site's front matter and asset conventions, place the article and card in the target checkout, and verify the rendered result before updating `article.status`.

## Next hardware work

| Owner | Input | Expected output |
| --- | --- | --- |
| Electrical | Actual modules, product screenshots and V3 schematic | Identify board revisions, I²C pull-up rail, usable supply and available current. |
| Firmware | Confirmed module wiring and supplied card | OLED continues to respond alongside a successful PN532 firmware-version query and repeatable UID reads. Start from the NFC path in `nfc_audio`. |
| PCB and enclosure | Verified wiring, power budget, module dimensions and tag read-range tests | Connector and mounting layout with an antenna placement that works through the intended face material. |
