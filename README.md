# Agent Systems Reading List

按系统设计问题组织的阅读清单。每条只列链接、标题、简介。方法论与评测细节见 [ROADMAP](ROADMAP.md)。

---

## Phase 1：建立直觉

### Building Effective AI Agents

<https://www.anthropic.com/research/building-effective-agents>

Anthropic。区分 workflow 和 agent，讲什么时候不该上 agent、tool design、planning、delegation。读完能理解 Claude Code 为什么是一个 while loop。

### Effective Context Engineering for AI Agents

<https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents>

Anthropic。把 skill、memory、RAG、context compression 统一进 context engineering 的框架；重点看 just-in-time context——Claude Code 如何用引用按需加载，而不是一次性塞满。

## Phase 2：Coding Agent 架构

### Dive into Claude Code: The Design Space of Today's and Future AI Agent Systems

<https://arxiv.org/abs/2604.14228>

论文。拆解 Claude Code 在 runtime、context compression、permission、session、skill、hook、subagent、worktree 上的设计取舍，而不是讲怎么写 prompt。

### Context Engineering for Coding Agents

<https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html>

Martin Fowler。软件工程视角，讨论 rules、skills、specs、workflow。

## Phase 3：Tool 与 Skill

### Writing Effective Tools for AI Agents

<https://www.anthropic.com/engineering/writing-tools-for-agents>

Anthropic。tool description、granularity、evaluation，以及如何让模型自己改进工具描述。

### Code Execution with MCP

<https://www.anthropic.com/engineering/code-execution-with-mcp>

Anthropic。重点不在 MCP，而在于为什么 tool 不该全部暴露给模型：lazy loading、state、context 属于 runtime design。

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

## 书（收益有限）

对想设计 agent 平台的人，下面这些更像「生产工程入门」而非「agent 入门」，收益不如上面的博客和源码：

- *Building Applications with AI Agents*（O'Reilly）— 架构、tool、memory、context、reliability 的整体思维
- *AI Agents: The Definitive Guide*（O'Reilly）— 可靠性、部署、observability、成本、运维
- *AI Agents in Action*（O'Reilly）— 代码多，看具体实现
- *AI Agents and Applications*（Manning）— LangGraph、MCP、workflow
- *An Illustrated Guide to AI Agents*（O'Reilly）— 图多，建立整体框架
