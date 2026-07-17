---
title: Claude Code 命令使用技巧
published: 2026-05-19
description: Claude Code 终端命令大全
tags:
  - AI
  - claude-code
category: 学习心得
---

| 类别                 | 命令                | 功能说明              | 使用模版 / 示例                                     |
| ------------------ | ----------------- | ----------------- | --------------------------------------------- |
| **思考与决策支持**        | `/criticize`      | 批判性审查输出，找出漏洞或逻辑问题 | `/criticize 请指出以下内容中的逻辑漏洞：\n<内容>`             |
|                    | `/expand`         | 扩展想法或内容，增加细节      | `/expand 扩展以下段落，加入更多例子和细节：\n<内容>`             |
|                    | `/debate`         | 对立观点分析            | `/debate 赞成和反对远程工作的理由分别是什么？`                  |
| **编码 & 调试**        | `/refactor`       | 自动重构代码，提高可读性      | `/refactor 优化这段 Python 函数，提高可读性：\n<代码>`       |
|                    | `/test`           | 生成测试用例            | `/test 为这段 JS 代码生成单元测试：\n<代码>`                |
|                    | `/explain`        | 解释代码或概念           | `/explain 解释这段 SQL 在做什么：\n<代码>`               |
|                    | `/optimize`       | 提供性能优化建议          | `/optimize 优化以下算法，使运行更快：\n<代码>`               |
| **文案 & 写作**        | `/rewrite`        | 重写文本，调整语气或风格      | `/rewrite 用更礼貌专业的语气重写：\n<内容>`                 |
|                    | `/summarize`      | 摘要文本              | `/summarize 将以下文本总结为三段核心要点：\n<内容>`            |
|                    | `/translate`      | 翻译                | `/translate 翻译成英文：\n<内容>`                     |
|                    | `/tone`           | 调整语气              | `/tone 用友好幽默语气重写：\n<内容>`                      |
| **知识学习 & 提升**      | `/quiz`           | 生成小测验             | `/quiz 根据以下文章生成 10 道选择题：\n<内容>`               |
|                    | `/flashcards`     | 制作复习卡片            | `/flashcards 为以下内容生成复习卡：\n<内容>`               |
| **格式化与结构化输出**      | `/template`       | 按模板输出             | `/template 按以下结构输出：标题、背景、目标、方法\n<内容>`         |
|                    | `/table`          | 内容转表格             | `/table 将以下产品对比整理成表格：\n<内容>`                  |
|                    | `/json`           | 生成规范化 JSON        | `/json 将以下列表转换成 JSON 格式：\n<内容>`               |
| **多角度思考**          | `/perspectives`   | 从多个角色角度分析         | `/perspectives 作为工程师、产品、CEO 分析这个功能优缺点：\n<内容>` |
|                    | `/5whys`          | 根因分析              | `/5whys 为什么会出现这个 bug？\n<内容>`                  |
| **Prompt 设计 & 提升** | `/coach`          | 优化 prompt         | `/coach 优化以下 prompt，使其更精确：\n<prompt>`         |
|                    | `/chain`          | 分步执行              | `/chain 将完成商业计划书的任务拆分成 10 个步骤`                |
| **任务自动化辅助**        | `/schedule`       | 日程安排 / 提醒         | `/schedule 帮我安排接下来 3 天的复习计划`                  |
| **隐藏 / 特殊功能**      | `/btw`            | 插入并行小问题，不打断当前任务   | `/btw 请帮我回答一个小问题：\n<问题>`                      |
|                    | `/rewind`         | 回退 / 撤销           | `/rewind 回到上一轮对话`                             |
|                    | `/insights`       | 生成交互分析报告          | `/insights 分析过去一个月的使用习惯并生成 HTML 报告`           |
|                    | `/model opusplan` | 模型切换（内部模式）        | `/model opusplan 切换到 Sonnet 模式`               |
|                    | `/simplify`       | 三角度代码审查与优化        | `/simplify 优化以下代码：\n<代码>`                     |
|                    | `/branch`         | 从当前对话 fork 新分支    | `/branch 开启一个新分支对话`                           |
|                    | `/loop`           | 定时 / 循环任务         | `/loop 5m 检查任务状态，每 5 分钟执行一次`                  |
