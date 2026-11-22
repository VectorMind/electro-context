# Electro agent entry point

For a device chat open `projects/<device>/`. Read this file, [WORKFLOW.md](WORKFLOW.md), the device's AGENTS.md, project.yaml and spec.md. Find current work in [plans/open.md](plans/open.md). For the first NFC reader session use the intake packet listed there.

This repo owns shared electronics tooling, device specifications, editable PCB sources, and decisions that cross firmware, electrical and mechanical boundaries. Firmware, CAD and website repositories own their implementations. Read sibling sources for context and give the owning agent a scoped handoff when changes are needed.

Resolve machine paths using [config/README.md](config/README.md). Keep original evidence and fabrication assets in the device's Drive home. Put downloads, extracted text and exploratory scripts in the device's ignored `.cache/`. A fresh checkout plus the referenced durable assets must be sufficient to repeat a recorded build.

Leave Git history changes, flashing and publication to tasks that explicitly call for them.
