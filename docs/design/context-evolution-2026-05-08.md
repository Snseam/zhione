---
title: ZhiOne context evolution record
date: 2026-05-08
status: working-record
language: zh-CN
scope:
  - 本轮关于 ZhiOne 未来发展、Mintlify、Claude Dreams、ICLR 2026、MCP 2026 路线的讨论记录
  - 用于避免后续 context compact 导致信息失真
---

# ZhiOne / 知一 上下文演化记录

## 0. 保存目的

用户要求将本轮对话内容完整保存成文件，避免后续上下文压缩时丢失关键信息。

本文件保存：

- 用户输入的核心材料和问题
- 对当前 ZhiOne 本地文档的读取结论
- 结合 Mintlify、Claude Dreams、ICLR 2026、MCP 2026 路线后的产品与架构判断
- 已经锁定的 v0 / v1 方向
- ASCII 架构图
- 后续需要继续推进的问题

注意：用户在对话中贴入了较长的 Mintlify 文章和 David Soria Parra 分享转写。这里不逐字重贴全文，而是保存与 ZhiOne 相关的关键信息、结论和设计影响，避免把外部文章原文变成项目文档主体。

---

# 1. 当前本地项目状态

当前目录：

```text
/Users/yyl-macbookpro/Program/ZhiOne
```

本轮读取到的文件：

```text
README.md
karpathy_obsidian_memory_research_2026-05-07.md
zhione_project_full_record.md
```

Git 状态在本轮初始读取时为：

```text
## main
```

此前在刚添加文档时曾显示为新仓库且文档未提交；后续状态显示当前分支为 `main`。

---

# 2. 本地文档中的既有共识

## 2.1 ZhiOne 一句话定义

`zhione_project_full_record.md` 中的统一定义：

> 知一 / ZhiOne 是一个本地 Agent Memory 内核 / Local Context Engine。
> 它把散落在 PC 工作流里的文档、代码、会议、笔记与交互记录，编译成一层去重、可追溯、可更新、可遗忘、可按任务调用的记忆层，让人类和 Agent 共用同一套知识内核。

## 2.2 ZhiOne 不是在做什么

ZhiOne 不是：

- 又一个笔记软件
- 单纯的本地 RAG
- 把所有原始文档都塞进长上下文
- 纯粹让 Agent 记得更多

## 2.3 ZhiOne 真正在做什么

ZhiOne 更接近：

- 本地知识中间件
- Agent Memory Kernel
- Context Governor / Context Compiler
- AI-first 存储层 + Human-first 展示层
- 兼容现有 PC 工作流的知识操作系统雏形

## 2.4 现有 MVP 思路

本地文档中原有 MVP 建议：

```text
1. File watcher / parser
2. basic chunk + semantic dedup
3. hybrid search + rerank
4. scope/type/budget gating
5. Context Pack API（优先 MCP / CLI）
6. audit UI（至少能回答“为什么用了这段上下文”）
```

本轮讨论对这个 MVP 做了升级：从单纯读层升级为 `ContextPack Reader + Dream Consolidation Queue`。

---

# 3. Karpathy / Obsidian / LLM Wiki 对 ZhiOne 的既有启发

`karpathy_obsidian_memory_research_2026-05-07.md` 中保存了 Karpathy LLM Wiki 思路：

```text
raw sources -> LLM-maintained wiki -> Obsidian / human / agent reads wiki
```

关键启发：

- Obsidian 是 human-facing shell，不是 memory kernel 本身。
- LLM Wiki 是一种 personal knowledge compilation pattern。
- Wiki 层是比 raw sources 更干净、可 lint、可审计的中间表示。
- ZhiOne 应该进一步下沉为 local agent memory kernel。

ZhiOne 相比 Karpathy LLM Wiki 需要补充：

```text
semantic dedup
provenance
memory type gating
valid time / confidence / trust
participation control
context budget management
Context Pack API
audit UI
multi-agent shared memory
memory abstain / mute / deprioritize
benchmark and regression loop
```

---

# 4. Mintlify 文章带来的判断

用户贴入了一篇关于 Mintlify 的文章，核心信息包括：

- Mintlify 以 AI-native documentation 为定位，围绕技术文档构建产品。
- 文章称 Mintlify 完成 4500 万美元 B 轮，估值 5 亿美元，ARR 约 1000 万美元。
- 客户包括 Anthropic、X、PayPal 等。
- 核心叙事：文档不再只是给人看的说明书，而是产品与 AI / Agent 交互的接口。
- Mintlify 支持 Markdown、机器可读目录、`llms.txt`、`llms-full.txt`、MCP server、AI assistant、文档变更 PR 等能力。
- 文章强调：不要只问 AI 能做什么，也要问 AI 运行时需要什么。

