---
title: Harness 一线观察（一）：开源 Coding Agent 功能横评
date: 2026/08/31
updated: 2026/08/31
tags: [AI, 原创]
---

> 或许是全网规模最大、指标最丰富的客观 Harness 评测？

评测结果[见此](https://blog.alu.mk/harness-comparison/)。原始数据可以在[仓库](https://github.com/alumkal/harness-comparison)中查看。

## 样本选择

Harness 样本集来自 [bradAGI/awesome-cli-coding-agents](https://github.com/bradAGI/awesome-cli-coding-agents)。这里向其作者表示真诚的感谢。

本评测限于开源的交互式终端 harness，理由如下：

- 本评测通过阅读代码分析功能的实现情况，而这对闭源 harness 不可能
- 非交互式 harness 通常用于 benchmark，而非日常使用
- 不提供终端界面的 harness 通常面向非程序员群体，与本评测的视角本就不符

总共有 84 个 harness 符合上述要求，其中有 61 个达到了最高分的一半。

## 指标选择

指标的选择主要考虑开发者的需求，在此基础上尽量广泛。明确排除以下几项：

- headless mode：非交互式 harness 在本评测范围之外
- IDE、聊天软件接入：这些功能通常是 harness 的下游而非 harness 本身
- prompt cache 友好性：难以通过阅读代码判定
- 内置 prompt 的丰富程度：这些功能通常通过 skills 提供，本质上并非 harness 的一部分

具体指标由我和 Claude Fable 5 讨论得到，包含 9 大类（基础能力、上下文管理、后台任务、子智能体、易用性、安全、提示词套件、开发工具、模型供应商），37 小项。
另有两个主观评分的大类（可扩展性、前卫特性），不计入总分。

## 评测流程

评测分两阶段：粗测和校准。

粗测阶段为每个样本 harness 分配一个 subagent。这个 agent 会 clone 仓库，阅读代码并给出初步报告。
报告中为每小项客观指标给出 0/2/4（未实现/部分实现/完整实现）的分数，附带文字说明与代码证据。

校准阶段汇总每项指标的实际实现情况，决定 2 分和 4 分的锚点。
细化规则后，为每个大类分配一个 subagent，通读所有 harness 的对应章节后重新为每个 harness 的每项指标给出 0-5 的评分。
5 分用于奖励每个指标上最优秀的 1-2 个实现。

每个大类内计算平均分，以 9 个客观大类的平均分之和作为总分。

评测由 Claude Fable 5 监工，GPT 5.6 Luna 执行。

## 结果

前三名分别是 [oh-my-pi](https://github.com/can1357/oh-my-pi)、[qwen-code](https://github.com/QwenLM/qwen-code)、和 [codewhale](https://github.com/Hmbown/CodeWhale)。

Oh My Pi 总分最高，并且取得了最多的 5 分，唯一的明显短板是缺少沙箱机制。

Qwen Code 总分紧随其后，并且在子智能体大类中取得了惊人的满分成绩，但缺少 OAuth 支持（也就是说不能接入 Codex 等会员订阅）使其失去了相当一部分潜在用户。

CodeWhale（曾用名 DeepSeek TUI）总分第三且最为均衡，只有浏览器一个小项缺失。考虑到浏览器能力可以通过 MCP 获取，这个缺点基本可以忽略。

有趣的是，第一名和第三名都几乎是单人项目。
