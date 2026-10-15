# DDDvis website reports

`DDDvis` is the explicit conversation command for generating or publishing a selected report to this project's shared preview service. Ordinary HTML generation and `visualize` do not publish. Follow-up corrections to the same authorized publication retain that authorization.

## Project binding

```json
{
  "project_id": null,
  "service_config": null
}
```

Setup fills `project_id` with the identifier used by the existing service's project registry and `service_config` with the stable absolute path to its local configuration or runbook. Preserve an existing binding; two null fields mean deliberately unbound, not a working deployment.

The service configuration supplies its actual publishing interface, project registry, current-entry/status record locations, and stop/resume controls. Resolve these from that file rather than inventing command flags or inferring a URL from old logs. Credentials and runtime records remain in private service storage, outside the repository and published directory. A project binding is not permission to publish every file under its root.

## Report publication

- Generate HTML from results already available in the task, or publish a specified existing report. If required experiment results are missing, report the dependency; publication does not start a new experiment.
- Prefer self-contained HTML for new reports. Existing static report directories may include their entry HTML and required JS, CSS, images and display data. Use an explicit export directory, preserving relative paths; applications requiring a persistent backend are outside this workflow.
- Preserve original project artifacts and their provenance. Publish only the selected report and its resources; repository roots, source-data roots and credential directories are not website roots.
- Keep a report's path when updating its website copy. Add distinct reports under their own project/experiment; reports with the same name in different projects must not overwrite one another. Preserve existing report links and concurrent publishers' directory entries.
- The website holds the current published copy, not an additional history archive. Failed publication preserves the previous usable copy and does not serve staging files.

Use the bound service's supported operations. When its publisher lacks directory support or another required capability, report the exact missing capability rather than dropping resources or claiming the contract is implemented.

## Current entry and delivery

The service owns one current-entry record with the public base URL, availability and check time. Read it for each delivery and combine the verified current base URL with the report path. A local process or readiness endpoint alone does not prove public availability. Mark stale or failed public checks as unavailable instead of presenting an old URL as working.

Report one of these outcomes:

- **Accessible:** publication and necessary resources are ready, authentication/access checks passed, and the current report link is available.
- **Copied, temporarily unavailable:** the website copy exists, but the public entry failed its check or is recovering; give the status/record location without claiming accessibility.
- **Not published:** generation, resource preparation or copying failed; state the cause and retain the original and previous usable copy.

After tunnel recreation, validate the new entry before marking it available. Historical chat links and browser bookmarks do not change or redirect automatically. Users obtain the new URL through a later `DDDvis` request or status inquiry; this workflow does not add email, chat push or bookmark synchronization.

## Shared recovery service contract

Setup records this contract but does not install, enable or repair the runtime. A separately configured recovery program must run independently of Codex, stdin and an open terminal, and write durable checks, actions, current-entry and terminal-status records. Codex observes those records read-only; the recovery program has narrowly scoped control over only this website's HTTP service and Tunnel.

- Check every 60 seconds; require 3 consecutive abnormal results before recovery.
- Distinguish the local HTTP service from tunnel/public-entry faults. Restore the affected component; recreate a Quick Tunnel only when the tunnel cannot otherwise recover. A normal authentication challenge is not an outage, and bypassed authentication is not healthy.
- Attempt at most 2 repairs per round. If still unsuccessful, wait 600 seconds before checking again. Network/provider outages enter this waiting path without changing system networking, firewall rules or unrelated services.
- Preserve report contents, paths, registry and password. Update the single current-entry record after the new authenticated entry is verified; project bindings do not need hostname rewrites.
- Persist manual stop intent. Background checks, cooldown completion and machine restarts do not undo it; only an explicit resume does. The service's documented stop/resume operations must maintain this intent.

Keep recovery capability/status distinct from publishing capability. If the bound service lacks a watchdog or current-entry record, report that gap; neither process restart settings nor this document prove automatic recovery. Server-side public checks also do not prove reachability from the user's own network.
