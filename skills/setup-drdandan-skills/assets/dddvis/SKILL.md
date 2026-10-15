---
name: dddvis
description: "Generate or publish selected static reports through the project's preview service when the user explicitly invokes DDDvis. Ordinary HTML or visualize requests do not publish."
---

# DDDvis

Use this skill for an explicit `DDDvis` website-display request. Discussing or configuring DDDvis is not a publication request. Follow-up corrections to the same authorized report do not require the trigger again.

1. Read `docs/agents/dddvis.md` from the target repository root, then its bound service configuration. These define the project identity, actual publishing interface, current-entry record and recovery controls. If unbound or missing, report what must be configured; do not create a tunnel as a publishing fallback.
2. Identify the selected report from the request or current task. Generate HTML from existing results when needed, preserving the project's artifact placement and provenance. If the required experimental results are absent, name the missing input rather than running a new experiment. Ask only when the report cannot be identified from available context.
3. Prepare a self-contained HTML or an explicit static report directory with all required resources. Use the service's supported publication operation, preserve relative resources and prior usable output, and retain the same report path on updates. Distinct projects/reports remain separate. Report unsupported capabilities rather than publishing an incomplete bundle.
4. Read the program-owned publication and current-entry records. Check that the selected report/resources and authentication were verified and that the public-entry evidence is current. Use the bound service's bounded verification operation if evidence needs refreshing; observe any background work through its durable records and the project's long-running-job workflow.
5. Return the exact outcome defined in the project document: accessible, copied but temporarily unavailable, or not published. Include a current report link only with availability evidence. Background recovery remains owned by the service; preserve manual stop intent, report content and credentials, and do not improvise a restart loop in the chat.

Completion means a verified accessible report, or a precise failure/unavailable result with retained artifacts and records. A copied file, running process or rewritten URL alone is not successful delivery.
