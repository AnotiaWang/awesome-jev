# Awesome Jev [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

精选的 [Jev](https://docs.typesafe.ai/introduction) 应用、库、工具与资料。Jev 是 TypeSafe 的旗舰 [System One](https://docs.typesafe.ai/concepts/system-one) 模型。

**[English](README.md)** | **[简体中文](README_zh.md)**

> 传入 state 和类型化问题，直接得到代码可用的结构化答案。

Jev 于 2026 年 9 月 15 日开放 early access。本列表为非官方整理，与 [TypeSafe AI](https://typesafe.ai) 无隶属关系。欢迎提交 PR——生态还很年轻，长得很快。

## 目录

- [Jev 是什么？](#jev-是什么)
- [官方资源](#官方资源)
- [社区](#社区)
- [SDK 与客户端](#sdk-与客户端)
- [应用](#应用)
- [Demo 与游戏](#demo-与游戏)
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
- [Vercel AI Gateway 上的 Jev](https://vercel.com/ai-gateway/models/jev) - 托管的 `typesafe-ai/jev`，走 AI SDK `evaluate`，不必等 TypeSafe waitlist。
- [Manifesto](https://typesafe.ai/manifesto) - 主张给软件用的机器原生智能，而不是聊天。
- [The Bitterest Lesson](https://typesafe.ai/blog/bitterest-lesson) - 优化错任务，规模再大也盖不过。
- [AI: too good to be true, too bad to be useful](https://typesafe.ai/blog/ai-too-good-to-be-true-too-bad-to-be-useful-typesafe-ai) - 自动化不该用偏好对齐过的聊天模型。

## 社区

- [Discord](https://discord.gg/typesafe) - TypeSafe 官方服务器。Builder demo 在 [Show and Tell](https://discord.com/channels/1483217544214085663/1483217545040232493)。
- [X @typesafeai](https://x.com/typesafeai) - 产品与研究动态。
- [LinkedIn](https://www.linkedin.com/company/typesafe-ai/) - 公司公告与招聘。

## SDK 与客户端

官方在前，社区在后。除非另行说明，社区包与 TypeSafe 无隶属关系。

- [Python SDK](https://github.com/typesafe-ai/typesafe-sdk-python) - 官方客户端。`pip install typesafe-sdk`。文档：[Python SDK](https://docs.typesafe.ai/sdk/python)。
- [JavaScript / TypeScript SDK](https://github.com/typesafe-ai/typesafe-sdk-js) - 官方客户端。`npm install @typesafe-ai/sdk`。文档：[JavaScript SDK](https://docs.typesafe.ai/sdk/javascript)。
- [System One adapter（Python）](https://github.com/typesafe-ai/system-one-adapter-python) - 官方提供的 `TypeSafeClient` 替身，后端走 LLM API，方便用同一套问题对比 Jev 与聊天模型。`pip install system-one-adapter`。
- [Vercel AI SDK provider](https://ai-sdk.dev/providers/ai-sdk-providers/typesafe-ai) - `@ai-sdk/typesafe-ai` + `experimental_evaluate`。可用 `typeSafeAi.evaluationModel('jev-latest')`，或 Gateway id `typesafe-ai/jev`。
- [Elixir SDK](https://github.com/nshkrdotcom/typesafe_sdk) - 社区 Hex 包 [`typesafe_sdk`](https://hex.pm/packages/typesafe_sdk)，支持 `system_one` 与模型列表。文档：[HexDocs](https://hexdocs.pm/typesafe_sdk)。
- [Jev（Elixir OTP）](https://github.com/dannote/jev) - Hex 包 [`jev`](https://hex.pm/packages/jev)：把 Jev 当成对等 GenServer，答案以消息到达再 pattern match，测试可以不碰网络
- [Ruby SDK](https://github.com/joshmn/typesafe-sdk) - 社区 Ruby 3.1+ 客户端：Noul / Choice / Score、重试、模型列表、线程安全连接池。没有异步客户端。
- [RubyLLM TypeSafe](https://github.com/kieranklaassen/ruby_llm-typesafe) - RubyLLM 2 的 TypeSafe provider，带离线模型元数据和类型化响应。
- [typesafe-ai-rails](https://github.com/GenieRobot/typesafe-ai-rails) - 基于官方 Python SDK 的 Rails 集成：配置、用量/成本遥测、可选置信度策略。
- [Rust SDK (typesafe-ai-rs)](https://github.com/gilljon/typesafe-ai-rs) - 独立的异步 / 阻塞 System One 客户端。
- [TypeSafe AI for Rust](https://github.com/Twister915/typesafe-ai) - 另一个 Rust 客户端：异步 + 阻塞传输、类型化响应、可观测重试。
- [typesafe-rs](https://github.com/AbdelStark/typesafe-rs) - 偏延迟的 Rust 传输 SDK，目标对齐官方客户端行为。
- [s1-rs](https://github.com/AbdelStark/s1-rs) - Rust derive 层：Choice / Score / Noul、类型化问题集、置信度门控、无网络测试。
- [Advocaat](https://github.com/pithings/advocaat) - 小型 TypeScript 客户端，给 chance / choice / score 打了 tagged helper。
- [Scala / ZIO SDK](https://github.com/jamesward/zio-typesafe-ai) - 社区 ZIO 客户端，带 noul / choice / score 的小 DSL。
- [.NET SDK](https://github.com/saibimajdi/typesafe-dotnet-sdk) - 社区客户端，类型化问题 + 带置信度的答案。
- [PHP SDK](https://github.com/Butochnikov/typesafe-sdk-php) - 非官方 PHP 客户端：类型化 DTO、Promise 与异常。下面的 Laravel 包基于它。
- [Laravel TypeSafe Jev](https://github.com/Butochnikov/laravel-typesafe-jev) - 非官方 Laravel 12/13 集成：配置、Facade、scoped DI，以及基于 PHP SDK 的 recording fake。
- [jev-go](https://github.com/Gaurav-Gosain/jev-go) - 非官方 Go 客户端，返回类型化判断与校准概率。`go get github.com/Gaurav-Gosain/jev-go`。
- [Stumble/jev-go](https://github.com/Stumble/jev-go) - 非官方零依赖 Go SDK，支持 TypeSafe 直连和 Vercel AI Gateway，并提供类型化问题、重试、交互式 CLI 和可安装的 agent skill
- [jevclient](https://github.com/AboveColin/jevclient) - 非官方异步 Python 客户端（`pip install jevclient`）。带 Noul / Choice / Score helper，与官方 `typesafe-sdk` 不是同一个包。
- [LlamaIndex Jev](https://github.com/WiktorB2004/llama-index-jev) - 非官方 LlamaIndex 重排序（`JevRerank`）与路由（`JevSingleSelector` / `JevMultiSelector`），基于官方 Python SDK
- [Swift SDK](https://github.com/ainame/swift-typesafe) - 非官方 Swift 6.4 客户端，对齐 Python SDK 0.6.0 API，含 Linux
- [TypeSafe AI Swift SDK](https://github.com/alterhq/typesafe-sdk-swift) - 非官方零依赖 Swift 6 客户端，支持 Choice / Score / Noul、严格并发、可配置鉴权与重试，以及无网络测试

## 应用

把 Jev 放进真实循环里的开源产品与 demo。

- [Jev Ultrafast](https://github.com/browser-use/jev-ultrafast) - [Browser Use](https://github.com/browser-use) 的浏览器 Agent。一次请求里由 Jev 选出操作和 DOM 元素；只有 `TYPE_TEXT` 才让小模型写字。Google Flights 苏黎世 → 伦敦约 7 秒。含库、本地 inspector 与测时。
- [JevBrowserExt](https://github.com/chy4pro/JevBrowserExt) - Jev Ultrafast 的 Chrome 扩展（Manifest V3）移植：Jev 一次请求同时选出操作和 DOM 元素，只有打字时才调用小文本模型，直接跑在用户自己的标签页里（OpenRouter / TypeSafe / Cloudflare 三种渠道）；附 17 个任务的 headless Chromium 测试套件和完整轨迹（仓库 docs/ 目录，同一套任务多轮 13–14/17）。
- [jev-ego](https://github.com/romaluev/jev-ego) - [ego lite](https://lite.ego.app/) 上的浏览器 Agent：一次 TypeSafe 请求选出操作和编号元素；面向 Agent 的 observe/act/suggest/step CLI
- [jev-browser](https://github.com/Ying-Kai-Liao/jev-browser) - 非官方浏览器自动化：LLM 规划目标，Jev 在 Playwright 快照上决定每次点击/输入（约 300 ms/次）。提供库、CLI 与 MCP 服务（`npx -y -p jev-browser jev-browser-mcp`）。
- [Jev Browser（Vlad Terin）](https://github.com/vlad-terin/jev-browser) - Agent skill + 运行时：Codex 规划，Jev 选元素，runner 执行并逐步校验。
- [typesafe-computer-use](https://github.com/awlevin/typesafe-computer-use) - macOS computer-use：OCR 屏幕，Jev 分类下一步动作再点击。约 $0.0002/步。
- [Mobile Jev](https://github.com/droidrun/mobile-jev) - [Mobilerun](https://mobilerun.ai) 上的 Android Agent：每次点击由 Jev 决定。打开 Uber，旧金山机场 → 金门大桥，约 21 秒 / 9 步到支付页。含实时 studio、CLI 与 traces。不需要 ADB。
- [Unclutter](https://github.com/kitze/unclutter) - Chrome / Firefox 扩展：Jev 标出页面上不重要的元素，本地按页面模板记住并在下次访问时藏起来。
- [TypeSafe AdBlock](https://github.com/realZachi/typesafe-adblock) - Chrome 扩展：Jev 判断 DOM 元素是不是广告再删掉。自带密钥、无后端。作者写明这是 demo，不是正经广告拦截器
- [HA-Jev](https://github.com/AboveColin/HA-Jev) - 非官方 Home Assistant 集成：把关于实体状态的类型化提问变成传感器与自动化动作；可直接选取实体、设备或区域来构造 state，并附带用量、成本与每日 token 预算实体
- [Every](https://github.com/sufianetaouil/every) - 语义代码搜索 CLI：对每个函数问 yes/no，按 Noul 概率排序。
- [blink](https://github.com/ellipsis-dev/blink) - 代码库搜索：一组 walker 并行走文件系统，由 Jev 判断哪个文件能回答自然语言查询
- [Jev Search](https://github.com/superagents-lab/jev-search) - 非官方网页搜索应用：用 Jev 的 Choice 和 Noul 判断选择来源、时间范围和候选查询词，再对 Search1API 返回的结果进行相关性排序
- [neo4jev](https://github.com/jexp/neo4jev) - Neo4j 图导航：每个节点上由 Jev 选择跟哪条关系走，并对 log 概率做 beam search
- [hono-jev-router](https://github.com/yusukebe/hono-jev-router) - 实验性 Hono 路由器：用自然语言描述路由，由 Jev 匹配进来的请求
- [sqlite3-jev](https://github.com/mattn/sqlite3-jev) - SQLite C 扩展：把 `jev_noul` / `jev_choice` / `jev_score` 做成 SQL 函数，只依赖 libcurl
- [jevql](https://github.com/kylemclaren/jevql) - 非官方类 psql 命令行工具与 Go/TS/Python SDK：无需扩展即可在原生 Postgres 中使用 `jev()` / `jev_prob` / `jev_choice` / `jev_score`，SQL 在服务端执行，剩余行由 Jev 批量判断，结果会缓存
- [jev-resilience](https://github.com/Vicente-MD/jev-resilience) - 非官方 Spring WebFlux starter：语义熔断器，用 Jev 抓 HTTP 200 里的静默失败
- [tripwire](https://github.com/noelzappy/tripwire) - 非官方 AI SDK middleware 与 OpenAI 兼容代理：约 100 ms 内对每条 LLM 回复做七项 Jev 检查，按置信度门控
- [ProgressGate](https://github.com/AshutoshVJTI/progressgate) - 检测 Agent 循环里的语义停滞：Jev 评判轨迹，代码返回 CONTINUE / WARN / REPLAN / HALT
- [jev-harness](https://github.com/AntonioCoppe/jev-harness) - 非官方生产层：策略、置信度门控、影子模式、配方和 eval CLI
- [jev-tree](https://github.com/reachjalil/jev-tree) - 在分类树上递归做 Choice，突破 Jev 单次最多 255 个选项的上限
- [jev-shell-history](https://github.com/mrnugget/jev-shell-history) - Fish 风格的 zsh 自动补全：输入时由 Jev 给近期历史排序
- [Supercov](https://github.com/supercorp-ai/supercov) - 面向编码 agent 的代码质量与测试覆盖率工具：Jev 给每个源文件打分，agent 就知道该先修哪里
- [Jev Review](https://github.com/devagrawal09/jev-review) - 分阶段代码审查工作流 + 本地 dashboard，由聚焦的 Jev 调用驱动。
- [Foreman](https://github.com/thruwire/foreman) - 软件工厂循环：Codex 写实现，Jev 独立判断是否做完、测试够不够、要不要人来看。
- [Jev Drone](https://github.com/RomanSlack/jev-drone) - MuJoCo 四旋翼：控制和安全留在代码里，Jev 做较慢的战术判断。
- [Jev Plays StarCraft](https://github.com/phyous/tsai-sc) - 原版星际争霸共享战役的结构化 state harness，带验证跑次和概率轨迹。
- [Jev × Civilization II](https://github.com/phyous/tsai-civ2) - 浏览器里跑原版文明 II；Jev 选帝国、城市、科研和单位动作。实验性，尚未验证通关
- [Jev Trade](https://github.com/aowang-ai/jev-trade) - Hyperliquid 实盘桌面：每个 tick 由 Jev 用 Choice 回答多空、开平或 hold、以及杠杆；下单和撤单由代码执行。默认 dry-run；配置私钥后会真下单。在线：[jev-trade.com](https://www.jev-trade.com/)。
- [Jev Trader](https://github.com/jarrodwatts/jev-trader) - 每个 Monad 区块对 Kuru 的 MON-USDC 下一笔买卖。在线 demo：[jev-trader.vercel.app](https://jev-trader.vercel.app/)。
- [Human Compiler](https://github.com/asfarsadewa/human-compiler) - 粘贴职场废话，Jev 打被动攻击 / 紧急感 / 信息密度，代码按 rustc 风格报诊断。在线：[human-compiler.asfarlab.fun](https://human-compiler.asfarlab.fun)。
- [JEVMETER](https://github.com/ChetasLua/jevmeter) - 给任意视频挂上实时 Jev 仪表：逐句打分，导出 16:9 成片。演示：[Chetaslua](https://x.com/chetaslua/status/2100473581251748216)。
- [jev-audio-beeper](https://github.com/santos-sanz/jev-audio-beeper) - 低延迟音频脏话检测：Jev 判定后 ffmpeg 在约 466 ms 内叠一声 beep，不改其余音轨。
- [jev-askable-arm](https://github.com/TarunTomar122/jev-askable-arm) - 仿真 Franka 上用英文目标做 zero-shot；Jev 把硬编码原语串起来。
- [jev-codex-router](https://github.com/0xNatoshi/jev-codex-router) - Codex 每轮路由：Jev 选模型、思考深度和速度模式。
- [jev-router](https://github.com/gargpratyush/jev-router) - Claude Code 与 Codex 的每轮路由：简单活走快档，难活走强档。`npm i -g jev-router`。
- [jev-secret-detection](https://github.com/teyhouse/jev-secret-detection) - 用 Jev 扫 diff 里的密钥，结果可复现。
- [commit-miner](https://github.com/devanshbatham/commit-miner) - 用 Jev 给 commit diff 分类的 Rust CLI：修 bug、安全/CWE、变更类型。可出 HTML/CSV 报告。
- [jev-eval-agent](https://github.com/vinilana/jev-eval-agent) - 早期 Jev 测试的公开评测 harness。
- [Jev Logs](https://github.com/reachjalil/jevlogs) - OpenTelemetry 日志分流：先让 Jev 打诊断价值和优先级，再决定要不要花 LLM。
- [Smart home assistant demo](https://docs.typesafe.ai/demos/smart-home) - 官方互动 demo，演示 [speculative fan-out](https://docs.typesafe.ai/patterns/fan-out)：一次请求问很多题，代码留下有用的答案，LLM 只负责拆分复合指令和闲聊。源码计划随发布上 GitHub。
- [jev.nvim](https://github.com/valentynkit/jev.nvim) - Neovim 插件：用 Treesitter 把缓冲区拆成函数，向每个函数提出一个自然语言问题让 Jev 打分，结果按概率排进 quickfix 列表。
- [jev-skip](https://github.com/valentynkit/jev-skip) - 浏览器扩展：读取 YouTube 字幕轨道，在片头结束前就把每段视频的赞助概率画到进度条上，不依赖众包数据库，据报告在 23 个视频上抓住了 SponsorBlock 77% 的赞助时长，每个视频约 0.0008 美元。

## Demo 与游戏

发布后 48 小时内涌出来的玩具、小站和实时 Agent。

- [Yes / No](https://yesno.coderai.dev) - 免登录 Noul demo。问一句，得到 yes / no / maybe，必要时联网检索。
- [Jev Tetris](https://jev-omega.vercel.app) - Jev 根据空洞、堆高、起伏选旋转和落点列。
- [Jev Pac-Man](https://jev-pacman.ephraimduncan.com) - 迷宫做成 JSON，每个路口由 Jev 选转向，实时玩。
- [typesafe-mario](https://github.com/fhshaik/typesafe-mario) - 从结构化模拟器状态玩超级马里奥。
- [jev-doom-agent](https://github.com/lukaske/jev-doom-agent) - 浏览器里的 Doom（Chocolate Doom WASM），空间状态 + 实时决策遥测。
- [jev-gomoku](https://github.com/mizchi/jev-gomoku) - MoonBit 客户端 + Jev 对打五子棋。文章：[jev 同士に五目並べで対戦させた](https://zenn.dev/mizchi/articles/jev-plays-gomoku)。
- [jev-t-rex-runner](https://github.com/joshlarsen/jev-t-rex-runner) - Chrome 小恐龙由 Jev 来跳。
- [snake-jev](https://github.com/siroccomask/snake-jev) - 贪吃蛇：每局几百次类型化转向决策。
- [Jev Guard](https://guard-jev.vercel.app) - 评论审核 playground。
- [Hollow Creek](https://hollow-creek-sigma.vercel.app) - 村庄 NPC 每个 tick *评判*你（做什么、对你什么感觉），而不是聊天。
- [Jev mood demo](https://jev-demo.vercel.app) - 长时间对它好或坏，结构化 state 跟踪心情。
- [Jev Room](https://jev-room.moe136231.chatgpt.site) - 一句话 → 六个房间设定。Jev 选，应用渲染。
- [TypeSafe Typewriter](https://typesafe-demo.val.run/) - Val Town 在线 demo：打字时 16 条类型化判断实时更新。发布帖：[Steve Krouse](https://x.com/stevekrouse/status/2100287368221659289)。
- [got-jev](https://github.com/phureewat29/got-jev) - 权力的游戏角色扮演：你是琼恩·雪诺。故事模型写下一场，Jev 回答他在哪、有多危险、该配什么音乐。
- [Little Airways](https://github.com/lbotinelly/jev-little-airways) - 玩具群岛空管：每架飞机只看见自己附近，Jev 判断备降 / 紧急 / 谁先落地，约 150 ms。
- [jev-plays-pokemon-red](https://github.com/valentynkit/jev-plays-pokemon-red) - 基于 PyBoy 的精灵宝可梦红版：路线和数值运算都由代码掌控，Jev 只在分支点做选择，每回合战斗都会记录一次用 Brier 分数对照 RAM 状态检验的濒死预测。
- [jev-canvas](https://github.com/gaborishka/jev-canvas) - 用语音和摄像头追踪的手指在 tldraw 画布上绘图；Jev 在每段实时转写上决定动作、目标和位置。支持英语和乌克兰语指令。

## Agent 工具

把 Jev 接到编程 Agent 与 MCP 客户端上的工具。

- [TypeSafe agent skill](https://github.com/typesafe-ai/skills) - 官方技能包：原语、模式、如何组织 evaluation。Claude Code：`claude plugin marketplace add typesafe-ai/skills`，再 `claude plugin install typesafe@typesafe-ai`。其他 Agent：`npx skills add typesafe-ai/skills --skill typesafe-ai`。
- [fast-jev-compaction](https://github.com/tamaratran/fast-jev-compaction) - Claude Code 插件 + npm 库：用 Jev 给工具调用打分并丢掉过时的，而不是把上下文摘要掉
- [SkillRanker](https://github.com/Dicklesworthstone/skillranker) - Rust CLI：根据当前会话上下文，让 Jev 给下一步该用哪个 agent skill 排序，带 Claude Code hook
- [Jevbridge](https://github.com/gamesonrblx/Jevbridge) - 非官方 ACP/MCP 适配器：把 Jev 的类型化判断和 computer use 接到 Codex、Claude、Grok、OpenCode 旁边
- [eve](https://github.com/vercel/eve) - Vercel 的 Agent 框架。实验性 `autoModel` 默认用 Gateway 上的 `typesafe-ai/jev`，从白名单里挑语言模型。
- [jev-mcp](https://github.com/jkudish/jev-mcp) - Node MCP，封装三条 cookbook：`jev_verify`（引文核验）、`jev_screen`（注入/护栏）、`jev_find`（无需 embedding 的语义排序）。`npx -y github:jkudish/jev-mcp`。
- [Jev MCP（Python）](https://github.com/blakestone-x/jev-mcp) - Python MCP：classify、score、check、match、screen。
- [Jev Review MCP](https://github.com/NiazMorshed2007/jev-review) - 本地优先的 MCP：Claude Code、Codex、Cursor、OpenCode 边写边拿 Jev 的结构化质量审查。与上面应用里的 [Jev Review](#应用) 不是同一个项目。
- [typesafe-mcp](https://github.com/itsmostafa/typesafe-mcp) - Go CLI + 单二进制 MCP，适配 Claude Desktop、Claude Code、Codex。
- [pi-typesafe](https://github.com/DevMortimer/pi-typesafe) - Pi 扩展：一份经同意的、密钥托管的 TypeSafe 客户端，批量 `typesafe_evaluate`，可离线测传输。
- [pi-jev](https://github.com/y0usaf/pi-jev) - Pi 扩展：影子模式工具调用门控、输出评判、类型化 `jev_ask`。
- [pi-warden](https://github.com/DevMortimer/pi-warden) - 基于 pi-typesafe 的 Pi 护栏：把判决当成 held tool result 而不是对话框；对照项目规则文件检查写入。
- [pi-jev-auto-mode](https://github.com/jomatsu/pi-jev-auto-mode) - Pi 自动模式：Jev 按语义批准 `bash` / `write` / `edit`，判断不了就拒绝。
- [Bicameral](https://github.com/AbdelStark/bicameral) - Pi 编程 harness：LLM 写代码，Jev 提供策略、循环检测和 review 的类型化反射。明确不是沙箱。
- [jev-pref](https://github.com/doeixd/jev-pref) - 把 AGENTS.md 里的偏好变成 Jev 驱动的 AI linter：在 `jev-pref.json` 定义项目语义审查规则，对 diff hunk、暂存文件或 PR 求值，并把结果反馈给编程 Agent。`npx jev-pref setup`。
- [ask-jev-skill](https://github.com/shantanugoel/ask-jev-skill) - Hermes skill：Agent 需要有界决策时去问 Jev。
- [jev-system-architect](https://github.com/samtay32/jev-system-architect) - 专门找脆弱语义逻辑、改写成 Choice / Score / Noul 边界的 skill。
- [augustus](https://github.com/24601/Augustus) - 设计判断 skill：把 Choice/Score/Noul 映射到决策理论、重排序、路由等经典方法，并给出组合代数、问题设计诊断和可证伪的验证门
- [jev-browser MCP](https://github.com/Ying-Kai-Liao/jev-browser) - 同上项目；MCP 工具 `browser_do`、`browser_check`、`browser_choose`，Agent 不必读完整页面快照也能操作页面。
- [jev-ego](https://github.com/romaluev/jev-ego) - 同上项目；在已打开的 ego lite TaskSpace 上 observe/act/suggest/step
- [jev-axi](https://github.com/shiftynick/jev-axi) - CLI 加 Claude Code、Codex hook：命令执行前先用 Jev 给危险性打分，并筛查抓取到的文本是否含提示注入，常规命令在本地判定、不发送任何内容
- [jev-belay](https://github.com/valentynkit/jev-belay) - Claude Code 的 Stop 钩子：先从对话记录里找证据，只有在文件改动且之后没有通过检查时才发起一次四问的 Jev 调用来核实"完成"，任何出错都放行。
- [jev-commit](https://github.com/valentynkit/jev-commit) - Git 预提交钩子：用一次 Jev 调用判断提交信息是否匹配暂存的改动，并检查调试残留、未提及的改动和凭据泄露，只有检测到凭据才会阻止提交。

## 研究与开源模型

受 Jev 接口启发的独立工作。它们不是 TypeSafe 的模型。

- [jevlike](https://github.com/vinnylarouge/jevlike) - 训练一个小的单次 scorer：上下文 + N 个文本选项 → 每个选项一个概率。含 Doom / 国际象棋视觉 demo，以及 Wikispeedia 下一跳例子。明确*不是* TypeSafe 架构或 RLCD 的复现。
- [openjev](https://github.com/TheoLeeCJ/openjev) - 家用 RTX 3090 能不能跑 Jev 风格的东西？直接读选项 logits，不生成文本。不是 TypeSafe 的模型。
- [PocketJev](https://github.com/NullPo-jp/PocketJev) - iPhone 端侧视觉判断：MLX + Qwen3-VL 选项 logits。相机 + 三选一，不生成文字，约 1 秒，不存照片。
- [jev-visual](https://github.com/hr98w/jev-visual) - Apple Silicon 上的教学向 Jev 风格视觉推理：共享多模态上下文、候选打分，含分拣厂 / Breakout / 手势 demo。不是 TypeSafe 的模型
- [jevmlx](https://github.com/bnsd55/jevmlx) - 给任意 MLX 模型做 Jev 风格并行约束决策：一次前向得到带概率的、按 schema 合法的 JSON
- [JEVfire](https://github.com/kikoncuo/jevfire) - CUDA LLM 上的 Jev 风格并行决策（vLLM），带浏览器马里奥 demo（本地约 71 ms/步）
- [decider](https://github.com/Mapika/decider) - 基于 Qwen3.5-2B 的微调：一次前向就给出类型化决策和校准概率。非官方，不是 TypeSafe 的架构。
- [LitJev](https://github.com/zhengxuyu/litjev) - Jev 的复现：把任意 Qwen 模型变成快速决策模型，提供与 Jev 相同的 `/v1/systemone` schema（Choice、Score、Noul），不训练、不生成回答文本。非官方，不是 TypeSafe 的模型。
- [PlayJev](https://github.com/OmniJev/PlayJev) - Qwen3.5-0.8B-Base 微调后从 448 px 画面玩十款浏览器小游戏：每步一次前向，概率直接从选项字母上读出，不生成任何文本。权重和十款游戏的浏览器 demo 都已公开。非官方，不是 TypeSafe 的模型。
- [typesafe-ai-benchmark](https://github.com/iammrduncan/typesafe-ai-benchmark) - 同一套 System One 问题，对比 Jev 与 Cerebras 上的 Qwen 3.8 27B。视频：[Shannon](https://x.com/iamMrDuncan/status/2100467548298899918)。
- [Jev Rerank Bench](https://github.com/anessbelbati/jev-rerank-bench) - 重排序对比：原始 provider 响应、打分代码、不确定区间、写明的局限。
- [Jev Spam Eval](https://github.com/bitnovus/jev-spam-eval) - 探索性零样本垃圾邮件研究，对照训练过的 TF-IDF 基线，并写了事后调参的 caveat。
- [Jev Phishing Bench](https://github.com/anisselbd/jev-phishing-bench) - 2000 封邮件：Jev 对 Claude Haiku 4.5 做点不点链接，带校准、延迟和成本。这里准确率是 Haiku 更高。
- [jev-agent-failure-benchmark](https://github.com/TokenTrim/jev-agent-failure-benchmark) - Who&When Pro（注入的 Agent 故障）：Jev 对强 LLM，预测是谁 / 哪一步 / 哪类错误。
- [jev-sec-bench](https://github.com/Gaurav-Gosain/jev-sec-bench) - 公开语料上的盲测：提示注入和漏洞代码检测，基于 jev-go。
- [Jev DSPy Lab](https://github.com/jmanhype/jev-dspy-lab) - 非官方 DSPy 配套评测：录制并重放 TypeSafe 调用，测量校准、选择性风险、置信度弃权、延迟、token 和建模成本。
- [jevcal](https://github.com/abhixhek/jevcal) - 非官方命令行工具：用你自己的标注数据按目标准确率为每个问题拟合置信度阈值，在留出集上验证，给出仍需回退到 LLM 的流量比例，并在 Jev 更新导致已锁定阈值失效时让 CI 失败
- [ASSAY-001](https://github.com/jourdanlabs/assay-001) - 独立预注册核验：Banking77 / CLINC150 上测 Jev 校准与类型安全。结论分裂，日志全公开。文章：[donttrustme.ai](https://donttrustme.ai/assay-001.html)
- [Jev search rerank eval](https://github.com/zhuyansen/jev-search-rerank-eval) - 9831 对标注：Jev rerank 对照 BM25 / bge-m3，并量化评委循环偏差。融合最好；Jev 单独打不过 embedding
- [吸烟史抽取评测](https://github.com/vclic/smoking-extraction-benchmark) - 1000 条合成病历：Jev 对 OpenAI structured outputs，比准确率、成本和延迟

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

独立实测、实验与报道。官方博文见 [官方资源](#官方资源)。

- [Mini-Vibe Check: TypeSafe's Jev Judged Everything I’ve Written in 0.7 Seconds](https://every.to/also-true-for-humans/mini-vibe-check-typesafe-s-jev-judged-everything-i-ve-written-in-0-7-seconds) - Every 的 Mike Taylor 用 Jev 扫过自己的写作语料。
- [TypeSafe AI debuts model for machines that plays Doom](https://www.theregister.com/ai-and-ml/2026/09/16/typesafe-ai-debuts-model-for-machines-that-plays-doom/5296711) - 发布新闻报道。
- [TypeSafeのJevを正しく驚く、それってLLMでできませんか？](https://zenn.dev/nwn/articles/824026c76116e0) - 用 Gemma 的 logit 并行复现 JSON 捷径，并在公开 Mario harness 上对比 Jev 与 LLM。
- [jev 同士に五目並べで対戦させた](https://zenn.dev/mizchi/articles/jev-plays-gomoku) - Jev 对打五子棋，带源码和耗时日志。
- [Jev: one judge call, or twelve dimension scores? I measured both on three tasks](https://agentjournal.dev/blog/llm-judge-vs-feature-extraction/) - 独立实测：三个分类任务上，每行一次直接提问 vs 12–14 个 Jev 维度加本地拟合权重，附 token 成本、置信区间与误报率。
- [Testing Jev on public and private data: classifier or filter?](https://amankumar.ai/blogs/jev-measured) - 16000 次调用对照 gpt-5.4-mini 与 gpt-5.6-luna：哪里赢、哪里崩、阈值怎么定

## 相关

- [PyPI 上的 typesafe-ai](https://pypi.org/project/typesafe-ai/) - 社区注册的重定向包。真正该装的是 `typesafe-sdk`；此名用于挡住 slopsquatting。与 TypeSafe 无隶属关系。

## 贡献

见 [CONTRIBUTING.md](CONTRIBUTING.md)。简而言之：开一个 PR，加上项目链接和一句话简介。项目应当有用、有趣，并且真正基于 Jev（或明确受其接口启发）。

## 许可证

[CC0 1.0](LICENSE) — 本列表贡献到公有领域。
