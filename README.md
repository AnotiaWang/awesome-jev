# Awesome Jev [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of applications, libraries, tools, and resources for [Jev](https://docs.typesafe.ai/introduction), TypeSafe's flagship [System One](https://docs.typesafe.ai/concepts/system-one) model.

**[English](README.md)** | **[简体中文](README_zh.md)**

> Send state and typed questions; get structured answers your code can use directly.

Jev launched in early access on 15 September 2026. This list is unofficial and not affiliated with [TypeSafe AI](https://typesafe.ai). Pull requests are welcome — the ecosystem is young and growing fast.

## Contents

- [What is Jev?](#what-is-jev)
- [Official](#official)
- [SDKs & Clients](#sdks--clients)
- [Applications](#applications)
- [Agent Tools](#agent-tools)
- [Research & Open Models](#research--open-models)
- [Cookbooks](#cookbooks)
- [Patterns](#patterns)
- [Articles](#articles)
- [Contribute](#contribute)

## What is Jev?

Large language models generate text. Jev does not. It evaluates typed *questions* against a *state* and returns values your code can branch on, sort by, and route with — plus calibrated probabilities and confidence.

| Question | Goal | Returns |
| --- | --- | --- |
| [Choice](https://docs.typesafe.ai/primitives/choice) | Pick one option from a list | `choice`, `probabilities`, `confidence` |
| [Score](https://docs.typesafe.ai/primitives/score) | Rate the state on a rubric | `score`, `probabilities`, `confidence` |
| [Noul](https://docs.typesafe.ai/primitives/noul) | Is this statement true? | `noul` (0–1) |

Questions in one request run in parallel against the same state. Atomic questions, composed in code.

## Official

- [TypeSafe](https://typesafe.ai) - Company homepage, waitlist, and product overview.
- [Documentation](https://docs.typesafe.ai/introduction) - Introduction, primitives, patterns, API, and SDKs. Start with the [quick start](https://docs.typesafe.ai/introduction/quickstart).
- [Playground](https://console.typesafe.ai/playground) - Paste a state, add questions, see typed answers in the browser.
- [API keys](https://console.typesafe.ai/settings/keys) - Dashboard for TypeSafe API keys (`TYPESAFE_API_KEY`).
- [HTTP API](https://docs.typesafe.ai/api) - `POST https://api.typesafe.ai/v1/systemone`.
- [Workflow evals](https://evals.typesafe.ai) - Published eval methodology and per-model results.
- [GitHub org](https://github.com/typesafe-ai) - Official open-source repositories.
- [Agent skill](https://docs.typesafe.ai/agent-skill) - Drop-in skill for Claude Code, Codex, and other coding agents ([`typesafe-ai/skills`](https://github.com/typesafe-ai/skills)).
- [Jev 1.13 jaggedness](https://docs.typesafe.ai/model-jaggedness/jev-1.13) - Known failure modes of the current public model.
- [Introducing System One Models and Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) - Launch post: architecture, pricing, Doom and Wikiracing demos, FAQ.

## SDKs & Clients

Official first, then community clients. Community packages are not affiliated with TypeSafe unless noted.

- [Python SDK](https://github.com/typesafe-ai/typesafe-sdk-python) - Official client. `pip install typesafe-sdk`. Docs: [Python SDK](https://docs.typesafe.ai/sdk/python).
- [JavaScript / TypeScript SDK](https://github.com/typesafe-ai/typesafe-sdk-js) - Official client. `npm install @typesafe-ai/sdk`. Docs: [JavaScript SDK](https://docs.typesafe.ai/sdk/javascript).
- [System One adapter (Python)](https://github.com/typesafe-ai/system-one-adapter-python) - Official drop-in `TypeSafeClient` replacement backed by LLM APIs, for comparing Jev against chat models on the same questions. `pip install system-one-adapter`.
- [Vercel AI SDK provider](https://ai-sdk.dev/providers/ai-sdk-providers/typesafe-ai) - `@ai-sdk/typesafe-ai` plus `experimental_evaluate`. Use `typeSafeAi.evaluationModel('jev-latest')` or the Gateway id `typesafe-ai/jev`.
- [Elixir SDK](https://github.com/nshkrdotcom/typesafe_sdk) - Community Hex package [`typesafe_sdk`](https://hex.pm/packages/typesafe_sdk) for `system_one` and model listing. Docs: [HexDocs](https://hexdocs.pm/typesafe_sdk).

## Applications

Open-source products and demos that put Jev in a real loop.

- [Jev Ultrafast](https://github.com/browser-use/jev-ultrafast) - Browser agent from [Browser Use](https://github.com/browser-use). Jev picks an operation and a DOM element in one request; a small LLM writes text only for `TYPE_TEXT`. Zürich → London on Google Flights in ~7s. Library, local inspector, and measurements included.
- [jev-browser](https://github.com/Ying-Kai-Liao/jev-browser) - Unofficial browser automation: an LLM plans the outcome, Jev decides each click/type on a Playwright snapshot (~300 ms/call). Ships as a library, CLI, and MCP server (`npx -y -p jev-browser jev-browser-mcp`).
- [Smart home assistant demo](https://docs.typesafe.ai/demos/smart-home) - Official interactive demo of [speculative fan-out](https://docs.typesafe.ai/patterns/fan-out): many questions in one call, code keeps the relevant answers, LLM only for splits and chit-chat. Source is slated for GitHub at release.

## Agent Tools

Tools that expose Jev to coding agents and MCP clients.

- [TypeSafe agent skill](https://github.com/typesafe-ai/skills) - Official skill: primitives, patterns, and how to structure evaluations. Claude Code: `claude plugin marketplace add typesafe-ai/skills` then `claude plugin install typesafe@typesafe-ai`. Other agents: `npx skills add typesafe-ai/skills --skill typesafe-ai`.
- [jev-mcp](https://github.com/jkudish/jev-mcp) - MCP server wrapping three cookbook patterns: `jev_verify` (citation check), `jev_screen` (prompt-injection / guardrails), `jev_find` (semantic ranking without embeddings). `npx -y github:jkudish/jev-mcp`.
- [jev-browser MCP](https://github.com/Ying-Kai-Liao/jev-browser) - Same project as above; MCP tools `browser_do`, `browser_check`, `browser_choose` so an agent can drive the page without reading full snapshots.

## Research & Open Models

Independent work inspired by Jev's interface. These are not TypeSafe models.

- [jevlike](https://github.com/vinnylarouge/jevlike) - Train a small one-pass scorer that maps context + N text options to a probability per option. Includes Doom / chess vision demos and a Wikispeedia next-click example. Explicitly *not* a reproduction of TypeSafe's architecture or RLCD.

## Cookbooks

Official, copy-pasteable workflows. Full index: [console cookbooks](https://console.typesafe.ai/docs/cookbooks) and [docs index](https://docs.typesafe.ai/llms.txt).

- [Parallel questions](https://docs.typesafe.ai/cookbooks/parallel_questions) - Batch many questions over one state; one call instead of N.
- [Line-by-line search](https://docs.typesafe.ai/cookbooks/semantic_find) - Score hundreds of line ids against a query with Choice + a Noul “does an answer exist?” check.
- [Re-ranking](https://docs.typesafe.ai/cookbooks/rerank_typesafe) - BM25 shortlist, then one TypeSafe question per query–candidate pair.
- [Guardrails for LLMs](https://docs.typesafe.ai/cookbooks/llm_guardrails) - Screen messages in and out of an LLM; threshold probabilities in code.
- [Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check) - Choice over whether a quote’s context supports the claim; confidence gates human review.
- [Classifying RAG passages](https://docs.typesafe.ai/cookbooks/classifying_rag_passages) - Keep, flag, or drop retrieved passages (contradiction, prompt injection) before the answering model.
- [Function calling](https://docs.typesafe.ai/cookbooks/function_calling) - Map natural-language requests onto ordinary typed functions with closed-set arguments.
- [Skill suggestion](https://docs.typesafe.ai/cookbooks/skill_suggestion) - Rank an agent skill catalog, then read only the top few.
- [Hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification) - Beam search over deep taxonomies with Choice probabilities.
- [SDE cascade](https://docs.typesafe.ai/cookbooks/sde_cascade) - Two-stage structured-data-extraction cascade (mini → verify → reasoning).
- [Date extraction](https://docs.typesafe.ai/cookbooks/date_extraction_cookbook) - Ask for named date parts, resolve and validate in code.
- [Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) - Regex candidates, then Jev selects the requested span.
- [Knowledge graph entity alignment](https://docs.typesafe.ai/cookbooks/entity_alignment) - Score merge / leave unlinked / send to a curator.
- [Autoresearch feature discovery](https://docs.typesafe.ai/cookbooks/autoresearch_feature_discovery) - Propose TypeSafe questions as numeric features for a supervised model.
- [Classification using confidence](https://docs.typesafe.ai/cookbooks/classification_using_confidence) - Report a fine label only when confidence is high; otherwise climb the hierarchy.
- [Structure recovery](https://docs.typesafe.ai/cookbooks/autoformat) - Reconstruct Markdown from de-formatted plain text.
- [Self-consistency: nouls](https://docs.typesafe.ai/cookbooks/consistency_noul_cookbook) / [choices](https://docs.typesafe.ai/cookbooks/consistency_choice_cookbook) - Route uncertain probabilities to review without hiding the raw values.

## Patterns

Architectural recipes from the docs.

- [Speculative fan-out](https://docs.typesafe.ai/patterns/fan-out) - Ask many questions, including ones that may not apply; filter in code.
- [Confidence-gated routing](https://docs.typesafe.ai/patterns/confidence-routing) - The answer is *what*; confidence is *whether to act*.
- [Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) - Atomic scores, weights you own in code.
- [Intent routing](https://docs.typesafe.ai/patterns/intent-routing) - Classify, then hand off to logic, a specialist LLM, or a human.

See also: [How to build with TypeSafe](https://docs.typesafe.ai/concepts/how-to-build-with-system-one), [use-case map](https://docs.typesafe.ai/concepts/use-case-map), [confidence](https://docs.typesafe.ai/confidence).

## Articles

Independent write-ups and news. Official posts live under [Official](#official).

- [Mini-Vibe Check: TypeSafe's Jev Judged Everything I’ve Written in 0.7 Seconds](https://every.to/also-true-for-humans/mini-vibe-check-typesafe-s-jev-judged-everything-i-ve-written-in-0-7-seconds) - Every's Mike Taylor runs Jev over a writing corpus.
- [AI That Doesn't Talk](https://ziplyne.agency/blog/ai-that-doesnt-talk-typesafe-jev-guide) - Practical guide: playground, Python/JS SDKs, raw HTTP, agent skill.
- [TypeSafe Jev: the First Decision-Only Model Class](https://www.developersdigest.tech/blog/typesafe-jev-system-one-models-release-guide-2026) - Release-week technical roundup: API, evals, adapter, skill.
- [He Says He Co-Invented ChatGPT. His New AI, Jev, Won't Write a Word](https://dev.to/gabrielanhaia/he-says-he-co-invented-chatgpt-his-new-ai-jev-wont-write-a-word-e3c) - Walkthrough of the Vercel AI SDK `experimental_evaluate` provider.
- [What Is Jev?](https://mohammedshehu.com/jev-typesafe-ai/) - Short practical intro with a Python ticket-triage example.
- [TypeSafe AI debuts model for machines that plays Doom](https://www.theregister.com/ai-and-ml/2026/09/16/typesafe-ai-debuts-model-for-machines-that-plays-doom/5296711) - News coverage of the launch.

## Related

- [typesafe-ai on PyPI](https://pypi.org/project/typesafe-ai/) - Community redirect shim. The real package is `typesafe-sdk`; this name was registered to block slopsquatting. Not affiliated with TypeSafe.

## Contribute

See [CONTRIBUTING.md](CONTRIBUTING.md). In short: open a pull request that adds a project with a link and a one-line description. Useful, interesting, and actually built on Jev (or clearly inspired by its interface).

## License

[CC0 1.0](LICENSE) — this list is dedicated to the public domain.
