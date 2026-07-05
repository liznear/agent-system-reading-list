---
source: https://www。anthropic。com/engineering/effective-context-engineering-for-ai-agents
---

# Reading Notes: Effective Context Engineering for AI Agents


这几年出来了很多新名词， 比如更早之前的 prompt engineering。 在 2026 年又有了 loop engineering。 我感觉 context engineering 可能是最能持久的。 在不改变模型的前提下， context 几乎是我们唯一能影响模型输出的手段了。 不管是 Tool / MCP / SKILL， 本质上都是为 agent 提供的 context 的手段 (或者让 agent 改变外部状态)。

这篇文章给出了一些 high-level 的原则

1. Context 是有限的资源， 需要管理。
2. 在具体和通用之间找到平衡。

也有一些实操上的建议， 比如

1. 让你的系统更加 intuitive，比如测试相关的就放在测试目录。 这其实和人类接触新 code base 的偏好一样。 一个遵守规范的 codebase 更容易被人和 AI 理解。
2. compact / note taking / sub-agent 的作用和使用场景

## Random Thoughts

- 我自己做了一个小工具， 会让 agent 提前 plan 好任务。 在每一个子任务完成之后自动做一个 mini compaction， 只 compact 这个子任务的内容， 子任务之前的上下文依然保留。 直觉上觉得会减少 context 的占用， 但是我发现它会出现多某个文件多次的情况， cost 方面虽然减少了 input size， 但是增加了 compact 的开销， 我也不确定实际效果如何。 需要 benchmark 一下。
