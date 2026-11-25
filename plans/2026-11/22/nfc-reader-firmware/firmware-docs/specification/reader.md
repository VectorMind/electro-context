# NFC reader firmware specification

The primary `esp32c3` target reads Jewel/Topaz tags through the PN532 at 100 kHz and 7-bit address `0x24`. The reader uses hardware I²C on GPIO0 SDA and GPIO1 SCL with its own 3.3 V pull-ups. The onboard 72×40 OLED uses software I²C on GPIO5 SDA and GPIO6 SCL. Startup reports the PN532 address and version, scan settings and removal debounce on native USB serial at 115200 baud.

The built-in default is Jewel only with back-to-back scans and 250 ms removal debounce. Print `Jewel tag UID:` with all four bytes in order and preserve leading zeros. Show the eight uppercase hex digits on the OLED in two rows. Return to `SCAN` / `TAG` after removal. A valid empty scan starts the debounce and a later valid empty scan confirms removal. An exchange error or a valid tag read clears the pending timer. Type A and mixed scan modes remain available in the built-in configuration.

The owner confirmed Topaz detection. The current back-to-back defaults and C3 OLED path have build evidence. Capture display alignment, hold suppression, removal and re-presentation, and actual scan timing on the current C3 image.

## ESP32-S2 Cucumber reference

The `cucumber_s2_i2c` environment runs the same reader logic on GPIO16 SDA and GPIO17 SCL with the DEBUG UART0 console and serial-only tag output.
