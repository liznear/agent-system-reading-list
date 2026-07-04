# Agent Systems Roadmap

面向「想设计 Agent 系统」而不是「想调用某个 Agent 框架」的方法论与路线图。按系统设计问题组织，而不是常见的 Prompt → RAG → Agent 入门路线。各阶段对应的阅读清单见 [README](README.md)。

生产级 Agent（Claude Code、Cursor、Codex）出现才一年多，目前的高收益资料主要来自工程团队的博客和设计文档、开源 Agent 的源码与 issue、以及 Evaluation / Observability / Runtime 方向的论文。这份清单把这些散落的资料重新归位。

不必顺序读完。挑当前最关心的系统设计问题跳读即可。

---

## Phase 1：建立直觉

**Building Effective AI Agents — Anthropic**
<https://www.anthropic.com/research/building-effective-agents>

讲 workflow 和 agent 的区别、什么时候不该上 agent、tool design、planning、delegation。读它不是为了学 API，而是为了理解 Claude Code 为什么长成一个 while loop。重点看 Workflow vs Agent、Tool Design 两节。

**Effective Context Engineering for AI Agents — Anthropic**
<https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents>

Skill、Memory、RAG、Context Compression 本质都属于 context engineering，而不是 prompt engineering。文章介绍了 Claude Code 如何通过引用（文件路径、查询、链接）按需加载上下文，而不是一次性塞进模型。重点看 "Just-in-time Context"。

## Phase 2：Coding Agent 架构

**Dive into Claude Code: The Design Space of Today's and Future AI Agent Systems — arXiv**
<https://arxiv.org/abs/2604.14228>

分析 Claude Code 为什么这样设计，而不是 Claude 怎么写 prompt。值得关注的维度：runtime、context compression、permission、session、skill、hook、subagent、worktree。

**Context Engineering for Coding Agents — Martin Fowler**
<https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html>

软件工程视角，讨论 rules、skills、specs、workflow，而不是 prompt。

## Phase 3：Tool 与 Skill

**Writing Effective Tools for AI Agents — Anthropic**
<https://www.anthropic.com/engineering/writing-tools-for-agents>

tool description、tool granularity、tool evaluation，以及如何用 Claude 自己帮助改进工具设计。做 tool 的人应该看。

**Code Execution with MCP — Anthropic**
<https://www.anthropic.com/engineering/code-execution-with-mcp>

重点不在 MCP 本身，而在于它解释了为什么 tool 不应该全部暴露给模型：lazy loading、state、context，这些都是 runtime design 的问题。

## Phase 4：Evaluation

评估对象已经从「模型」变成了「Agent 系统」。对一个 skill / agent，真正想知道的是：有没有帮助、帮助了多少、会不会引入副作用、值不值得长期维护。

先看一手资料，而不是框架：

- Anthropic 的 tool evaluation 文档——关注 evaluation dataset 怎么设计
- OpenAI Evals——关注什么叫 success、什么叫 win rate、怎么比较两个 agent

然后再看落地方案：LangSmith、Braintrust、DeepEval、MLflow、TruLens、Inspect AI。

### 分层评估

按重要性从高到低：

1. **Task Success** — 同一组任务分别 without skill / with skill 跑，比 pass rate。这是核心。
2. **Cost** — input/output tokens、tool calls、latency、花费，算 improvement / dollar。
3. **Trajectory** — skill 有没有改变 agent 的行为轨迹（tool count、files opened、retry count）。
4. **Latency** — wall clock / model latency / tool latency。
5. **Judge** — 没有标准答案的任务（README、PR、总结）才用 LLM judge 打分。
6. **Human Preference** — 双盲 A/B 给工程师选，win rate 比 judge 可信。
7. **Failure Analysis** — 不统计平均，而是分类失败原因（hallucination、wrong file、instruction ignored）。
8. **Skill Coverage** — skill 是否真正被触发（trigger rate、正确触发、误触发）。
9. **A/B Test** — 线上对照，看 completion、acceptance、undo、user rating。
10. **Trace Evaluation** — 给整条 trace 的每一步打分，2025 年后越来越流行。

### 判定 success 的优先级

- 能自动验证就自动验证（单元测试、集成测试、exit code、benchmark、compile）—— 最好
- 自己写 checker（JSON 字段校验、SQL 结果一致性）—— 次之
- LLM judge —— 最后手段，只用于没有标准答案的任务

### Skill 专有指标

trigger rate、正确 / 误触发、skill conflict、skill adoption（agent 是否真用了 skill 里的指令）、prompt expansion、context saved、retry reduction、tool reduction、cost saved。这些比单纯比成功率更能定位 skill 的价值和问题。

### 评测流程

```
Dataset (Task 1 / Task 2 / ...)
   ├── Run (No Skill)
   └── Run (With Skill)
          │
          ▼
   完整 Session / Trace      ← 工具调用、读写文件、重试、耗时、token
          │
          ▼
   Automatic Checker         ← 必要时才 LLM Judge
          │
          ▼
      Metrics → Compare Report
```

把 agent trace 当一等公民采集和比较，能回答的不只是「skill 有没有提升」，还有「为什么提升（或为什么没有）」。

### 给 Claude Skill 的四层框架

- **Outcome** — 任务是否成功（pass rate）
- **Efficiency** — token / cost / latency / tool calls 是否改善
- **Behavior** — trace 是否更短、更稳定，是否减少无效搜索或重试
- **Reliability** — 不同模型版本、随机种子、代码仓库上是否仍有效

Agent 随机性大，每个 task 跑 3–5 次（或 temperature=0），比平均成功率和方差。

## Phase 5：Observability

重点研究 agent trace，而不是 prompt。

```
User → Planner → Tool → Tool → Tool → Subagent → Memory → Done
```

对这条链上的每一步，要想清楚：该记录什么、怎么 replay、怎么 diff 两条 trace、怎么 evaluation。这是目前还没有成熟标准、值得投入的方向。

## Phase 6：源码

不要一次读完。一周读一个模块，比如：

1. Claude Skill
2. Session
3. Hook
4. Subagent

候选项目：OpenHands、Aider、OpenCode。真实项目里的架构取舍往往比书更有参考价值。

---

## 研究主题

长期跟踪的方向：

- **Agent Runtime** — while loop、状态机、恢复机制、任务调度，所有 agent 的核心
- **Context Engineering** — skill、memory、RAG、context compression 都归这里
- **Agent Evaluation** — 从 success rate 走向 trace-level evaluation
- **Agent Observability** — trace、metrics、replay、cost attribution，目前缺标准
- **Tool / Skill Design** — 高质量的 tool、skill、hook 让 agent 更可靠高效
- **Human–Agent Interaction** — Claude Code、Cursor、Codex 的交互设计值得拆解比较

## 书（收益有限，按需）

对想设计 agent 平台的人，下面这些更像「生产工程入门」而非「agent 入门」，收益不如上面的博客和源码：

- *Building Applications with AI Agents*（O'Reilly）— 架构、tool、memory、context、reliability 的整体思维
- *AI Agents: The Definitive Guide*（O'Reilly）— 可靠性、部署、observability、成本、运维
- *AI Agents in Action*（O'Reilly）— 代码多，看具体实现
- *AI Agents and Applications*（Manning）— LangGraph、MCP、workflow
- *An Illustrated Guide to AI Agents*（O'Reilly）— 图多，建立整体框架
