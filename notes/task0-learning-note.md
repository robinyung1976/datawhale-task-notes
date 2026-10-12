# Task 0：Agent 基础与环境准备

## 学习心得

一开始我觉得 Agent 大概就是“接了大模型、能回答问题的程序”。这次先看离线演示、再真正用 Kimi 跑联网搜索后，我才比较清楚地看到它们的区别。离线版是把一条现成轨迹放给我看；联网版里，Kimi 收到问题后会自己决定要不要查、查什么，再根据工具返回继续回答。

我这次问的是 Kimi API 的 Context Caching。运行时一共走了 3 轮决策，实际调用了 6 次 web_search，最后才生成答案。看这条轨迹时，我注意到工具结果不是“查完就结束”，而是会回到模型那里，模型再决定下一步。这让我对 ReAct 的“想、做、看”有了更具体的印象：LLM 负责判断，工具负责查信息，Harness 把调用和结果接起来。也因此，离线演示跑通只能说明我看懂了流程；确认 API 请求成功、并且轨迹里出现真实工具调用，才算验证了联网实验。

## 实验记录

- 环境：官方仓库 .venv，Python 3.13.13
- 离线实验：web-search-agent 的 offline-demo，输出预设 ReAct 轨迹并生成 demo.json
- 联网实验：Moonshot provider，kimi-k3；3 轮决策、6 次 web_search 调用，成功生成回答
- 提问：搜索月之暗面官方资料，了解 Kimi API 的 Context Caching
- 结果 JSON：保存在本机 /tmp/task0-kimi-web-search.json，没有上传到公开仓库
- 复现命令：在 chapter1/web-search-agent 目录中，先按官方 README 在本机设置 MOONSHOT_API_KEY，再运行：

    ../../.venv/bin/python main.py "请搜索月之暗面官方资料，简要说明 Kimi API 的上下文缓存（Context Caching）是什么，并根据搜索结果回答。" --provider kimi --model kimi-k3 --max-steps 3 --output /tmp/task0-kimi-web-search.json

## 环境运行截图

![Task 0 官方离线演示运行截图](../%E6%88%AA%E5%B1%8F2026-10-12%2014.43.35.png)

这张图是 offline-demo 的预设轨迹，不是联网运行截图。真实联网运行已在上面的记录中注明；截图没有展示 API Key，也没有把密钥提交到 GitHub。

## 对本次课程内容的反馈

先看离线轨迹、再跑真实联网搜索，这个顺序对我挺有帮助；希望 README 能继续把示例轨迹和真实 API 结果标得醒目一些，也提醒大家别把 Key 截进公开截图。
