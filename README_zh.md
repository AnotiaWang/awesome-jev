# Awesome Jev [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

精选的 [Jev](https://docs.typesafe.ai/introduction) 应用、库、工具与资料。Jev 是 TypeSafe 的旗舰 [System One](https://docs.typesafe.ai/concepts/system-one) 模型。

**[English](README.md)** | **[简体中文](README_zh.md)**

> 传入 state 和类型化问题，直接得到代码可用的结构化答案。

Jev 于 2026 年 9 月 15 日开放 early access。本列表为非官方整理，与 [TypeSafe AI](https://typesafe.ai) 无隶属关系。欢迎提交 PR——生态还很年轻，长得很快。

## 目录

- [Jev 是什么？](#jev-是什么)
- [官方资源](#官方资源)
- [SDK 与客户端](#sdk-与客户端)
- [应用](#应用)
- [Agent 工具](#agent-工具)
- [研究与开源模型](#研究与开源模型)
- [Cookbook](#cookbook)
- [模式](#模式)
- [文章](#文章)
- [贡献](#贡献)

## Jev 是什么？

大语言模型生成文本。Jev 不生成文本。它针对一份 *state* 评估类型化 *问题*，返回代码可以直接分支、排序、路由的值，并附带校准概率与置信度。

| 问题类型 | 用途 | 返回值 |
| --- | --- | --- |
| [Choice](https://docs.typesafe.ai/primitives/choice) | 从给定选项中选一个 | `choice`、`probabilities`、`confidence` |
| [Score](https://docs.typesafe.ai/primitives/score) | 按量规给 state 打分 | `score`、`probabilities`、`confidence` |
| [Noul](https://docs.typesafe.ai/primitives/noul) | 这句话为真吗？ | `noul`（0–1） |

同一次请求里的问题会对同一份 state 并行求值。问题尽量原子，组合逻辑写在你的代码里。

## 官方资源

- [TypeSafe](https://typesafe.ai) - 官网、候补名单与产品介绍。
- [文档](https://docs.typesafe.ai/introduction) - 入门、原语、模式、API 与 SDK。建议从 [Quick start](https://docs.typesafe.ai/introduction/quickstart) 开始。
- [Playground](https://console.typesafe.ai/playground) - 粘贴 state、添加问题，在浏览器里看类型化结果。
- [API keys](https://console.typesafe.ai/settings/keys) - TypeSafe API 密钥控制台（`TYPESAFE_API_KEY`）。
- [HTTP API](https://docs.typesafe.ai/api) - `POST https://api.typesafe.ai/v1/systemone`。
- [Workflow evals](https://evals.typesafe.ai) - 公开的评测方法与各模型结果。
- [GitHub 组织](https://github.com/typesafe-ai) - 官方开源仓库。
- [Agent skill](https://docs.typesafe.ai/agent-skill) - 给 Claude Code、Codex 等编程 Agent 用的技能包（[`typesafe-ai/skills`](https://github.com/typesafe-ai/skills)）。
- [Jev 1.13 jaggedness](https://docs.typesafe.ai/model-jaggedness/jev-1.13) - 当前公开模型已知的毛边与失败模式。
- [Introducing System One Models and Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) - 发布博文：架构、定价、Doom / Wikiracing demo、FAQ。

## SDK 与客户端

官方在前，社区在后。除非另行说明，社区包与 TypeSafe 无隶属关系。

- [Python SDK](https://github.com/typesafe-ai/typesafe-sdk-python) - 官方客户端。`pip install typesafe-sdk`。文档：[Python SDK](https://docs.typesafe.ai/sdk/python)。
- [JavaScript / TypeScript SDK](https://github.com/typesafe-ai/typesafe-sdk-js) - 官方客户端。`npm install @typesafe-ai/sdk`。文档：[JavaScript SDK](https://docs.typesafe.ai/sdk/javascript)。
- [System One adapter（Python）](https://github.com/typesafe-ai/system-one-adapter-python) - 官方提供的 `TypeSafeClient` 替身，后端走 LLM API，方便用同一套问题对比 Jev 与聊天模型。`pip install system-one-adapter`。
- [Vercel AI SDK provider](https://ai-sdk.dev/providers/ai-sdk-providers/typesafe-ai) - `@ai-sdk/typesafe-ai` + `experimental_evaluate`。可用 `typeSafeAi.evaluationModel('jev-latest')`，或 Gateway id `typesafe-ai/jev`。
- [Elixir SDK](https://github.com/nshkrdotcom/typesafe_sdk) - 社区 Hex 包 [`typesafe_sdk`](https://hex.pm/packages/typesafe_sdk)，支持 `system_one` 与模型列表。文档：[HexDocs](https://hexdocs.pm/typesafe_sdk)。

## 应用

把 Jev 放进真实循环里的开源产品与 demo。

- [Jev Ultrafast](https://github.com/browser-use/jev-ultrafast) - [Browser Use](https://github.com/browser-use) 的浏览器 Agent。一次请求里由 Jev 选出操作和 DOM 元素；只有 `TYPE_TEXT` 才让小模型写字。Google Flights 苏黎世 → 伦敦约 7 秒。含库、本地 inspector 与测时。
- [jev-browser](https://github.com/Ying-Kai-Liao/jev-browser) - 非官方浏览器自动化：LLM 规划目标，Jev 在 Playwright 快照上决定每次点击/输入（约 300 ms/次）。提供库、CLI 与 MCP 服务（`npx -y -p jev-browser jev-browser-mcp`）。
- [Smart home assistant demo](https://docs.typesafe.ai/demos/smart-home) - 官方互动 demo，演示 [speculative fan-out](https://docs.typesafe.ai/patterns/fan-out)：一次请求问很多题，代码留下有用的答案，LLM 只负责拆分复合指令和闲聊。源码计划随发布上 GitHub。

## Agent 工具

把 Jev 接到编程 Agent 与 MCP 客户端上的工具。

- [TypeSafe agent skill](https://github.com/typesafe-ai/skills) - 官方技能包：原语、模式、如何组织 evaluation。Claude Code：`claude plugin marketplace add typesafe-ai/skills`，再 `claude plugin install typesafe@typesafe-ai`。其他 Agent：`npx skills add typesafe-ai/skills --skill typesafe-ai`。
- [jev-mcp](https://github.com/jkudish/jev-mcp) - MCP 服务，封装三条 cookbook：`jev_verify`（引文核验）、`jev_screen`（注入/护栏）、`jev_find`（无需 embedding 的语义排序）。`npx -y github:jkudish/jev-mcp`。
- [jev-browser MCP](https://github.com/Ying-Kai-Liao/jev-browser) - 同上项目；MCP 工具 `browser_do`、`browser_check`、`browser_choose`，Agent 不必读完整页面快照也能操作页面。

## 研究与开源模型

受 Jev 接口启发的独立工作。它们不是 TypeSafe 的模型。

- [jevlike](https://github.com/vinnylarouge/jevlike) - 训练一个小的单次 scorer：上下文 + N 个文本选项 → 每个选项一个概率。含 Doom / 国际象棋视觉 demo，以及 Wikispeedia 下一跳例子。明确*不是* TypeSafe 架构或 RLCD 的复现。

## Cookbook

官方可直接照着改的工作流。完整目录：[console cookbooks](https://console.typesafe.ai/docs/cookbooks) 与 [文档索引](https://docs.typesafe.ai/llms.txt)。

- [Parallel questions](https://docs.typesafe.ai/cookbooks/parallel_questions) - 对同一份 state 批量提问；一次调用代替 N 次。
- [Line-by-line search](https://docs.typesafe.ai/cookbooks/semantic_find) - 用 Choice 给几百行 id 打分，再用 Noul 检查「到底有没有答案」。
- [Re-ranking](https://docs.typesafe.ai/cookbooks/rerank_typesafe) - BM25 短名单，再对每个 query–候选对问一次 TypeSafe。
- [Guardrails for LLMs](https://docs.typesafe.ai/cookbooks/llm_guardrails) - 筛 LLM 的入站/出站消息；概率阈值写在代码里。
- [Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check) - Choice 判断引文上下文是否支撑主张；低置信度交给人工。
- [Classifying RAG passages](https://docs.typesafe.ai/cookbooks/classifying_rag_passages) - 在回答模型之前保留、标记或丢掉检索段落（矛盾、注入等）。
- [Function calling](https://docs.typesafe.ai/cookbooks/function_calling) - 把自然语言请求映射到普通类型化函数与闭集参数。
- [Skill suggestion](https://docs.typesafe.ai/cookbooks/skill_suggestion) - 给 Agent 技能目录排序，只细读前几名。
- [Hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification) - 在深层分类树上用 Choice 概率做 beam search。
- [SDE cascade](https://docs.typesafe.ai/cookbooks/sde_cascade) - 两阶段结构化抽取级联（mini → 校验 → 推理）。
- [Date extraction](https://docs.typesafe.ai/cookbooks/date_extraction_cookbook) - 先问文档里点名的日期部件，再在代码里解析校验。
- [Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) - 正则出候选，再让 Jev 选出目标片段。
- [Knowledge graph entity alignment](https://docs.typesafe.ai/cookbooks/entity_alignment) - 打分：合并 / 不链接 / 交给策展人。
- [Autoresearch feature discovery](https://docs.typesafe.ai/cookbooks/autoresearch_feature_discovery) - 把 TypeSafe 问题当成数值特征，喂给监督学习。
- [Classification using confidence](https://docs.typesafe.ai/cookbooks/classification_using_confidence) - 置信度够才报细分类，否则上爬一层。
- [Structure recovery](https://docs.typesafe.ai/cookbooks/autoformat) - 从丢掉格式的纯文本重建 Markdown。
- [Self-consistency: nouls](https://docs.typesafe.ai/cookbooks/consistency_noul_cookbook) / [choices](https://docs.typesafe.ai/cookbooks/consistency_choice_cookbook) - 不确定的概率走人工审核，同时保留原始数值。

## 模式

文档里的架构配方。

- [Speculative fan-out](https://docs.typesafe.ai/patterns/fan-out) - 一次问很多题（包括可能用不上的），在代码里过滤。
- [Confidence-gated routing](https://docs.typesafe.ai/patterns/confidence-routing) - 答案告诉你*是什么*；置信度告诉你*要不要动手*。
- [Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) - 原子分数，权重由你的代码掌控。
- [Intent routing](https://docs.typesafe.ai/patterns/intent-routing) - 先分类，再交给确定性逻辑、专用 LLM 或人。

另见：[How to build with TypeSafe](https://docs.typesafe.ai/concepts/how-to-build-with-system-one)、[用例地图](https://docs.typesafe.ai/concepts/use-case-map)、[置信度](https://docs.typesafe.ai/confidence)。

## 文章

独立评测与报道。官方博文见 [官方资源](#官方资源)。

- [Mini-Vibe Check: TypeSafe's Jev Judged Everything I’ve Written in 0.7 Seconds](https://every.to/also-true-for-humans/mini-vibe-check-typesafe-s-jev-judged-everything-i-ve-written-in-0-7-seconds) - Every 的 Mike Taylor 用 Jev 扫过自己的写作语料。
- [AI That Doesn't Talk](https://ziplyne.agency/blog/ai-that-doesnt-talk-typesafe-jev-guide) - 实践指南：Playground、Python/JS SDK、裸 HTTP、agent skill。
- [TypeSafe Jev: the First Decision-Only Model Class](https://www.developersdigest.tech/blog/typesafe-jev-system-one-models-release-guide-2026) - 发布周技术综述：API、评测、adapter、skill。
- [He Says He Co-Invented ChatGPT. His New AI, Jev, Won't Write a Word](https://dev.to/gabrielanhaia/he-says-he-co-invented-chatgpt-his-new-ai-jev-wont-write-a-word-e3c) - Vercel AI SDK `experimental_evaluate` provider 的走读。
- [What Is Jev?](https://mohammedshehu.com/jev-typesafe-ai/) - 短文入门，带 Python 工单分流示例。
- [TypeSafe AI debuts model for machines that plays Doom](https://www.theregister.com/ai-and-ml/2026/09/16/typesafe-ai-debuts-model-for-machines-that-plays-doom/5296711) - 发布新闻报道。

## 相关

- [PyPI 上的 typesafe-ai](https://pypi.org/project/typesafe-ai/) - 社区注册的重定向包。真正该装的是 `typesafe-sdk`；此名用于挡住 slopsquatting。与 TypeSafe 无隶属关系。

## 贡献

见 [CONTRIBUTING.md](CONTRIBUTING.md)。简而言之：开一个 PR，加上项目链接和一句话简介。项目应当有用、有趣，并且真正基于 Jev（或明确受其接口启发）。

## 许可证

[CC0 1.0](LICENSE) — 本列表贡献到公有领域。
