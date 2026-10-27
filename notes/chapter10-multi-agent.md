# Chapter 10：多 Agent 协作

这章对应我的 [Task 5 打卡笔记](task5-multi-agent.md)。两篇记录的是**同一次**官方实验 10-2，没有另外运行第二次模型调用。

我用官方 `chapter10/book-translation` 的四章教学样例，比较了多 Agent 分工和单 Agent 完成同一翻译任务。多 Agent 方案把工作交给 Manager、Glossary、Translation、Proofreading：术语先共享，逐章翻译后再审校、修订。实验使用 Python 3.12.13 和 Kimi `kimi-k3`。最终两种方案都生成了四章译文；多 Agent 的指定术语遵从率是 15/15，单 Agent 是 8/15，但全流程 token 用量分别是 24,211 和 7,080。术语分数来自程序的字符串匹配，不能当成人工翻译质量评分。

## 我的理解

这次实验让我明白，多 Agent 的好处不在“人多力量大”这句话，而在任务能不能真的拆开、每个角色能不能拿到恰好够用的材料。术语表、译文文件和审校报告是角色之间的交接物；Manager 要清楚谁负责什么，还得看审校意见决定是否返工。这样做让多章译文的用词更统一，也增加了调用成本。如果只有一小段翻译，我会先用单 Agent；遇到多章内容、明确的术语规范和独立审校需求，才考虑分工。

## 课程反馈

四章样例很适合看清协作流程。希望教材继续把教学样例和整书正式评测的边界写清楚，免得我把这次的四章结果误当成整书验收。

## 环境运行截图

下图是已保存的真实 Kimi 实验结果在本机浏览器中的回放。截图时没有重新请求 API，也不是运行当时的终端画面。

![第 10 章实验 10-2 多 Agent 翻译结果回放](images/task5/translation-result-replay.png)

更完整的实验步骤、对照表、限制和本机结果位置见 [Task 5：多 Agent 协作](task5-multi-agent.md)。参考：[官方 Chapter 10 实验代码](https://github.com/bojieli/ai-agent-book/tree/main/chapter10/book-translation)。
