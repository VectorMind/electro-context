# Handoff to the NFC reader intake agent

The intake is closed. Continue with the [firmware handoff](../nfc-reader-firmware/handoff.md).

Open projects/nfc-reader/ in electro-context. Read its AGENTS.md, project.yaml and spec.md, then [the intake plan](plan.md).

First resolve board_reference and read c3-oled42.md with its linked sources. Check the device Drive folder for owner-supplied material. If the folder is missing, record that fact and continue with available sources. Record a short source inventory in the packet with repo-relative paths or Drive-relative filenames and what each supports.

Update spec.md with confirmed inputs, intended device behavior where stated, and unresolved questions. For the Elechouse module, ask for the exact unit/revision or photos and documentation if needed. Leave bus, GPIOs, supply details, tag behavior and dimensions open wherever evidence is missing. Use the thermostat article only for publication conventions and robot_serial_display only for its firmware workflow.

Return a summary of changed files, evidence, questions for the owner and the next independently actionable workspace tasks. Keep edits within the Electro docs for this intake. Firmware implementation, PCB layout, enclosure models and publication belong to later packets.
