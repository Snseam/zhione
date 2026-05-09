# ZhiOne / 知一

ZhiOne is a local context engine for AI agents. It turns scattered documents,
notes, code history, meeting records, and working conversations into a
deduplicated, traceable, updateable, and task-aware memory layer.

The project is at the product and architecture design stage. The current
repository is public so the concept, design notes, and future implementation can
evolve in the open.

## What ZhiOne Is

ZhiOne is not another notes app and not a thin RAG wrapper. It is intended to be
a local memory kernel that sits below tools such as Codex, Claude Code, editors,
Obsidian, and future desktop agents.

The target shape:

```text
local sources -> memory operations -> canonical memory -> context packs -> agents
```

Core goals:

- Reduce repeated context reads across related local documents.
- Preserve provenance so every memory can be traced back to source material.
- Keep memory updateable, supersedable, forgettable, and auditable.
- Let agents request the smallest sufficient context for a task.
- Keep humans in control through reviewable files, diffs, and audit views.

## Repository Status

This repository currently contains:

- Product and architecture notes.
- Research notes about local AI memory and LLM-maintained wiki workflows.
- Early roadmap and contribution guidelines.

There is no production implementation yet. Contributions are most useful when
they clarify product scope, memory operation contracts, data model boundaries,
evaluation methods, or small prototype slices.

## Documentation

- [Documentation index](docs/README.md)
- [Roadmap](ROADMAP.md)
- [License decision](docs/license-decision.md)
- [Project conversation archive](docs/archive/project-conversation-record-2026-05-07.md)
- [Context evolution notes](docs/design/context-evolution-2026-05-08.md)
- [Karpathy / Obsidian / memory research](docs/research/karpathy-obsidian-memory-2026-05-07.md)

## Proposed Architecture

The first useful version should stay small:

1. Ingest local Markdown and structured text sources.
2. Extract memory candidates with provenance.
3. Merge or supersede duplicate and stale memory.
4. Build task-scoped context packs through a CLI or MCP interface.
5. Expose an audit trail explaining why each memory was used.

Implementation should favor local-first storage, plain files where possible, and
clear operation logs over opaque automatic mutation.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Until code lands, issues and PRs should
focus on:

- narrowing MVP scope;
- specifying memory operations;
- defining evaluation fixtures;
- improving documentation clarity;
- identifying risks around privacy, provenance, and agent autonomy.

## Security And Privacy

ZhiOne is intended to work with local personal and organizational knowledge.
Privacy and provenance are core product requirements, not add-ons.

Do not commit private notes, credentials, customer data, proprietary source code,
or raw conversation logs unless they are intentionally public and reviewed. See
[SECURITY.md](SECURITY.md).

## License

ZhiOne is licensed under the Apache License 2.0. See [LICENSE](LICENSE) and the
license rationale in [docs/license-decision.md](docs/license-decision.md).
