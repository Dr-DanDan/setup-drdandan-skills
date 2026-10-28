# DDDvis report portal

`DDDvis` is the explicit conversation command for generating or publishing a selected report to the project's bound portal. Ordinary HTML generation and `visualize` do not publish. Follow-up corrections to the same authorized report retain that authorization.

## Project binding

```json
{
  "project_id": null,
  "service_config": null
}
```

Setup fills `project_id` from the service's project registry and `service_config` with the stable absolute path to its configuration or runbook. Preserve existing bindings and confirmed access choices. For new bindings, use a password-protected shared LAN portal. Two null fields mean deliberately unbound, not a working deployment.

The bound configuration supplies the publisher, project registry, access model, current-entry and status records, and stop/resume controls. Resolve deployment addresses, ports and paths there. Report source directories, entry files and resource sets come from the selected report. These values belong to each deployment and project, not to the reusable skill template.

Credentials and runtime records remain in private service storage, outside the repository and published directory. A project registration labels its reports; it does not authorize publication of every file under the project root.

## Report publication

- Generate a report from available results, or publish the selected existing report. Missing results are a dependency; publication does not start an experiment or another computation.
- Publish a display copy only on an explicit DDDvis request. Later source changes do not alter that copy; an explicit update replaces the current version at the same report path.
- Support a self-contained HTML file or an explicit static export directory with a declared entry file. Include the required scripts, styles, images, display data and downloadable files, preserving relative paths and existing interactions. Serve required resources through the portal so viewing does not depend on external assets.
- Preserve source artifacts and their provenance. Export only the selected report and its resources; repository roots, source-data roots and credential directories are not publication roots.
- Organize reports by project and the project's report group, such as an experiment when one exists. Preserve existing navigation and report identities. Same-named reports in different projects must not overwrite one another.
- Keep the current published copy rather than an extra history archive. Failed publication retains the previous usable copy and does not expose staging files or overwrite concurrent publishers' entries.
- Apply the service's password protection to the homepage, reports, static resources and downloads. Reuse existing login information; publishing a report does not rotate credentials.

Use the service's supported publisher. If directory publication or another required capability is missing, report the gap rather than omit resources or claim completion. Applications that require a persistent backend need a separate hosting workflow.

## Current entry and delivery

The service owns a current-entry record containing the configured base URL, availability and check time. For the default LAN portal, this is its LAN address. For each delivery, read the record and combine the verified base URL with the report path. An active process or local health endpoint alone does not establish a usable report.

Acceptance checks cover the browser entry, navigation, report interactions and required resources/downloads over the configured address. For the default password-protected portal, anonymous and incorrect-password requests must not read protected content; authenticated requests must retrieve the required content completely. Keep access from the checked network distinct from unverified access elsewhere. Mark stale or failed access evidence as unavailable.

Report one of these outcomes:

- **Accessible:** the report and necessary resources are published, authentication and access checks passed, and the current report URL is verified.
- **Copied, temporarily unavailable:** the display copy exists, but entry or access checks failed; provide the status record without presenting the URL as working.
- **Not published:** preparation or publication failed; state the cause and retain the original and previous usable copy.

When the service address changes, verify the new entry before delivery. Existing bookmarks do not update automatically; later DDDvis requests read the current address from the service record.

## Background service and migration

Setup records the binding, requirements and actual capabilities. It does not install, enable, repair or stop runtime services. A separately provisioned portal runs independently of Codex and an open terminal, and writes durable progress and terminal-status records. Codex observes these records read-only.

- Scope configured recovery to this portal's HTTP service. Use the service's documented check interval, retry limit and cooldown; preserve its reports, registry, paths and credentials. Report missing recovery capability accurately.
- Persist manual stop intent across background checks and machine restarts. Only an explicit resume clears that intent through the documented service controls.
- For an authorized migration from a public tunnel, retain the existing service until the LAN portal and migrated reports pass acceptance. Then retire the old tunnel and its public-recovery service so they cannot recreate public entries. Preserve report identities and contents throughout the migration.

Record requested capability changes or migration as pending until implemented and verified. Writing this contract does not establish service availability or automatic recovery.