## 4.1 对 ZhiOne 的关键判断

Mintlify 的启发不是“ZhiOne 也应该做文档站”，而是：

> AI 已经成为知识的一等读者，甚至正在成为知识维护者。

因此 ZhiOne 应该吸收 Mintlify 的机制，但不能缩小成 Mintlify 式文档产品。

## 4.2 Mintlify 可借鉴机制

ZhiOne 可以借鉴：

```text
1. AI-readable knowledge layer
   Markdown / llms.txt / llms-full.txt / skill.md / machine-readable index

2. MCP as live knowledge access
   Agent 查询当前最新知识，而不是依赖模型训练记忆

3. Agent edits via PR / review
   AI 可以生成文档或知识更新，但需要 diff 和人工审核

4. AI assistant with citations
   回答必须绑定知识源和引用

5. Agent analytics / content gaps
   观察 Agent 读了什么、问了什么、哪些知识缺失或过期
```

## 4.3 ZhiOne 与 Mintlify 的边界

```text
Mintlify:
documentation becomes AI interface

ZhiOne:
memory lifecycle becomes agent infrastructure
```

ZhiOne 的中心对象不应该是“文档页面”，而应该是“可治理记忆单元”。

---

# 5. 最新 AI 论文和产品趋势对 ZhiOne 的影响

本轮查阅和讨论了 ICLR 2026 / Anthropic / MCP 相关方向。核心不是列论文，而是抽取与 ZhiOne 直接相关的机制。

## 5.1 多轮对话会让 Agent 迷路

相关论文：

- `LLMs Get Lost In Multi-Turn Conversation`

关键发现：

- 同样信息如果被拆成多轮逐步提供，模型表现显著下降。
- 问题不是信息不足，而是模型容易被早期假设锁定，后续难以恢复。

对 ZhiOne 的启发：

```text
raw conversation history 不是长期上下文
session trace -> checkpoint summary -> MemoryEvent -> ContextPack
```

因此 ZhiOne 不能依赖原始聊天历史作为长期状态，必须把多轮过程重新编译成结构化记忆。

## 5.2 MemoryAgentBench 的四类记忆能力

相关论文：

- `Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions`
- MemoryAgentBench

它把 memory agent 的核心能力拆成：

```text
accurate retrieval
test-time learning
long-range understanding
selective forgetting
```

对 ZhiOne benchmark 的启发：

```text
B1 accurate retrieval: 找到正确来源
B2 test-time learning: 新信息是否进入后续任务
B3 long-range understanding: 跨多文件/多会话是否保持一致
B4 selective forgetting: 过期/冲突/无关记忆是否不参与
```

## 5.3 Claude Dreams 产品化了“睡眠整理”

Anthropic Claude Dreams 的官方思路：

- Agent 工作时写 memory 是局部和增量的。
- 长期会积累重复、矛盾、过期条目。
- Dreams 会读取 memory store 和过去 sessions，生成一个新的整理后 memory store。
- 原输入 store 不被直接修改，便于审核和丢弃。

对 ZhiOne 的启发：

```text
ZhiOne 不能只做:
读文档 -> 去重 -> 给 agent

还必须从第一天有最小睡眠整理闭环:
Agent 使用 ContextPack
-> 产生 trace / session transcript
-> offline dream job 整理知识
-> merge duplicates / resolve stale / find contradictions / surface insights
-> 生成候选 MemoryDiff
-> 人审或规则门控后进入 canonical layer
```

结论：

> v0 从 ContextPack Reader 升级为 ContextPack Reader + Dream Consolidation Queue。

## 5.4 Agentic RAG 进入过程级检索

相关方向：

- MC-Search
- HiPRAG

核心启发：

- RAG 不应该只是 top-k chunk。
- 检索计划、中间判断、支撑证据、遗漏项、冲突，都应该可审计。

ZhiOne 的 `ContextPack` 应该包含：

```text
task_intent
retrieval_plan
selected_evidence
omitted_duplicates
conflicts
confidence
budget_decision
why_used
why_not_used
```

## 5.5 GraphRAG 有价值，但 v0 不应重度依赖复杂图谱

相关方向：

- UniGraphRAG
- LinearRAG

讨论结论：

