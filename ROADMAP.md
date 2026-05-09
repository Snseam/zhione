# Roadmap

ZhiOne is in the design stage. The roadmap below is intentionally small and
evidence-driven.

## Phase 0: Open Project Foundation

- Keep public repository structure understandable.
- Record the license and contribution model.
- Separate canonical docs from working-note archives.
- Identify private or sensitive material before adding more public docs.

## Phase 1: MVP Specification

- Define the memory object model:
  - source;
  - claim;
  - provenance;
  - confidence;
  - validity period;
  - supersession;
  - deletion or forgetting state.
- Define memory operations:
  - ingest;
  - extract;
  - merge;
  - supersede;
  - forget;
  - query;
  - pack;
  - audit.
- Define the first evaluation fixture for duplicated Markdown documents.
- Decide the first interface: CLI first, MCP first, or both with one shared core.

## Phase 2: Local Prototype

- Build a minimal local ingest pipeline for Markdown files.
- Generate context packs with provenance and token-budget control.
- Produce an audit report explaining why context was selected.
- Run the prototype against the repository's own docs.

## Phase 3: Agent Integration

- Expose a stable CLI command for agent use.
- Add an MCP server once the operation contract is stable.
- Validate with at least two agent workflows.
- Add regression tests for duplicate suppression and stale-memory handling.

## Phase 4: Review UI

- Provide a human review surface for memory changes.
- Show source links, confidence, supersession chains, and deletion controls.
- Keep raw source mutation opt-in and reviewable.

## Non-Goals For Now

- Full notes-app replacement.
- Cloud sync.
- Team administration.
- Automatic irreversible rewriting of personal knowledge bases.
- Large framework commitments before the memory contract is clear.
