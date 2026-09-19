# Scope limits seed

Use this seed for root `AGENTS.md`. Adapt the project-specific parts from
repository evidence before presenting the draft:

- Replace `<repository role and operator model>` with a short factual description,
  such as "a local research and training codebase with cooperating operators".
- Replace `<existing provenance contracts>` with concrete required records, or
  remove that sentence when the repository has none.
- Replace `<required project workflows>` with the relevant safety, migration,
  verification, review, experiment, or durable-job contracts already required by
  the repository. Keep the list short.

Never write the angle-bracket placeholders into `AGENTS.md`.

```markdown
## Scope limits

These limits bound proposed fixes and discretionary verification, never what to inspect or report. Report any defect reachable through documented commands, public interfaces, or real project data, even if it appears rare. Then keep the response proportional:

1. Treat this as <repository role and operator model> unless the task or a project document defines a different boundary.
2. Add hashes, checksums, or fingerprints only when required by an existing provenance contract, or when they replace a materially more expensive operation and their result changes the next action. Existing <existing provenance contracts> remain in scope.
3. Add no feature flags, migration frameworks, compatibility layers, wrappers, or recovery hierarchies for scenarios outside the current requirement.
4. Treat an edge case as relevant when it is reachable through this repository's documented inputs, commands, public interfaces, or real data. Merely constructible cases are insufficient.
5. Use engineering judgement directly. Do not replace a settled decision with scoring tables, new checklists, repeated audits, or verification loops.
6. User instructions, higher-priority safety requirements, and existing project workflows take precedence. When relevant, this includes <required project workflows>.
7. Keep deliverable prose direct. Consolidate genuine limitations under a dedicated section instead of adding defensive caveats to every paragraph, and keep internal instructions out of the artifact.

Before any discretionary check, state the live uncertainty it can resolve and how failure would change the next action. Scope checks to the code, consumers, or numerical behavior affected by the change.

When the requested deliverable exists and the directly relevant checks pass, stop. Record optional confidence improvements as suggestions rather than implementing or using them to block completion. Say plainly when something is correct; do not manufacture findings.
```