```text
v0 不做完整知识图谱
v0 做 semantic clusters + source graph + lightweight entity/topic links
GraphRAG 作为 enricher plugin
关系抽取不能成为核心读路径的前置依赖
```

## 5.6 长上下文不是答案

相关方向：

- MemAgent
- ReMemR1
- Retrieval-of-Thought

对 ZhiOne 的三层知识启发：

```text
MemoryEvent: 原始记忆事件不可丢
CanonicalCluster: 当前规范表述可更新
Thought/WorkflowTrace: 成功任务路径可复用
```

ZhiOne 需要区分：

- 事实知识
- 过程知识
- 组织知识

## 5.7 时间和版本是核心

相关方向：

- Memory-T1

企业知识最大风险不是“找不到”，而是“找到旧的但看起来合理”。

每个 memory / canonical cluster 需要：

```text
valid_from
valid_to
source_version
supersedes
contradicted_by
last_verified_at
authority_rank
```

## 5.8 多 Agent 需要治理，不是简单共享

相关方向：

- Lazy Agents to Deliberation

ZhiOne 不能让多个 agent 共享一个无边界 memory bucket。必须区分：

```text
shared memory
agent-private scratch
task-local trace
promoted canonical memory
```

Dream job 不只整理内容，也可以整理 agent 行为：

```text
哪个 agent 的信息贡献高？
哪个 agent 经常引入错误上下文？
哪些任务需要从 checkpoint 重启？
哪些 trace 应该被晋升为 workflow memory？
```

---

# 6. 已锁定的 ZhiOne v0 定义

用户确认升级 v0 定义：

> ZhiOne v0 = ContextPack Reader + Dream Consolidation Queue

## 6.1 v0 支持的数据源

v0 第一版只支持：

```text
本地 Markdown / MDX / text 文件夹
```

暂不接入：

- Notion
- Slack
- Linear
- 飞书
- Git history
- 多人权限系统

## 6.2 v0 同时验证两个 profile

```text
developer-docs:
repo docs / README / AGENTS.md / architecture / PRD / design notes / code notes

business-docs:
PRD / meeting notes / SOP / FAQ / release notes / operation manuals
```

底层处理一致，上层模板不同。

## 6.3 v0 主接口

用户确认 v0 使用：

```text
CLI + MCP
```

CLI 用于：

- 调试
- benchmark
- 批处理
- 可复现运行

MCP 用于：

- Codex / Claude Code / enterprise agent 调用
- 动态 ContextPack 获取
- 解释和审计

## 6.4 v0 智能程度

用户确认 v0 做：

```text
语义去重 + 证据
```

也就是：

- 分块
- embedding / 文本相似
- 聚类 canonical source
- ContextPack 附来源和 why-used

暂不做复杂知识图谱。

## 6.5 Dream 机制范围

用户确认 v0 Dream 采用：

```text
候选变更队列
```

Dream job 只生成可审查的 MemoryDiff：

- merge
- supersede
- archive
- conflict
- new_insight

不自动修改 canonical memory。

---

# 7. v0 核心对象设计

## 7.1 SourceDocument

```text
SourceDocument
- id
- path
- kind: markdown | mdx | text
- profile: developer-docs | business-docs
- content_hash
- modified_at
```

## 7.2 SemanticUnit

```text
SemanticUnit
- id
- source_document_id
- heading_path
- text
- text_hash
- embedding_ref
- unit_type: concept | decision | requirement | procedure | changelog | note
- provenance
```

## 7.3 CanonicalCluster

```text
CanonicalCluster
- id
- canonical_text
- member_unit_ids
- duplicate_score
- authority_source_id
- status: active | stale | archived | conflicted
- valid_from / valid_to
- supersedes / contradicted_by
```

## 7.4 RetrievalRequest

```text
RetrievalRequest
- query
- task_type
- profile
- source_scope
- budget
- mode: explore | execute | creative | review
```

## 7.5 ContextPack

```text
ContextPack
- request_id
- selected_clusters
- selected_units
- omitted_duplicates
- conflicts
- budget_report
- why_used
- citations
```

## 7.6 Trace

```text
Trace
- request_id
- agent_id
- context_pack_id
- used_cluster_ids
- user_outcome_signal optional
- failure_notes optional
```

## 7.7 MemoryDiff

```text
MemoryDiff
- id
- diff_type: merge | supersede | archive | new_insight | conflict
- proposed_by: dream_job
- evidence
- before
- after
- risk_level
- status: pending | accepted | rejected | superseded
```

---

# 8. v0 模块边界

