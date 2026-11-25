# Firmware development workflow

## Start a change

Read the [Electro manifest](../../VectorMind/electro-context/projects/nfc-reader/project.yaml), [device specification](../../VectorMind/electro-context/projects/nfc-reader/spec.md), [current wiring and bench checklist](../../VectorMind/electro-context/projects/nfc-reader/README.md) and the [firmware handoff](../../VectorMind/electro-context/plans/2026-11/22/nfc-reader-firmware/handoff.md). These links assume the sibling checkout layout under the same dev root. For another host, resolve the manifest paths using [Electro host configuration](../../VectorMind/electro-context/config/README.md).

Use [the firmware specification](specification/reader.md) for observable behavior. If a proposed change needs a new GPIO, a different bus or a harness modification, send the proposal to the Electro owner before treating it as a firmware input.

## Plan, implement and verify

Before a substantial change, create a local packet at `.plans/YYYY/MM/DD/<topic>/plan.md`. Link the affected specification, describe the intended behavior, record consequential decisions as open or closed, and define observable exit checks. Update the durable spec when an agreement changes.

When implementation begins, add `implementation.md` to the same packet. Record the actual PlatformIO environment, board, framework, dependency versions, build command and result. Track bench validation separately, including missing measurements or failed observations. Return results to Electro in `handoff-back.md` with links to the spec, implementation record and bench evidence.
