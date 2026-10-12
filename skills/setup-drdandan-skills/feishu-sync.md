# Feishu ticket sync

Project tickets can be transferred with `lark-push-tickets` / `lark-pull-tickets`, installed together. The issue tracker and local engineering configuration remain owned by setup.

```json
{
  "schema": "feishu-sync/v1",
  "project_url": null,
  "context_paths": []
}
```

Bind `project_url` to the project's mother-document Wiki URL on first transfer, using the paired helper's `bind` command. Setup does not require remote authentication. In multi-context projects, `context_paths` contains only setup-declared `src/<context>/CONTEXT.md` files; their docs/adr paths are also allowed.

## Files and dependencies

Transfer project Markdown under `.scratch/`, root CONTEXT/CONTEXT-MAP/TODO and docs/adr. Grill outputs retain their established spec, glossary and ADR locations. Local `docs/agents/` and AGENTS are maintained by setup; attachments are separate reference material.

Online ticket documents carry a leading `feishu-ticket/v1` header with their project-relative path and requires list. Structural dependencies and explicit local dependency links produce this list and clickable remote navigation. Ordinary reference links do not trigger imports.

## Import and conflict rules

HITL selects main tickets. Automatically include their required project-local dependencies, check precise document revisions and reuse unchanged, verified local files. Fetch only the selected closure's necessary bodies. Required dependencies must be ready before overwriting the corresponding main ticket; preserve successful independent groups and report blocked ones.

Display conflicts before replacing local edits or unpulled remote edits. Only verified successes advance each file's baseline. Transfer does not complete tasks or change their Status/Blocked by. Only add/update; no automatic deletion, relocation or cross-project imports.

`.scratch/.feishu-sync/` holds mappings, hashes, plans, staging and program-owned progress/results. Ignore it in Git and exclude it from transfer. Background workers persist their own state; observe read-only log snapshots and end the Codex turn between checks.