```text
Ingest
- 扫描本地 Markdown/MDX/text
- 按标题和段落切 SemanticUnit
- 记录 source hash 和 provenance

Index
- BM25 / keyword index
- embedding index
- cluster index
- source graph: document -> heading -> unit -> cluster

Reader
- 接收 RetrievalRequest
- 混合检索 + rerank
- 语义去重
- 组装 ContextPack
- 写 Trace

Dream
- 定期或手动读取 Trace + CanonicalCluster + SourceDocument changes
- 生成 MemoryDiff queue
- 不直接修改 canonical cluster

Review
- CLI 或最小 Web/audit view
- 展示 MemoryDiff、why-used、duplicates、conflicts
- 接受/拒绝候选变更

Interfaces
- CLI: index/query/dream/diff/review
- MCP: query_context / explain_context / list_diffs / read_source
- Future publisher: llms.txt / llms-full.txt / skill.md / wiki view
```

---

# 9. v0 在线读路径

```text
agent query
-> RetrievalRequest
-> retrieve candidate units
-> map units to canonical clusters
-> remove duplicates
-> preserve citations to original sources
-> mark conflicts/stale candidates
-> assemble ContextPack within budget
-> emit Trace
```

---

# 10. v0 离线 Dream 路径

```text
recent traces + changed docs + active clusters
-> detect repeated retrieval of equivalent units
-> detect contradictions and stale sources
-> identify knowledge gaps from failed/low-confidence traces
-> propose MemoryDiff
-> wait for review
```

Dream 的核心原则：

> Dream 只产生候选，不自动改记忆。

这样既吸收 Claude Dreams 的离线整理思想，也避免黑盒自动改知识。

---

# 11. v0 验收标准

第一版必须证明：

```text
1. 对一组有重复内容的 Markdown 文档，ContextPack token 比直接读取明显减少。
2. ContextPack 仍能保留来源引用，不因为去重丢证据。
3. Agent 可以通过 CLI 和 MCP 拿到同一份 ContextPack。
4. 多次查询后，dream job 能产生合理的 MemoryDiff 候选。
5. 人可以看到 why-used、omitted duplicates、conflicts、proposed diffs。
```

第一版不做：

```text
- 多人权限
- 复杂图谱数据库
- 自动接受高风险 MemoryDiff
- Notion/Slack/Linear/飞书集成
- 完整 Mintlify 式文档站
- learned retrieval policy
```

---

# 12. Research Radar / Architecture Review Loop

用户提出：

> 论文和 AI 的进步是不断更新的，如何设计一套机制，可以定期获取最新的信息，并且评判是否需要对架构进行更新。当然这个机制可以不用一开始就做，可以规划在后续版本中。

用户确认它应定位为：

```text
核心进化层
```

## 12.1 核心流程

```text
定期获取前沿信息
-> 结构化理解论文/产品更新
-> 映射到 ZhiOne 能力模块
-> 评估证据强度和适配度
-> 生成 Architecture Proposal
-> 沙盒实验
-> benchmark / shadow run
-> 人审后进入 roadmap 或插件
```

## 12.2 信息源

```text
论文源:
- arXiv cs.AI / cs.CL / cs.IR / cs.LG
- OpenReview: ICLR / NeurIPS / ICML / ACL / EMNLP
- Papers with Code / Semantic Scholar

产品与工程源:
- Anthropic / OpenAI / Google / Microsoft / Meta 官方 docs 和 blog
- LangChain / LlamaIndex / Zep / Mem0 / Graphiti / MCP 生态
- GitHub trending / release notes

社区信号:
- HN / Reddit / X / 研究者博客
- Karpathy / Lilian Weng / Chip Huyen 等高质量个人来源
```

## 12.3 ResearchItem

```text
ResearchItem
- title
- source
- date
- domain: memory | retrieval | graph | agent | eval | compression | UI | security
- core_claim
- method_summary
- required_assumptions
- benchmark_used
- evidence_level
- code_available
- license
- cost_complexity
- relevance_to_zhione
```

## 12.4 模块 taxonomy

每个新技术必须映射到 ZhiOne 的模块：

```text
Ingest adapter
Parser / chunker
Semantic dedup
Retriever / reranker
Context packer
Dream consolidator
MemoryDiff generator
Evaluator / benchmark
Publisher: llms.txt / skill.md / wiki
MCP / CLI interface
Audit UI
```

## 12.5 ArchitectureImpactReport

