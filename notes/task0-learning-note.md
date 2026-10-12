# Task 0：Agent 基础与环境准备

## 学习心得

以前我会把 Agent 简单理解成“接了大模型的聊天程序”。看完 Chapter 1，再亲自跑实验后，我觉得关键差别在于它不只是生成一句回复，还会根据任务决定要不要调用工具，并把工具结果接回后续判断里。LLM 负责决定下一步，Context 让它看到当前任务和已有结果，Tools 负责查询或执行，Harness 则把模型请求、工具调用和返回结果组织起来。

我先看了离线演示，之后用 Kimi K3 和 Moonshot Formula 做联网搜索。第一次把最大轮数设成 3，模型连续请求搜索，最后到达迭代上限，没有给出答案。这次把上限设为 8 后，模型仍然搜索了几轮才作答：日志里有 6 次模型请求，均返回 HTTP 200；调用了 5 次 `web_search`，Formula 工具调用也都成功，最后生成了回答。这个过程让我更直观地看到，工具成功返回和 Agent 完成任务是两回事；离线轨迹适合看控制流程，但不能代替真实模型和联网工具的验收。

## 实验记录

- 环境：官方配套仓库 `.venv`，Python 3.12.13
- 模型：Moonshot `kimi-k3`
- 工具：Moonshot Formula `moonshot/web-search:latest`
- 端点：`https://api.moonshot.cn/v1`
- 问题：搜索月之暗面官方资料，了解 Kimi API 的 Context Caching
- 首次尝试：`--max-steps 3`；连续调用搜索后到达迭代上限，未生成最终答案
- 本次重跑：`--max-steps 8`；6 次模型请求均返回 HTTP 200，5 次 `web_search`/Formula 调用成功，最终生成答案；轨迹共 13 步
- 本次原始结果保存在本机 `/tmp/task0-kimi-online-rerun.json`，其中含服务端加密搜索结果，没有上传

在 `chapter1/web-search-agent` 目录运行。先加载本机已有的环境配置，再运行实验；不要把 API Key 写进命令记录或提交到 Git：

```bash
set -a
source ../../chapter2/context-compression/.env
set +a
../../.venv/bin/python main.py "请搜索月之暗面官方资料，简要说明 Kimi API 的上下文缓存（Context Caching）是什么，并根据搜索结果回答。" --provider kimi --model kimi-k3 --max-steps 8 --output /tmp/task0-kimi-online-rerun.json
```

## 环境运行截图

### 离线演示

![Task 0 离线演示运行截图](../%E6%88%AA%E5%B1%8F2026-10-12%2014.43.35.png)

离线演示展示 ReAct 轨迹，使用预设示例结果，不代表真实联网搜索。

### Kimi K3 联网运行

![Task 0 Kimi K3 联网运行截图](../%E6%88%AA%E5%B1%8F2026-10-12%2021.14.44.png)

这张截图记录了真实联网运行的请求状态、工具调用次数和模型最终输出。截图里的 Context Caching 解释是模型生成的回答，具体价格、缓存机制等信息没有在本笔记中逐项独立核验，不把截图本身当作官方事实来源。API Key 和原始加密搜索结果均未公开。

## 对本次课程内容的反馈

离线示例和真实联网实验分开说明很有帮助。希望实验说明也提醒大家区分“工具执行成功”和“Agent 最终完成任务”，这样排查迭代上限问题时会更有方向。
