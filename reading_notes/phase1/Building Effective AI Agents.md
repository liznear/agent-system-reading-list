---
source: https://www.anthropic.com/research/building-effective-agents
---

# Reading Notes: Building Effective AI Agents

这是比较早期的关于 agent 的文章了，但是它的大部分核心思想直到现在也是非常 make sense。我觉得最主要的几点是：

1. 怎么简单怎么来。
2. 思考怎么去增强 agent 的能力，比如提供合适的 context，或者设计合适的 tool。
3. 即使功能一样，不同的 tool interface 可能会有非常不一样的效果。

唯一可能有些区别的就是，现在大部分的 agent 都不再是一个确定的 workflow 了。也许 workflow 会适合一些有相对固定的流程的场景，但是现在的趋势更多是提供一个 skill 让 agent follow。workflow 也许更确定，更省钱，但是 agent + skill 更灵活。本质上就是从用代码定义 workflow，改成用自然语言定义 workflow（也就是 skill）。除非这个 workflow 依赖于确定的状态，比如报销流程，需要提交 -> 审批 -> 放款。如果完全交给 agent 做可能现在还不行。

## Random Thoughts

- 也许 agent 的最终使用方式就是定义一个 goal 和验收标准，agent 自己搞，一直搞到成功验收，或者你的钱花完。
- 也许可以整一个通用的 workflow engine platform。它提供了一个标准接口，允许用户根据业务定义每一个任务的状态和操作。比如报销任务。每个报销请求有自己当前的状态，一系列供 agent 调用的操作（可以改变状态，比如 ask_for_approval；也可以是为了获取 context， 比如 get_receipts），以及一个状态机。agent 通过执行各种操作让报销请求进入任何一个终止态。