```text
ImpactReport
- solves_what_problem
- affected_modules
- expected_gain
- evidence_strength: weak | medium | strong
- implementation_cost
- runtime_cost
- privacy_risk
- maintenance_risk
- backwards_compatibility
- recommended_action:
  - ignore
  - monitor
  - prototype
  - adopt_as_plugin
  - consider_core_change
```

## 12.6 沙盒实验

```text
candidate plugin
-> run on fixed EvalCase suite
-> run on historical traces
-> shadow compare against current baseline
-> measure:
   - retrieval quality
   - duplicate collapse
   - stale/conflict detection
   - context token reduction
   - latency/cost
   - regression rate
```

## 12.7 架构决策

通过实验的候选再生成 ADR / RFC：

```text
ADR
- why now
- what changes
- rejected alternatives
- migration path
- rollout flag
- rollback condition
- benchmark evidence
```

## 12.8 版本规划

```text
v0:
- 只预留 ResearchItem / EvalCase / plugin taxonomy 概念
- 不做自动 research radar

v0.5:
- 手动 research ingest
- 把论文/产品更新整理成 ResearchItem
- 生成 ArchitectureImpactReport 草稿

v1:
- 定期 research radar，每周/每月扫描论文和产品更新
- 自动归类到模块 taxonomy
- 生成候选 Architecture Proposal

v2:
- research-plugin sandbox
- 自动跑 benchmark / shadow run
- 通过阈值后生成 ADR

v3:
- ZhiOne 自己维护“技术路线记忆”
- Dream job 不只整理用户文档，也整理 ZhiOne 自身架构知识
```

一句话：

> ZhiOne 不只管理知识，也管理知识系统自己的进化。

---

# 13. 本轮提出的 proposed plan

本轮曾输出以下计划：

```text
ZhiOne 定位为本地优先的 Agent Memory Kernel / Context Engine，不是普通文档站，也不是单纯 RAG。

v0 做 ContextPack Reader + Dream Consolidation Queue：
- 在线为 Agent 提供去重、可追溯、按任务组装的上下文；
- 离线根据使用痕迹生成可审查的 MemoryDiff，避免文档和记忆越积越乱。

v1+ 加入 Research Radar / Architecture Review Loop，作为核心进化层：
- 定期吸收最新论文、AI 产品和工程实践；
- 判断是否需要通过插件、实验或架构 RFC 更新 ZhiOne。
```

核心假设：

```text
- v0 不做多人权限、协作工具集成、复杂知识图谱、自动高风险记忆修改。
- GraphRAG、thought reuse、adaptive routing 先作为后续插件轨道，不进入 v0 核心。
- Research Radar 是核心进化层，但从 v1 开始，不阻塞 v0。
```

---

# 14. ASCII 总体架构

用户要求用 ASCII 展示整体架构，本轮输出如下：

