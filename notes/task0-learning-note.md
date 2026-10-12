# Task 0：Agent 基础与环境准备

## 学习心得

以前我会把 Agent 简单理解成“接了大模型的聊天程序”。看完 Chapter 1，再亲自跑这次实验后，我觉得关键差别在于它不只是生成一句回复，还会根据任务决定要不要调用工具，并把工具结果接回后续判断里。LLM 负责决定下一步，Context 让它看到当前任务和已有结果，Tools 负责查询或执行，Harness 则把模型请求、工具调用和返回结果组织起来。

我先看了离线演示，之后用 Kimi K3 和 Moonshot Formula 做联网搜索。第一次把最大轮数设成 3，模型三轮都在请求搜索，程序到达上限后没有得到最终回答。后来把上限调到 8 重跑，模型第一轮调用了两次 `web_search`，两次 Formula 都执行成功；第二轮模型停止调用工具并给出了回答。这让我看到，工具返回并不等于任务完成，还要看模型有没有读完结果、作出最终回答。离线演示能让我看清轨迹，但只有后一次运行验证了真实模型和联网工具；两者不能混为一谈。

## 实验记录

- 环境：官方配套仓库 `.venv`，Python 3.12.13
- 模型：Moonshot `kimi-k3`
- 工具：官方 Moonshot Formula `moonshot/web-search:latest`
- 问题：搜索月之暗面官方资料，了解 Kimi API 的 Context Caching
- 首次尝试：`--max-steps 3`；模型三次都返回工具调用，达到迭代上限，未生成最终答案
- 重跑结果：`--max-steps 8`；2 次模型响应，2 次 `web_search` / Formula 执行均成功，第二次模型响应以 `stop` 结束并给出最终回答
- 结果文件：原始 JSON 保存在本机 `/tmp/task0-kimi-web-search-rerun.json`，没有上传；其中包含服务端加密搜索结果

在 `chapter1/web-search-agent` 目录运行。API Key 需在本机通过环境变量或本地 `.env` 配置，不要写进命令记录或提交到 Git：

```bash
../../.venv/bin/python main.py "请搜索月之暗面官方资料，简要说明 Kimi API 的上下文缓存（Context Caching）是什么，并根据搜索结果回答。" --provider kimi --model kimi-k3 --max-steps 8 --output /tmp/task0-kimi-web-search-rerun.json
```

## 环境运行截图

![Task 0 离线演示运行截图](../%E6%88%AA%E5%B1%8F2026-10-12%2014.43.35.png)

这张图记录的是离线演示，不是上面的联网重跑。它展示了 ReAct 轨迹，但不能作为真实搜索结果的证明。联网运行的最终答案和工具执行状态已在上方如实记录；API Key 和原始加密结果均未公开。

## 对本次课程内容的反馈

离线示例和真实联网实验分开说明很有帮助。希望实验说明也提醒大家区分“工具执行成功”和“Agent 最终完成任务”，这样排查迭代上限问题时更有方向。
