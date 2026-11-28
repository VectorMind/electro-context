# NFC reader enclosure fit

**Owner** Electro for measurement intake and shared decisions. The external NFC reader model project owns the CAD source.

The first print should accept the wired ESP32-C3 and PN532, keep the OLED visible, accept the selected USB cable, and allow Topaz reads through the lid. The enclosure uses two shells with one open notch in each mating edge for USB-C. The lid covers BOOT and RESET.

1. Measurement intake is complete. The owner confirmed that the harness allows the front PCB edges to align at the selected 12 mm gap (D9 = 0). E1 and E2 record the original support observations, with 3 mm side ledges chosen for the first fit. Keep original bench evidence in the device Drive folder. [Enclosure drawings and measurement record](../../../../projects/nfc-reader/enclosure.md)
2. The external model project is initialized. Its base and board-envelope preview use the measured layout and passed CAD verification. Review the base render before adding the lid. The model project owns the lid fastening, fit tolerances, and printable geometry. [Base render](../../../../projects/nfc-reader/enclosure.md#cad-iteration-1--base) · [Device spec](../../../../projects/nfc-reader/spec.md#enclosure-and-harness-concept)
3. Record the fit print results. Both boards should seat without loading the harness or underside parts, the cable should seat fully, the display should be readable, and the tag should read through the tap face.