```text
┌──────────────────────────────────────────────────────────────────────┐
│                         EXISTING WORKFLOW                            │
│                                                                      │
│  Developer docs        Business docs        Agent/session traces      │
│  README / AGENTS       PRD / SOP / FAQ      Codex / Claude Code       │
│  Architecture notes    Meeting notes        MCP agent outputs         │
│  Markdown / MDX        Release notes        Failures / decisions      │
└───────────────────────────────┬──────────────────────────────────────┘
                                │
                                v
┌──────────────────────────────────────────────────────────────────────┐
│                         INGEST LAYER                                  │
│                                                                      │
│  File watcher     Parser      Chunker      Metadata extractor         │
│  .md/.mdx/.txt -> headings -> SemanticUnit -> ProvenanceRef           │
└───────────────────────────────┬──────────────────────────────────────┘
                                │
                                v
┌──────────────────────────────────────────────────────────────────────┐
│                    CANONICAL MEMORY KERNEL                            │
│                                                                      │
│  SourceDocument                                                       │
│  SemanticUnit                                                         │
│  CanonicalCluster                                                     │
│                                                                      │
│  - semantic dedup                                                     │
│  - canonical source selection                                         │
│  - provenance / citations                                             │
│  - status: active / stale / archived / conflicted                     │
│  - valid time / supersede / contradiction                             │
└───────────────┬──────────────────────────────────────┬───────────────┘
                │                                      │
                v                                      v
┌──────────────────────────────┐        ┌──────────────────────────────┐
│       ONLINE READ PATH        │        │     OFFLINE DREAM PATH       │
│                              │        │                              │
│  RetrievalRequest             │        │  Trace + changed docs        │
│  -> hybrid search             │        │  -> dream consolidation      │
│  -> rerank                    │        │  -> duplicate detection      │
│  -> dedup                     │        │  -> stale/conflict detection │
│  -> budget control            │        │  -> new insight proposal     │
│  -> ContextPack               │        │  -> MemoryDiff queue         │
└───────────────┬──────────────┘        └───────────────┬──────────────┘
                │                                      │
                v                                      v
┌──────────────────────────────┐        ┌──────────────────────────────┐
│          CONTEXTPACK          │        │        MEMORYDIFF REVIEW     │
│                              │        │                              │
│  selected clusters            │        │  merge                       │
│  selected source units         │        │  supersede                   │
│  omitted duplicates            │        │  archive                     │
│  conflicts                     │        │  conflict                    │
│  why-used                      │        │  new_insight                 │
│  citations                     │        │                              │
│  budget report                 │        │  pending / accepted / reject │
└───────────────┬──────────────┘        └───────────────┬──────────────┘
                │                                      │
                v                                      v
┌──────────────────────────────────────────────────────────────────────┐
│                         INTERFACE LAYER                               │
│                                                                      │
│  CLI                         MCP                                      │
│  zhione index                query_context                            │
│  zhione query                explain_context                          │
│  zhione dream                list_diffs                               │
│  zhione review               read_source                              │
│                                                                      │
│  Future publishing exits:                                             │
│  llms.txt / llms-full.txt / skill.md / wiki view / docs MCP           │
└───────────────┬──────────────────────────────────────┬───────────────┘
                │                                      │
                v                                      v
┌──────────────────────────────┐        ┌──────────────────────────────┐
│          AGENT SIDE           │        │           HUMAN SIDE         │
│                              │        │                              │
│  Codex                        │        │  Audit view                  │
│  Claude Code                  │        │  Why-used view               │
│  internal enterprise agents   │        │  Duplicate clusters          │
│  future MCP clients           │        │  MemoryDiff approval         │
└───────────────┬──────────────┘        └───────────────┬──────────────┘
                │                                      │
                └──────────────────┬───────────────────┘
                                   v
┌──────────────────────────────────────────────────────────────────────┐
│                    EVAL / RESEARCH EVOLUTION LOOP                     │
│                                                                      │
│  EvalCase                                                             │
│  - overlap-collapse                                                   │
│  - memory-abstain                                                     │
│  - update-supersede                                                   │
│  - trace-to-diff                                                      │
│  - context-token-reduction                                            │
│                                                                      │
│  Research Radar                                                       │
│  - ingest new papers / products / OSS updates                         │
│  - map to module taxonomy                                             │
│  - ArchitectureImpactReport                                           │
│  - plugin sandbox                                                     │
│  - benchmark / shadow run                                             │
│  - ADR / RFC / promote / rollback                                     │
└──────────────────────────────────────────────────────────────────────┘
```

压缩版：

```text
Local docs + agent traces
-> canonical memory kernel
-> ContextPack for immediate agent use
-> Dream Queue for memory cleanup
-> Eval + Research Radar for long-term architecture evolution
```

---

# 15. David Soria Parra / MCP 分享对 ZhiOne 的启发

用户贴入了 David Soria Parra 在 AI Engineer 分享中关于 MCP 的内容，核心包括：

- 2026 是 Agent 走向生产化的一年。
- Coding Agent 和通用知识工作 Agent 的差异在于：后者核心不是推理能力，而是连接能力。
- 未来 Agent 会同时使用 Skills、MCP、CLI、computer use，而不是单一连接方案。
- MCP 上下文膨胀需要通过渐进式发现、程序化工具调用、Agent-first server design 解决。
- MCP 正从工具协议升级为应用分发层。
- MCP Apps 让 MCP server 可以携带 UI。
- 无状态传输、server discovery、cross-app access、Skills over MCP 都是 2026 的关键演进。

本轮查阅了官方来源：

- MCP 2026 Roadmap
- MCP Roadmap docs
- Anthropic engineering: Code execution with MCP
- MCP Apps official blog
- Skills Over MCP Charter

## 15.1 对 ZhiOne 的一句话结论

> ZhiOne 不能只是“通过 MCP 提供 ContextPack”，而应该成为 Agent 的渐进式上下文发现、编排和审计层。

## 15.2 MCP 不是唯一入口

David 强调 2026 的 Agent 会同时使用：

```text
Skills + MCP + CLI / computer use
```

因此 ZhiOne 不能 MCP-only。

应该升级成：

```text
CLI    -> 本地开发者、benchmark、可复现调试
MCP    -> Agent 标准连接、企业集成、UI 应用
Skills -> 分发领域知识、使用协议、工作流说明
```

