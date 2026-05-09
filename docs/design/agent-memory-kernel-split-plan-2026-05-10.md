---
title: ZhiOne and Agent Memory Kernel split plan
date: 2026-05-10
status: proposed-plan
language: zh-CN
scope:
  - ZhiOne product engineering boundary
  - standalone agent memory kernel repository
  - benchmark-driven memory algorithm iteration
---

# ZhiOne 与 Agent Memory Kernel 拆分方案

## Summary

将当前 ZhiOne 拆成两个正向循环的工程层：

```text
agent-memory-kernel
  通用、开源、benchmarkable 的 agent memory 内核

zhione
  使用该内核的本地 context engine / 产品壳
```

拆分后的核心原则：

- `agent-memory-kernel` 不知道 ZhiOne、ContextPack、MCP、audit UI。
- `zhione` 不实现记忆算法，只负责本地源接入、产品工作流、ContextPack 编译和人类审计。
- 两者通过稳定 `MemoryProvider` API 连接。
- 内核用公开 benchmark 和 ZhiOne 私有 fixtures 验证；ZhiOne 用真实 trace 反哺内核。

## Architecture

```text
External Benchmarks
LongMemEval / MemoryAgentBench / LoCoMo / MemoryArena
        |
        v
+--------------------------------------------------+
| agent-memory-kernel                              |
|--------------------------------------------------|
| TS runtime core                                  |
| Generic memory objects                           |
| Retrieval / dedup / update / forgetting          |
| Dream / consolidation candidates                 |
| Eval runner + Python benchmark adapters          |
+----------------------+---------------------------+
                       |
                       | MemoryProvider API
                       v
+--------------------------------------------------+
| ZhiOne                                           |
|--------------------------------------------------|
| Local source adapters: Markdown / MDX / text     |
| Workspace indexing                               |
| ContextPack compiler                             |
| CLI / MCP / future Skills                        |
| Audit UI / MemoryDiff review                     |
| Real trace collection                            |
+--------------------------------------------------+
```

## Key Interfaces

`agent-memory-kernel` exposes generic objects only:

```ts
type MemoryRecord
type MemoryQuery
type MemoryResult
type ProvenanceRef
type MemoryTrace
type MemoryDiff
type EvalCase
```

Core API:

```ts
interface AgentMemoryKernel {
  ingest(batch: MemoryIngestBatch): Promise<IngestReport>
  retrieve(query: MemoryQuery): Promise<MemoryResult>
  consolidate(request: ConsolidationRequest): Promise<MemoryDiff[]>
  explain(traceId: string): Promise<MemoryTrace>
}
```

`zhione` maps product concepts onto that API:

```ts
interface ZhiOneContextEngine {
  indexWorkspace(path: string, profile: Profile): Promise<IndexReport>
  prepareContext(task: TaskRequest): Promise<ContextPack>
  proposeMemoryReview(): Promise<MemoryDiff[]>
  explainContext(contextPackId: string): Promise<ContextAudit>
}
```

Important boundary:

```text
MemoryResult -> ZhiOne Context Compiler -> ContextPack
```

`ContextPack` stays in ZhiOne, not in the generic memory kernel.

## Implementation Strategy

1. Create separate repo working name: `agent-memory-kernel`.
2. Main runtime: TypeScript.
3. Benchmark bridge: Python adapters where external benchmark harnesses require Python.
4. Initial kernel storage: SQLite or in-memory test store behind `MemoryStore`.
5. Initial retrieval: BM25/FTS + pluggable embedding/vector adapter.
6. Initial consolidation: duplicate, stale, conflict, forget-candidate detection only.
7. ZhiOne consumes kernel as package dependency and becomes the first real host app.
8. ZhiOne keeps CLI/MCP/audit/ContextPack logic in its own repo.

## Evaluation Plan

Public benchmark adapters:

- LongMemEval: long-term conversational memory, update, abstention.
- MemoryAgentBench: accurate retrieval, test-time learning, long-range understanding, selective forgetting.
- LoCoMo: long conversation and temporal/causal memory.
- MemoryArena later: multi-session agentic task memory.

Private ZhiOne eval fixtures:

- duplicate Markdown collapse;
- stale PRD supersession;
- conflicting meeting notes;
- memory abstain when irrelevant;
- context budget overflow;
- source citation preservation;
- Dream-generated MemoryDiff quality.

Success criteria:

- Kernel benchmark runner can run without ZhiOne.
- ZhiOne can swap kernel implementation without changing CLI/MCP contract.
- ZhiOne traces can be exported into kernel eval fixtures.
- ContextPack quality improves when kernel retrieval/consolidation improves.

## Assumptions

- The agent memory repo is a general open-source kernel from day one.
- Runtime core is TypeScript; Python is used only for benchmark adapters and research harnesses.
- Kernel objects are generic; ZhiOne-specific `ContextPack` remains outside the kernel.
- ZhiOne remains the first host product and strongest real-world feedback loop.
- Research Radar later watches papers/products and proposes kernel experiments first, not direct ZhiOne rewrites.

## References

- [LongMemEval](https://arxiv.org/abs/2410.10813)
- [LoCoMo](https://aclanthology.org/2024.acl-long.747.pdf)
- [MemoryAgentBench](https://openreview.net/pdf/d521fe9beb913e6b527dd100f94b0329412ca274.pdf)
- [MemoryArena](https://arxiv.org/abs/2602.16313)
