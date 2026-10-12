# Task 0：Agent 基础与环境准备

## 学习笔记

我把 AI Agent 理解为由 LLM、Context 和 Tools 协同工作的系统：LLM 根据目标选择下一步，Context 提供任务要求、历史过程和工具结果，Tools 则让系统能够查询外部信息或执行操作。Chapter 1 的 ReAct 通过“思考—行动—观察”循环推进：模型提出行动，Harness 校验并调用工具，再把观察结果放回轨迹，供后续决策使用。

本次已在官方配套仓库中安装 Chapter 1 环境，并运行 `web-search-agent` 的 `offline-demo`，看到了完整的 ReAct 示例轨迹和结果文件。这验证了 Python 环境及演示流程，但离线轨迹和搜索结果是预设内容，不代表真实 LLM 调用或实时联网搜索。我目前没有 Moonshot API Key，因此尚未完成官方 Kimi 联网实验；Kimi 聊天会员也不能直接替代开放平台 API 凭证。后续会在具备对应 API 条件后再验证真实调用，并区分实验日志与真实搜索证据。

## 实验记录

- 环境：官方仓库 `.venv`，Python 3.13.13
- 实验：Chapter 1 `web-search-agent`，`offline-demo`
- 结果：成功输出示例 ReAct 轨迹，并生成 `demo.json`
- 边界：离线示例，不是实时搜索，也没有调用真实 LLM API

## 环境运行截图

![Task 0 官方离线演示运行截图](../%E6%88%AA%E5%B1%8F2026-10-12%2014.43.35.png)

截图展示的是官方 `offline-demo` 的示例轨迹，不是真实 LLM API 调用或实时搜索结果。
