# Agent Systems Reading List

按系统设计问题组织的阅读清单。每条只列链接、标题、简介。方法论与评测细节见 [ROADMAP](ROADMAP.md)。值得长期关注的博客、文档、播客见文末 [Resources](#resources)。

---

## Phase 1：建立直觉

### Building Effective AI Agents

<https://www.anthropic.com/research/building-effective-agents>

[Notes](reading_notes/phase1/Building%20Effective%20AI%20Agents.md)

Anthropic。区分 workflow 和 agent，讲什么时候不该上 agent、tool design、planning、delegation。读完能理解 Claude Code 为什么是一个 while loop。

### Effective Context Engineering for AI Agents

<https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents>

[Notes](reading_notes/phase1/Effective%20Context%20Engineering%20for%20AI%20Agents.md)

Anthropic。把 skill、memory、RAG、context compression 统一进 context engineering 的框架；重点看 just-in-time context——Claude Code 如何用引用按需加载，而不是一次性塞满。

### Agents (whitepaper)

<https://ia600601.us.archive.org/15/items/google-ai-agents-whitepaper/Newwhitepaper_Agents.pdf>

Google。用 Model / Tools / Reasoning / Agent Loop 这套词汇把 agent 的基本组件讲清楚，适合作为概念锚点。配套有 Kaggle 上的 [5-Day AI Agents Intensive Course](https://www.kaggle.com/learn-guide/5-day-agents)。

## Phase 2：Coding Agent 架构

### Dive into Claude Code: The Design Space of Today's and Future AI Agent Systems

<https://arxiv.org/abs/2604.14228>

论文。拆解 Claude Code 在 runtime、context compression、permission、session、skill、hook、subagent、worktree 上的设计取舍，而不是讲怎么写 prompt。

### Context Engineering for Coding Agents

<https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html>

Martin Fowler。软件工程视角，讨论 rules、skills、specs、workflow。

### Harness design for long-running application development

<https://www.anthropic.com/engineering/harness-design-long-running-apps>

Anthropic。讲长任务上的 harness 设计：上下文膨胀、"context anxiety"、如何让模型在长时间运行中保持连贯，以及前端设计和长时自治工程上的 frontier 表现。

### Scaling long-running autonomous coding

<https://cursor.com/blog/scaling-agents>

Cursor。在一个项目上同时跑几百个 agent、写出百万行代码后的复盘，重点是把扁平结构拆成 planner / worker 的流水线。

### Towards self-driving codebases

<https://cursor.com/blog/self-driving-codebases>

Cursor。描述一个能连续跑一周、贡献绝大部分 commit 的 agent harness，以及观察到的 agent 行为。

### Unrolling the Codex agent loop

<https://openai.com/index/unrolling-the-codex-agent-loop/>

OpenAI。Codex CLI 系列首篇，拆 agent loop 的内部结构和工程教训。

### Harness engineering: leveraging Codex in an agent-first world

<https://openai.com/index/harness-engineering/>

OpenAI。agent-first 思路下如何设计 harness，把 Codex 用作 code reviewer / SRE agent / 编码助手的取舍。

## Phase 3：Tool 与 Skill

### Writing Effective Tools for AI Agents

<https://www.anthropic.com/engineering/writing-tools-for-agents>

Anthropic。tool description、granularity、evaluation，以及如何让模型自己改进工具描述。

### Code Execution with MCP

<https://www.anthropic.com/engineering/code-execution-with-mcp>

Anthropic。重点不在 MCP，而在于为什么 tool 不该全部暴露给模型：lazy loading、state、context 属于 runtime design。

### Equipping agents for the real world with Agent Skills

<https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills>

Anthropic。Agent Skills 的设计动机：把领域专长打包成文件，让通用 agent 能做实际工作。

### Lessons from building Claude Code: How we use skills

<https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills>

Anthropic 内部。规模化使用数百个 skill 后的教训：什么时候 skill 有用、什么时候会拖后腿。

### Model Context Protocol

<https://modelcontextprotocol.io/>

MCP 官方文档与规范。理解 tool / data source 的接入协议；[架构概览](https://modelcontextprotocol.io/docs/learn/architecture) 和 [specification](https://modelcontextprotocol.io/specification/2025-06-18) 是核心。

## Phase 4：Evaluation

工具清单，方法论见 [ROADMAP](ROADMAP.md)。

### OpenAI Evals

<https://github.com/openai/evals>

通用的 LLM/agent evaluation 框架，支持自定义任务和自动评分。

### Inspect AI

<https://github.com/UKGovernmentBEIS/inspect_ai>

UK AI Security Institute 出的 agent benchmark，覆盖 tool use 和多轮任务。

### LangSmith

<https://smith.langchain.com/>

LangChain 的可观测与评估平台，trace、数据集管理、LLM judge、实验对比。

### Braintrust

<https://www.braintrust.dev/>

在线/离线评估、prompt A/B、回归测试。

### DeepEval

<https://github.com/confident-ai/deepeval>

Python 原生，类 pytest，适合放进 CI 自动评测。

### MLflow

<https://mlflow.org/>

含 GenAI evaluation，管理实验和指标。

### TruLens

<https://www.trulens.org/>

偏 RAG，反馈函数与质量评估。

## Phase 5：Observability

目前还没有成熟的 agent trace 标准，这一阶段更偏方向，没有固定清单。关注 agent trace 在每一步的记录、replay、diff、evaluation。思路见 [ROADMAP](ROADMAP.md)。

## Phase 6：源码

不要一次读完，一周读一个模块（skill / session / hook / subagent）。

### OpenHands

<https://github.com/All-Hands-AI/OpenHands>

开源 coding agent，runtime、sandbox、planner、checkpoint 的取舍都能在 PR 里看到。

### Aider

<https://github.com/Aider-AI/aider>

终端里的 AI pair programming，代码库地图和编辑策略值得读。

### OpenCode

<https://github.com/sst/opencode>

开源 coding agent，TypeScript 实现，架构较新。

---

## Resources

值得长期 follow 的来源。不是单篇文章。

### Engineering blogs

- **Anthropic Engineering** <https://www.anthropic.com/engineering> — agent、skill、context engineering 的一手出处，更新频繁。
- **Claude Blog** <https://claude.com/blog> — Claude Code 实践和产品侧的深度文章。
- **OpenAI Engineering** <https://openai.com/index/> — Codex 系列、harness、agent 工程文章（用 "Engineering" 标签筛）。
- **Cursor Blog** <https://cursor.com/blog> — 大规模自治编码、agent harness、模型训练的复盘。
- **Martin Fowler** <https://martinfowler.com/articles/exploring-gen-ai.html> — 把 agent 放回软件工程语境的系列。

### Docs & specs

- **Model Context Protocol** <https://modelcontextprotocol.io/> — tool / data source 接入协议的规范与 SDK。
- **Claude Code Docs** <https://code.claude.com/docs/en/features-overview> — 官方对 skills、hooks、subagents、plugin 的定义，读源码前的地图。
- **OpenAI Codex docs** <https://developers.openai.com/codex/> — AGENTS.md、memories、skills、MCP、subagents 在 Codex 里的角色。

### Newsletters & podcasts

- **Latent Space** <https://www.latent.space/> — swyx 维护，AI engineer 方向覆盖最密的 newsletter + 播客，访谈大多是 labs 一线工程师。

### Conferences

- **AI Engineer** <https://www.ai.engineer/> — agent / infra 方向的工程向会议，议题列表本身是一份很好的年度风向标。

---

## 书（收益有限）

对想设计 agent 平台的人，下面这些更像「生产工程入门」而非「agent 入门」，收益不如上面的博客和源码：

- *Building Applications with AI Agents*（O'Reilly）— 架构、tool、memory、context、reliability 的整体思维
- *AI Agents: The Definitive Guide*（O'Reilly）— 可靠性、部署、observability、成本、运维
- *AI Agents in Action*（O'Reilly）— 代码多，看具体实现
- *AI Agents and Applications*（Manning）— LangGraph、MCP、workflow
- *An Illustrated Guide to AI Agents*（O'Reilly）— 图多，建立整体框架