ZhiOne 是 Context Engine，而不是单个 MCP server。

## 15.3 渐进式发现正好对应 ZhiOne 核心痛点

MCP 被批评上下文膨胀，本质和 ZhiOne 起点一致：不要一上来全给。

ZhiOne 的 MCP 工具设计不应该是：

```text
list_all_docs
read_doc
search_everything
```

而应该是：

```text
discover_context_scope
search_context
get_context_pack
expand_cluster
explain_context
list_memory_diffs
```

也就是：

```text
先发现范围
-> 再按需展开
-> 最后才读取证据
```

## 15.4 程序化工具调用启发 ZhiOne 加 Context SDK

Anthropic 官方工程文章指出，把 MCP 工具当代码 API 使用，可以减少工具定义和中间结果进入上下文的成本。

ZhiOne 后续可以提供：

```text
zhione.search(query)
zhione.get_context_pack(...)
zhione.expand(cluster_id)
zhione.filter_by_source(...)
zhione.explain(...)
```

Agent 在 sandbox 里用代码筛选、聚合、过滤，最后只把最小结果带回模型。

这比让模型多轮调用 MCP 工具更适合大文档、大知识库场景。

## 15.5 Server 要 Agent-first，不要 REST API 映射

ZhiOne 不应该把内部对象 CRUD 暴露成 MCP 工具：

```text
getSemanticUnit
listCanonicalCluster
updateMemoryDiff
```

更合理的是面向 Agent 任务暴露高层能力：

```text
prepare_context_for_task
compare_context_sources
find_conflicts_for_task
summarize_changed_context
propose_memory_diff
review_memory_diff
```

Agent 关心的是“帮我完成当前任务需要什么上下文”，不是数据库表结构。

## 15.6 MCP Apps 对 ZhiOne audit UI 很有价值

MCP Apps 已经成为官方扩展，允许 MCP server 返回交互式 UI。

ZhiOne 的人类侧最小 UI 可以不是独立 Web App，而是 MCP App：

```text
ContextPack Viewer
Why-used Viewer
Duplicate Cluster Viewer
MemoryDiff Review UI
Conflict Resolution UI
```

这适合 ZhiOne，因为“人类审核”本来就是核心机制。

## 15.7 Skills over MCP 是 ZhiOne 的分发协议

Skills over MCP 正在成为工作组方向。

ZhiOne 可以把不同 profile 作为 skill 分发：

```text
developer-docs skill
business-docs skill
prd-review skill
architecture-review skill
research-radar skill
```

MCP 提供连接，Skill 提供“怎么用 ZhiOne”的领域知识。

## 15.8 对 ZhiOne 接口层的修正

原先是：

```text
CLI + MCP
```

建议升级为：

```text
Connection Layer
├─ CLI
│  ├─ index
│  ├─ query
│  ├─ dream
│  └─ eval
├─ MCP
│  ├─ progressive context discovery
│  ├─ ContextPack API
│  ├─ MemoryDiff API
│  └─ MCP Apps UI
├─ Skills
│  ├─ developer-docs
│  ├─ business-docs
│  └─ research-radar
└─ Context SDK
   ├─ code-mode APIs
   └─ sandbox-side filtering
```

## 15.9 v0 / v0.5 / v1 修正

v0 仍然不变：

```text
ContextPack Reader + Dream Consolidation Queue
```

但 v0 的 MCP 设计要遵守三条原则：

```text
1. progressive discovery first
2. task-level tools, not CRUD tools
3. explainability and audit are first-class
```

v0.5 增加：

```text
Context SDK for programmatic tool calling
MCP App for ContextPack / MemoryDiff review
Skills for developer-docs and business-docs
```

v1 再考虑：

```text
remote MCP / stateless transport
server discovery / server card
enterprise auth / gateway / cross-app access
```

## 15.10 底层判断

David 的分享不是削弱 ZhiOne，反而强化了它的必要性。

当 Agent 连接能力越来越强，工具、MCP server、skills、UI 都会变多，真正稀缺的是：

```text
上下文治理层
```

ZhiOne 应站的位置：

```text
Many tools, docs, apps, skills
        ↓
ZhiOne context governor
        ↓
agent gets only the right memory, at the right time
```

---

# 16. 需要保留的战略表达

## 16.1 ZhiOne 主定位

```text
ZhiOne is a local-first Agent Memory Kernel / Context Engine.
```

中文：

