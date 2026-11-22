# Specification-driven electronics work

## Intake and planning

Start from the project manifest and the open plan index. Follow the active handoff before exploring sibling repos. A dated plan describes a bounded change. The device spec holds accepted requirements and interfaces across changes. Record an observation with its source and hardware revision where known. Mark assumptions and missing evidence explicitly.

For substantial work, open a packet following [the plans guide](plans/README.md). Describe the problem, intended result, inputs, consequential open decisions and exit checks in plan.md. A decision marked closed is settled. Track implementation and bench verification independently. If a missing measurement blocks enclosure design, continue the independent firmware or documentation work covered by the plan.

## Ownership and handoffs

| Owner | Deliverable |
| --- | --- |
| Electro device project | Shared interfaces, acceptance criteria, editable PCB sources under pcb/ when available, integration and build records |
| ESP32 firmware repo | Firmware contracts, code, dependency pins and build instructions. Use robot_serial_display as the SDD example. |
| cad-context and its designated model project | CAD workflow and enclosure model. Follow cad-context's project paths, cache and publication conventions. |
| Device Drive folder | Datasheet snapshots, original measurements/photos, bench captures and named fabrication or print artifacts |
| HomeSmartMesh website | Public article and reader-accessible assets following the website's content conventions |

Assign one owner per edit. A handoff names the target workspace, relevant spec sections, inputs, allowed changes, unresolved dependencies and evidence to return. An external agent reads its own AGENTS.md and WORKFLOW.md and uses its native plan format. The firmware example uses .plans/YYYY/MM/DD/<topic>/ and the website uses plans/YYYY-MM/DD/<topic>/. Link packets across repos when available.

## Integration and closure

Record actual edits and verification in implementation.md once work has begun. Mark measured, documented and unverified claims distinctly. Transfer accepted cross-component decisions into the device spec in the same change. Move the packet from the open index to the closed index when its exit checks are met. Preserve any pending physical checks explicitly.

For a built device, add projects/<device>/builds/<id>.md with board revisions, PCB source revision, firmware commit and toolchain, enclosure source version, exact print/fabrication assets and bench evidence. Include a checksum for mutable binary inputs. Website documentation can then refer to the specific build it describes.
