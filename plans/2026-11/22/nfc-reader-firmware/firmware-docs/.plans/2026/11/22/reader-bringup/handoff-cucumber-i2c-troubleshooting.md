# Troubleshoot the Cucumber PN532 I²C tag poll

Continue in `C:/dev/ESP32Home/nfc-reader` with the existing `cucumber_s2_i2c` environment. Read the firmware repo's AGENTS.md, WORKFLOW.md and this packet's plan. The [Electro bench procedure](../../../../../../../VectorMind/electro-context/projects/nfc-reader/README.md#i²c-comparison-on-the-cucumber) records the four-wire hookup and electrical checks. The owner is taking over the firmware investigation from this handoff.

## Observed baseline

- ESP32-S2 Cucumber v1.1, PN532 in I²C mode, GPIO16 SDA, GPIO17 SCL, 100 kHz, address `0x24` and DEBUG UART0 at 115200 baud.
- All address probes shown returned ACK. Five version exchanges returned PN532 firmware 1.6 and IC `0x32`. Each successful startup reached tag polling, which means the sketch accepted the SAM and RF configuration responses.
- All five `InListPassiveTarget` calls reached the response stage and timed out with `-2` after accepting the command ACK. No UID or valid zero-target response was observed. The next version attempt in each cycle reported an invalid command ACK (`-1`). The earlier `Wire.requestFrom()` errors include reads during exchanges that eventually succeeded.
- The current code uses one passive activation attempt, a 150 ms response deadline, a 25 ms Wire transaction timeout and a 2 s recovery delay. The host timeout does not cancel the reader's command.

## First experiment

1. Instrument one `InListPassiveTarget` exchange with the existing timings and configuration. In [the exchange code](../../../../../src/cucumber_s2.cpp), record command write status and whether the PN532 ACK matches. In [the I²C transport](../../../../../src/pn532_i2c_diagnostic.h), distinguish errors during response-prefix read, resend request and full-frame reread. Capture elapsed time since the command, returned byte counts, ready-bit transitions and the first relevant frame bytes. Buffer or summarize observations so serial output inside the 2 ms polling loop does not dominate the timing.
2. Capture the first poll after a full power cycle with no card in range. Repeat with the S50 held flat against the antenna before power-up. Save the console log and, if available, an I²C capture at the reader header (SCL and SDA referenced to PN532 GND). The expected response body starts with `D5 4B 00` with no tag or `D5 4B 01` followed by target data and UID with the S50. [NXP Type A polling](https://www.nxp.com/docs/en/user-guide/141520.pdf#page=116)
3. If reads return no status, check whether the reader NACKs the read address, a line is held low, or VCC dips when RF polling starts. Verify 3.3 V pull-ups and idle levels on SDA/SCL at the reader. If status reads work but RDY stays clear, investigate command completion and the RF phase. If a valid prefix arrives but the reread fails, focus on the transport's partial read, STOP and resend sequence. NXP requires reading the rest of a ready frame before STOP, and the current transport deliberately stops after the prefix and requests retransmission. [NXP I²C transfer rules](https://www.nxp.com/docs/en/user-guide/141520.pdf#page=43)

## Recovery experiment and exit checks

Once the original timeout is captured, try NXP's command abort after a reproduced timeout and before issuing a fresh version query. Record the abort write result and the next command ACK and version response. A recovery improvement would explain the follow-on failure while the first poll may still time out. [NXP command abort](https://www.nxp.com/docs/en/user-guide/141520.pdf#page=37)

Return the exact build target, first failed stage, elapsed time and evidence for both cold starts. The bench exit checks are repeatable zero-target replies with an empty field, a full S50 UID on presentation and again after removal, and a known-good command exchange after a forced timeout and recovery. Keep upload and bench actions under the owner's control.
