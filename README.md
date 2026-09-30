# Autonomous Research Agent — ReAct Loop + Live ArXiv Tool Calling

An agent that doesn't just answer in one shot. It decides for itself when to search, what to search next, when it has enough, and when to stop — with guardrails so it can't loop forever.

**Try it yourself → [Open in Colab](https://colab.research.google.com/) | Just run all cells and paste a free [Groq](https://console.groq.com) API key.**

## Problem
A basic RAG pipeline does one search and one answer. Real research isn't one-shot — it needs multiple rounds of searching, reading, and deciding if the evidence is enough. But letting an LLM call tools in a loop is risky: agents are famous for getting stuck making the same search forever when a query returns nothing useful.

## Approach
- Built a tool registry with `search_arxiv` (live ArXiv API) and `save_note` (scratchpad)
- Implemented a ReAct-style loop: the model thinks, calls a tool, reads the result, thinks again
- Added 3 safety guards against infinite loops:
  1. Hard max-step limit
  2. Blocking identical repeated tool calls (tells the model to change strategy instead)
  3. A forced final answer once the step limit is hit, so it never just dies mid-thought
- Logged a full trace table of every step so the reasoning is visible, not a black box
- Stress-tested it with a nonsense topic on purpose, to see if it would hallucinate papers that don't exist

## Result (from my own run)
| Metric | Result |
|---|---|
| Live topic — searches made | 4 real ArXiv API calls |
| Live topic — total agent steps | 7 (hit step limit, forced final answer kicked in) |
| Repeated calls blocked | 0 (agent varied its own queries) |
| Nonsense-topic stress test | Correctly reported "no papers found" — did not invent any |

Real topic tested: *"How can retrieval augmented generation reduce hallucinations in large language models?"* — the agent pulled real 2025 ArXiv papers on retrieval-augmented and uncertainty-driven RAG methods and synthesized a findings + research-gaps summary from them.

## Tech Stack
Python · Groq API (`openai/gpt-oss-120b`, function/tool calling) · ArXiv API · XML parsing · pandas

## Why this matters
This is Project 3 of a 5-project Agentic AI series, each one building on the last one's weak point: RAG → structured/audited outputs → autonomous tool-calling agents (this one) → multimodal (vision) RAG → real-time Corrective RAG with routing and caching.

---
**Akshat Kesharwani** — Fresher Data Analyst / Data Scientist
Portfolio: https://akshatkesharwani-info.github.io/ | GitHub: https://github.com/akshatkesharwani-info