```text
ZhiOne 是本地优先的 Agent Memory 内核 / 上下文引擎。
```

## 16.2 主 slogan

```text
让 Agent 在对的时候，用对的记忆
```

## 16.3 副 slogan

```text
让 AI 只读一份知识
```

## 16.4 更新后的叙事

```text
ZhiOne 不只是文档去重工具。
它是一个 context lifecycle system：

读取 -> 使用 -> 追踪 -> 睡眠整理 -> 再组织 -> 再发布 -> 架构进化
```

---

# 17. 当前最重要的产品原则

## 17.1 不让 Agent 直接面对原始文件洪流

所有 Agent 应通过统一的 ContextPack API 获取任务感知后的最小足够上下文。

## 17.2 读层和整理层必须并存

只有读层会陷入“AI 文档陷阱”：

```text
文档越积越多
索引越来越厚
去重只能缓解读取
但不能解决更新、过期、冲突和洞察生成
```

因此 v0 必须包含 Dream Consolidation Queue。

## 17.3 新论文优先作为插件进入

Research Radar 的默认原则：

```text
新论文 / 新方法
-> ResearchItem
-> ArchitectureImpactReport
-> sandbox plugin
-> benchmark / shadow run
-> ADR / promote / rollback
```

不要因为新方法流行就改核心架构。

## 17.4 MCP 工具要 task-level，不要 CRUD-level

ZhiOne 的 MCP server 不是内部数据库 API，而是 Agent 的上下文治理接口。

## 17.5 Human review 是核心，不是附属

ContextPack、MemoryDiff、conflict、why-used 都必须可审计。

---

# 18. 后续可继续推进的问题

## 18.1 需要写成正式 PRD 的内容

- v0 用户画像
- developer-docs / business-docs 两个 profile 的具体样例任务
- ContextPack schema v0
- MemoryDiff schema v0
- Dream job v0 规则
- MCP progressive discovery tool set
- CLI 命令清单
- audit UI / MCP App 最小交互

## 18.2 需要写成工程 spec 的内容

- 本地存储：SQLite / 文件索引 / embedding index 选择
- parser / chunker 边界
- semantic dedup 算法 v0
- cluster canonical source 选择规则
- ContextPack budget 策略
- Trace 写入和脱敏
- MemoryDiff review 状态机
- EvalCase 数据格式

## 18.3 需要继续研究的内容

- Claude Dreams 更完整的产品形态和限制
- MCP Apps 的安全边界与适合 ZhiOne 的 UI 形态
- Skills over MCP 的最终 spec 走向
- Server Cards / discovery 对 ZhiOne 远期发布层的影响
- Programmatic tool calling / Context SDK 的安全沙箱设计
- MCP security / prompt injection / tool poisoning 防护

---

# 19. 本轮引用和查阅的主要来源

```text
MCP 2026 Roadmap
https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/

MCP Roadmap docs
https://modelcontextprotocol.io/development/roadmap

MCP transport future
https://blog.modelcontextprotocol.io/posts/2025-12-19-mcp-transport-future/

Anthropic: Code execution with MCP
https://www.anthropic.com/engineering/code-execution-with-mcp

MCP Apps
https://blog.modelcontextprotocol.io/posts/2026-01-26-mcp-apps/

Skills Over MCP Charter
https://modelcontextprotocol.io/community/skills-over-mcp/charter

Claude Dreams docs
https://platform.claude.com/docs/en/managed-agents/dreams

Mintlify AI-native docs
https://www.mintlify.com/docs/ai-native

ICLR 2026 Review Retrospective
https://blog.iclr.cc/2026/03/31/a-retrospective-on-the-iclr-2026-review-process/

LLMs Get Lost In Multi-Turn Conversation
https://openreview.net/forum?id=VKGTGGcwl6

MemoryAgentBench
https://iclr.cc/virtual/2026/poster/10010781
```

---

# 20. 下一步建议

最合理的下一步是把本文件和已有 `zhione_project_full_record.md` 合并沉淀成正式 PRD：

```text
docs/prd/zhione-v0-contextpack-reader-dream-queue.md
```

PRD 应聚焦：

```text
v0 = ContextPack Reader + Dream Consolidation Queue
```

不要一开始写成完整 ZhiOne 远景大文档，否则会重新发散。

工程计划应随后拆成：

```text
1. schema and storage
2. ingest and semantic unit extraction
3. retrieval and dedup
4. ContextPack CLI
5. MCP progressive discovery
6. Trace
7. Dream MemoryDiff queue
8. audit review
9. benchmark v0
```
