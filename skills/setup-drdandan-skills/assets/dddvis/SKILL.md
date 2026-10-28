---
name: dddvis
description: "Generate or publish selected static reports through the project's bound report portal when the user explicitly invokes DDDvis. Ordinary HTML or visualize requests do not publish."
---

# DDDvis

Use this skill for an explicit `DDDvis` website-display request. Discussing or configuring DDDvis is not a publication request. Follow-up corrections to the same authorized report do not require the trigger again.

1. Read `docs/agents/dddvis.md` from the target repository root, then its bound service configuration. Resolve the project identity, access model, publisher, current-entry record and lifecycle controls there. If unbound or missing, report the configuration needed; provisioning a service is a separate task.
2. Identify the selected report from the request or current task. Generate HTML from existing results when needed, preserving the project's artifact placement and provenance. If required results are absent, report the missing input rather than starting a new computation. Ask only when the report cannot be identified from available context.
3. Resolve the selected report's source directory, entry file and required resources/downloads. Publish a self-contained HTML file or complete static export directory through the supported publisher, following the project's display-copy and access rules. Preserve relative paths, prior usable output and the report's identity on updates. Report unsupported capabilities rather than publishing an incomplete bundle.
4. Read the program-owned publication and current-entry records. Check that authentication, report interactions and required resources were verified through the configured LAN entry and that the access evidence is current. Honor a different access model only when explicitly configured for that project. Use the service's bounded verifier when evidence needs refreshing; observe background work through its durable records and the project's long-running-job workflow.
5. Return the exact outcome defined in the project document: accessible, copied but temporarily unavailable, or not published. Include a current report link only with availability evidence. Background recovery remains owned by the service; preserve manual stop intent, report content and credentials, and do not improvise a restart loop in the chat.

Completion means a verified accessible report, or a precise failure/unavailable result with retained artifacts and records. A copied file, running process or rewritten URL alone is not successful delivery.
