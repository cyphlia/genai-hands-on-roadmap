# The Hands-On GenAI Roadmap
### Learn Generative AI by Building — From First Principles to Production Systems

**Philosophy of this document:** You already have theory. What's missing is *muscle memory* — the kind you only get by writing the code yourself, hitting real errors, and debugging them without an AI assistant doing it for you. Every phase below follows the same loop:

> **Learn the concept → Read the primary source → Build the smallest working version yourself → Break it on purpose → Rebuild it better → Ship a real project**

A hard rule for using this roadmap: **write every line of code by hand at least once per concept.** You can use autocomplete for boilerplate later, but the first implementation of anything new (a tokenizer, an attention head, a RAG pipeline, an agent loop) should come from your own fingers and your own debugging. That's the entire point — copy-pasting a working solution teaches you what code looks like, not how to think when it breaks.

---

## Table of Contents

1. [How to Use This Roadmap](#how-to-use)
2. [Prerequisites Check](#prerequisites)
3. [The Roadmap at a Glance](#roadmap-glance)
4. [Phase 0 — Environment & Tooling Setup](#phase-0)
5. [Phase 1 — LLM Foundations From Scratch](#phase-1)
6. [Phase 2 — Working With LLM APIs Like an Engineer](#phase-2)
7. [Phase 3 — Prompt Engineering as a Discipline](#phase-3)
8. [Phase 4 — Embeddings & Semantic Search](#phase-4)
9. [Phase 5 — Retrieval-Augmented Generation (RAG)](#phase-5)
10. [Phase 6 — Agents & Tool Use](#phase-6)
11. [Phase 7 — Fine-Tuning & Model Customization](#phase-7)
12. [Phase 8 — Multimodal GenAI](#phase-8)
13. [Phase 9 — Evaluation, Safety & Observability](#phase-9)
14. [Phase 10 — Production Systems & Scaling](#phase-10)
15. [Capstone Project Ideas](#capstone)
16. [Master Resource List](#resources)
17. [Suggested Weekly Study Structure](#study-structure)

---

<a name="how-to-use"></a>
## 1. How to Use This Roadmap

- **Don't skip phases**, even if you "know" the theory. The projects are designed to expose gaps that reading alone hides.
- **Every phase has:** core concepts → why it matters → a primary resource → a hands-on project → a "break it" exercise → a self-check.
- **The "break it" exercises are not optional.** Deliberately introducing bugs and failures is how you build the debugging intuition that separates people who understand GenAI from people who can only prompt it.
- **Keep a build log.** A simple markdown file or repo README where, after every project, you write: what you built, what broke, how you fixed it, what you'd do differently. This becomes your own reference material — better than any tutorial, because it's yours.
- **No-AI-assistance rule:** for the first pass of each project, don't use Copilot/ChatGPT/Claude to write the implementation. Use official docs, source code, and Stack Overflow/GitHub issues instead. You can use an AI assistant afterward to *review* your code or explain an error message you're stuck on for more than 30–45 minutes — but the first attempt is yours.

---

<a name="prerequisites"></a>
## 2. Prerequisites Check

Before starting, you should be comfortable with:

- **Python** (functions, classes, virtual environments, pip, basic OOP) — if rusty, spend 3–4 days on [Python Official Tutorial](https://docs.python.org/3/tutorial/) or [Automate the Boring Stuff](https://automatetheboringstuff.com/).
- **Command line basics** (cd, ls, grep, curl, environment variables, git).
- **Basic linear algebra** (vectors, matrices, dot products) and **basic probability** (distributions, conditional probability) — [3Blue1Brown's Essence of Linear Algebra](https://www.3blue1brown.com/topics/linear-algebra) is the best 3-hour investment you can make.
- **REST APIs and JSON** — you should know what a POST request, a header, and a JSON payload are.

If any of these are shaky, spend a week here first. Everything downstream assumes this baseline.

---

<a name="roadmap-glance"></a>
## 3. The Roadmap at a Glance

```
Phase 0  → Environment & Tooling            (2-3 days)
Phase 1  → LLM Foundations From Scratch     (2-3 weeks)   [the "hard part" — do not skip]
Phase 2  → Working With LLM APIs            (1 week)
Phase 3  → Prompt Engineering               (1 week)
Phase 4  → Embeddings & Semantic Search     (1-2 weeks)
Phase 5  → RAG Systems                      (2-3 weeks)
Phase 6  → Agents & Tool Use                (2-3 weeks)
Phase 7  → Fine-Tuning                      (2-3 weeks)
Phase 8  → Multimodal GenAI                 (1-2 weeks)
Phase 9  → Evaluation, Safety, Observability(1-2 weeks)
Phase 10 → Production & Scaling             (2 weeks)
Capstone → Full end-to-end product          (2-4 weeks)

Total: ~4-6 months at a steady, part-time pace
```

Each phase builds on the previous one. Don't jump to RAG before you've built embeddings by hand; don't jump to agents before you understand structured output reliably.

---

<a name="phase-0"></a>
## 4. Phase 0 — Environment & Tooling Setup

**Goal:** a reproducible local dev environment so every future project "just works."

**Set up:**
- Python 3.11+ with `venv` or `conda`/`uv`
- Git + GitHub (every project below should be its own repo)
- VS Code (or your editor of choice) with Python + Jupyter extensions
- A `.env` file pattern for API keys (never hardcode keys) using `python-dotenv`
- Sign up for at minimum one LLM API (Anthropic, OpenAI, or an open one via [Groq](https://groq.com) or [Together AI](https://www.together.ai) — Groq/Together have generous free tiers, good for cost-free experimentation)
- Install [Ollama](https://ollama.com) — lets you run open-weight models (Llama, Mistral, Qwen, Gemma) **locally**, which matters a lot for Phase 1 and Phase 7 where you need low-level access to weights and internals

**Project 0:** Build a tiny CLI tool `ask.py` that takes a prompt as a command-line argument, calls an LLM API, and prints the response — using nothing but the `requests` library (not the official SDK). This forces you to understand the raw HTTP request/response shape before any SDK abstracts it away.

**Self-check:** Can you explain, without looking it up, what headers and JSON fields your raw HTTP call needed, and why?

---

<a name="phase-1"></a>
## 5. Phase 1 — LLM Foundations From Scratch

This is the phase people skip and later regret. Understanding what's actually happening inside the model is what lets you debug weird outputs, design better prompts, reason about cost/latency, and eventually fine-tune.

### 5.1 Concepts to build, not just read about
- Tokenization (BPE) — how text becomes numbers
- Word embeddings and why "meaning" becomes geometry
- Self-attention — the actual mechanism, not the metaphor
- The Transformer block (attention + feedforward + residuals + layernorm)
- Autoregressive generation (sampling, temperature, top-k, top-p)
- Training objective (next-token prediction) vs. what "instruction-tuned"/"chat" models add on top (SFT, RLHF/DPO at a conceptual level)

### 5.2 Primary resource (do this, in order)
1. **[Andrej Karpathy — "Let's build GPT: from scratch, in code, spelled out"](https://www.youtube.com/watch?v=kCc8FmEb1nY)** (YouTube, ~2 hrs) — the single best resource for this phase. Code along in a Jupyter notebook, don't just watch.
2. **[Karpathy's "Neural Networks: Zero to Hero" series](https://karpathy.ai/zero-to-hero.html)** — do at least the micrograd and makemore videos before the GPT one if backprop is fuzzy.
3. **[The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)** by Jay Alammar — read alongside Karpathy's video for the visual intuition.
4. **["Attention Is All You Need"](https://arxiv.org/abs/1706.03762)** — the original paper. Read it *after* the video/blog, not before; it'll make sense this time.
5. **[Hugging Face NLP Course, Chapter 1-3](https://huggingface.co/learn/nlp-course)** — for tokenizers and the `transformers` library ecosystem.

### 5.3 Hands-on projects
- **Project 1a: Build a byte-pair encoding tokenizer from scratch** (no libraries). Train it on a small text corpus, encode/decode round-trip, inspect the vocabulary it learns.
- **Project 1b: Build and train a tiny GPT (nanoGPT-style) on a small dataset** (e.g., Shakespeare, or your own WhatsApp chat export) using Karpathy's code as a reference *after* you've attempted your own first. Get it to generate plausible-looking text.
- **Project 1c: Implement sampling strategies yourself** — greedy, temperature sampling, top-k, top-p (nucleus) — on your trained model's output logits, and observe how output quality/diversity changes.

### 5.4 Break it
- Set temperature absurdly high (e.g., 5.0) and absurdly low (0.01) and explain what you observe in terms of the softmax distribution.
- Deliberately corrupt your tokenizer's merge rules and see how generation degrades.

### 5.5 Self-check
Can you draw (on paper, no notes) the flow of a single token through one transformer block, labeling Q/K/V, attention scores, softmax, and the residual connections?

---

<a name="phase-2"></a>
## 6. Phase 2 — Working With LLM APIs Like an Engineer

**Goal:** move from "raw HTTP calls" to production-grade usage patterns: streaming, retries, cost/token accounting, system prompts, multi-turn state.

### Concepts
- System vs. user vs. assistant roles, and conversation state management
- Streaming responses (SSE) vs. blocking calls
- Token counting and cost estimation
- Rate limits, retries with exponential backoff, timeouts
- Context window management (truncation/summarization strategies)

### Resources
- [Anthropic API documentation](https://docs.claude.com) and [Messages API reference](https://docs.claude.com/en/api/messages)
- [OpenAI API reference](https://platform.openai.com/docs/api-reference) (useful even if you primarily use Claude — comparing API design choices sharpens understanding)
- [tiktoken](https://github.com/openai/tiktoken) / Anthropic's token counting endpoint, for hands-on token accounting

### Projects
- **Project 2a:** Rebuild `ask.py` from Phase 0 using the official SDK, add streaming output token-by-token to the terminal.
- **Project 2b:** Build a multi-turn CLI chatbot that maintains conversation history, handles context-window overflow by summarizing older turns, and tracks running token cost.
- **Project 2c:** Add retry logic with exponential backoff and jitter for rate-limit errors (simulate them by hammering the API quickly).

### Break it
Force a context-window overflow on purpose and observe/handle the error. Force a rate-limit error and verify your backoff logic actually works (log timestamps of retries).

### Self-check
Can you explain, in your own words, why streaming reduces perceived latency but not total latency or cost?

---

<a name="phase-3"></a>
## 7. Phase 3 — Prompt Engineering as a Discipline

**Goal:** treat prompting as an engineering practice with test cases, not guesswork.

### Concepts
- Zero-shot, few-shot, chain-of-thought prompting
- Structured output (JSON mode / XML tags / function-calling schemas) and why it matters for programmatic use
- Prompt templates and variables
- System prompt design (role, constraints, output format, examples)
- Prompt injection risk basics

### Resources
- [Anthropic's Prompt Engineering Guide](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview)
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- ["Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"](https://arxiv.org/abs/2201.11903) (paper)
- [Learn Prompting](https://learnprompting.org/) (free, structured course)

### Projects
- **Project 3a:** Build a "prompt test harness" — a script that runs the same task through several prompt variants (zero-shot, few-shot, CoT) against a small labeled dataset you create yourself (10–20 examples), and scores accuracy for each variant.
- **Project 3b:** Build a structured-data extractor: feed it messy text (e.g., resumes, receipts, support tickets) and force reliable JSON output matching a schema you define, with validation and retry-on-malformed-output logic.

### Break it
Deliberately write an ambiguous or contradictory prompt and diagnose why the model's output degrades. Try a basic prompt-injection against your own extractor and see if your validation catches it.

### Self-check
Given a task, can you predict *before running it* whether few-shot examples or explicit reasoning steps will help more — and explain why?

---

<a name="phase-4"></a>
## 8. Phase 4 — Embeddings & Semantic Search

**Goal:** understand vector representations of meaning well enough to build search/retrieval without a black-box library doing everything.

### Concepts
- What an embedding actually is (a learned vector) and how it differs from a one-hot or TF-IDF representation
- Cosine similarity vs. dot product vs. Euclidean distance
- Vector indexes (flat/brute-force vs. approximate nearest neighbor — HNSW, IVF)
- Chunking strategies for documents

### Resources
- [OpenAI/Anthropic embeddings guides](https://docs.claude.com/en/docs/build-with-claude/embeddings) (Anthropic recommends third-party embedding providers like Voyage AI — read their docs too: [Voyage AI docs](https://docs.voyageai.com/))
- [Pinecone's "What is a Vector Database" learning series](https://www.pinecone.io/learn/vector-database/) — vendor content, but genuinely well-written on the concepts
- [faiss library docs](https://faiss.ai/) — Facebook's similarity search library, good for understanding ANN indexes hands-on

### Projects
- **Project 4a:** Implement cosine similarity and brute-force nearest-neighbor search **from scratch with numpy**, no vector DB. Embed ~500 short text snippets (e.g., news headlines) and build a "find similar" search over them.
- **Project 4b:** Swap your brute-force search for `faiss` (or `chromadb`) and benchmark speed/accuracy differences as your dataset grows to 50k+ vectors.
- **Project 4c:** Build a simple document chunker (fixed-size, then sentence-aware, then semantic chunking) and evaluate retrieval quality differences between chunking strategies on the same corpus.

### Break it
Use a poor chunking strategy (e.g., mid-sentence splits) and observe how retrieval quality drops. Compare embeddings from two different models on the same query and explain why results differ.

### Self-check
Can you explain why cosine similarity is typically preferred over Euclidean distance for high-dimensional text embeddings?

---

<a name="phase-5"></a>
## 9. Phase 5 — Retrieval-Augmented Generation (RAG)

**Goal:** combine Phase 4 (retrieval) with Phase 2/3 (generation) into a system that answers questions grounded in your own documents.

### Concepts
- The basic RAG pipeline: ingest → chunk → embed → store → retrieve → augment prompt → generate
- Re-ranking retrieved chunks
- Hybrid search (keyword/BM25 + vector)
- Handling "I don't know" / hallucination when retrieval fails
- Evaluating RAG quality (retrieval precision/recall, answer faithfulness)

### Resources
- [Anthropic's guide: Building with RAG / Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval) — a genuinely useful engineering write-up, not just marketing
- ["Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks"](https://arxiv.org/abs/2005.11401) (the original RAG paper)
- [LlamaIndex documentation](https://docs.llamaindex.ai/) and [LangChain RAG tutorials](https://python.langchain.com/docs/tutorials/rag/) — useful *after* you've built one by hand, to see how frameworks abstract the same steps
- [Pinecone/Weaviate/Qdrant docs] — pick one vector DB and read its architecture docs (Qdrant's are particularly clear: [qdrant.tech/documentation](https://qdrant.tech/documentation/))

### Projects
- **Project 5a — "Talk to your PDFs":** Build a full RAG pipeline **by hand** (no LangChain/LlamaIndex yet) over a folder of PDFs you actually care about (textbooks, work docs). Chunk → embed → store in a local vector DB (chromadb/qdrant) → retrieve top-k → construct an augmented prompt → generate an answer with citations back to the source chunk.
- **Project 5b:** Add re-ranking (e.g., a cross-encoder re-ranker, or simple keyword-boosted re-ranking) on top of your retrieved results and measure whether answer quality improves.
- **Project 5c:** Build an evaluation set of 20-30 question/answer pairs for your document corpus, and write a script that scores your RAG system's answers against them (exact match won't work well — use an LLM-as-judge scoring rubric, which you also design yourself).
- **Project 5d (advanced):** Rebuild Project 5a using LangChain or LlamaIndex, and compare code complexity/flexibility against your hand-rolled version. This is where using a framework finally makes sense — because you understand what it's doing under the hood.

### Break it
Ask your RAG system a question that's *not* answerable from the documents and see if it hallucinates or correctly refuses. Fix this with better prompting/guardrails. Feed it a corpus with contradictory information and see how it handles conflicting retrieved chunks.

### Self-check
Can you explain, without a diagram in front of you, every step data takes from "user asks a question" to "answer is generated," including where things typically fail in production RAG systems?

---

<a name="phase-6"></a>
## 10. Phase 6 — Agents & Tool Use

**Goal:** move from single-shot generation to systems that can take actions, call tools/functions, and iterate over multiple steps toward a goal.

### Concepts
- Function calling / tool use schemas
- The agent loop: think → act → observe → repeat (ReAct pattern)
- Planning strategies (single-agent vs. multi-agent, task decomposition)
- Memory (short-term conversation vs. long-term/persistent)
- Guardrails: when to stop, how to prevent infinite loops or runaway actions

### Resources
- [Anthropic's tool use / function calling docs](https://docs.claude.com/en/docs/agents-and-tools/tool-use/overview)
- [Anthropic's "Building Effective Agents" engineering blog post](https://www.anthropic.com/research/building-effective-agents) — this is one of the best practical write-ups on when to use agentic patterns vs. simpler workflows
- ["ReAct: Synergizing Reasoning and Acting in Language Models"](https://arxiv.org/abs/2210.03629) (paper)
- [Model Context Protocol (MCP) docs](https://modelcontextprotocol.io/) — the emerging standard for connecting agents to tools/data sources

### Projects
- **Project 6a:** Build a single-tool agent from scratch: give the model access to one function (e.g., a calculator, or a weather API call) via a hand-written tool-calling loop (no agent framework). Handle the full loop yourself: parse the tool-call request, execute it, feed the result back, get the final answer.
- **Project 6b:** Extend to multiple tools (e.g., web search + calculator + a local file reader) and implement a basic ReAct-style loop with a maximum iteration count and logging of every thought/action/observation.
- **Project 6c — a real agent product:** Build a research assistant agent that takes a question, searches the web (or a fixed set of documents), synthesizes findings across multiple sources, and produces a cited report. This combines Phase 5 (retrieval) + Phase 6 (agentic looping).
- **Project 6d (advanced):** Build a minimal multi-agent system (e.g., a "planner" agent that breaks a task into subtasks and a "worker" agent that executes each one) and compare its output quality/cost against a single-agent version of the same task.

### Break it
Give your agent a tool that can fail (simulate a flaky API) and see whether your loop handles errors gracefully or crashes/loops forever. Deliberately remove the iteration cap and observe a runaway loop, then explain why the cap matters.

### Self-check
Can you explain the cost and latency tradeoffs of agentic loops vs. a single well-crafted prompt, and articulate a rule of thumb for when agentic complexity is actually justified?

---

<a name="phase-7"></a>
## 11. Phase 7 — Fine-Tuning & Model Customization

**Goal:** understand when fine-tuning is (and isn't) the right tool, and actually do it once, hands-on.

### Concepts
- When to fine-tune vs. use RAG vs. use better prompting (this decision matters more than the fine-tuning technique itself)
- Full fine-tuning vs. parameter-efficient fine-tuning (LoRA/QLoRA)
- Instruction tuning / SFT dataset format
- Basics of RLHF/DPO (conceptual — full RLHF is out of scope for a hands-on solo project, but DPO is approachable)
- Evaluation before/after fine-tuning

### Resources
- [Hugging Face's PEFT library docs](https://huggingface.co/docs/peft/index) and [Hugging Face fine-tuning course sections](https://huggingface.co/learn/nlp-course)
- [QLoRA paper](https://arxiv.org/abs/2305.14314)
- [Sebastian Raschka's "Finetuning LLMs" blog series](https://sebastianraschka.com/blog/) — some of the clearest hands-on writing available on this topic
- [Unsloth](https://github.com/unslothai/unsloth) — a library that makes LoRA fine-tuning runnable on consumer/free-tier GPUs (great for hands-on learning without needing serious hardware)
- [Google Colab](https://colab.research.google.com/) free tier / [Kaggle Notebooks](https://www.kaggle.com/code) free GPU hours — use these if you don't have a local GPU

### Projects
- **Project 7a:** Take a small open-weight model (e.g., Llama 3.2 1B/3B, Qwen 2.5 1.5B) and fine-tune it with LoRA on a small custom instruction dataset **you build yourself** (e.g., 100-300 examples in a specific style/domain you know well — customer support tone, a specific coding style, a particular Q&A format).
- **Project 7b:** Evaluate before/after: run the same test prompts through the base model and your fine-tuned model, and score/compare outputs on your own rubric.
- **Project 7c (advanced):** Try a DPO fine-tune with preference pairs (chosen vs. rejected responses) you construct yourself, and observe how the model's behavior shifts.

### Break it
Overfit deliberately (too many epochs, too small a dataset) and observe the model losing general capability/repeating training examples verbatim. This teaches you what overfitting *looks like* in generation, not just in a loss curve.

### Self-check
Given a real business problem, can you argue clearly whether it needs fine-tuning, RAG, better prompting, or some combination — and defend the cost/complexity tradeoff of each?

---

<a name="phase-8"></a>
## 12. Phase 8 — Multimodal GenAI

**Goal:** extend beyond text to images (and optionally audio), both generation and understanding.

### Concepts
- Vision-language models (image understanding via multimodal LLMs)
- Diffusion models — conceptual understanding of the denoising process
- Text-to-image and image-to-text pipelines
- Basic audio: speech-to-text (Whisper) and text-to-speech

### Resources
- [Anthropic vision docs](https://docs.claude.com/en/docs/build-with-claude/vision)
- ["How Diffusion Models Work" — Hugging Face Diffusers course](https://huggingface.co/learn/diffusion-course)
- [Jay Alammar's "The Illustrated Stable Diffusion"](https://jalammar.github.io/illustrated-stable-diffusion/)
- [OpenAI Whisper GitHub](https://github.com/openai/whisper) for hands-on speech-to-text

### Projects
- **Project 8a:** Build an image-understanding tool (e.g., a receipt/invoice parser, or a UI-screenshot-to-code describer) using a vision-capable LLM API.
- **Project 8b:** Run Stable Diffusion locally (via `diffusers` library) and manually implement the sampling loop conceptually — generate images, vary guidance scale/steps, and explain the effect of each on output quality.
- **Project 8c:** Build a voice notes transcription + summarization pipeline (Whisper for transcription → your Phase 3 prompting skills for summarization).

### Self-check
Can you explain, at a high level, why diffusion models generate images through iterative denoising rather than in one forward pass — and what "guidance scale" is actually doing?

---

<a name="phase-9"></a>
## 13. Phase 9 — Evaluation, Safety & Observability

**Goal:** learn to measure whether your GenAI system is actually good, and monitor it once deployed — a skill most tutorials skip entirely and that separates hobby projects from real products.

### Concepts
- Offline evaluation (test sets, LLM-as-judge, human eval)
- Online evaluation (A/B testing, user feedback signals)
- Hallucination detection strategies
- Logging/tracing for LLM pipelines (inputs, outputs, latency, cost per call)
- Basic red-teaming / adversarial testing of your own systems

### Resources
- [Anthropic's guide to building evals](https://docs.claude.com/en/docs/test-and-evaluate/develop-tests)
- [OpenAI Evals framework](https://github.com/openai/evals)
- [Langfuse docs](https://langfuse.com/docs) or [LangSmith docs](https://docs.smith.langchain.com/) — open-source/hosted observability tools for LLM apps

### Projects
- **Project 9a:** Add structured logging/tracing to your Phase 5 RAG system and Phase 6 agent — every call logged with input, output, latency, token cost, retrieved chunks. Build a simple dashboard (even a local Streamlit app) to visualize this.
- **Project 9b:** Build an LLM-as-judge evaluation script for one of your earlier projects, and validate it against your own manual scoring on a subset to check the judge is trustworthy.
- **Project 9c:** Red-team your own RAG/agent project — try to get it to leak system prompt contents, ignore instructions, or take an unintended action. Document what worked and patch it.

### Self-check
Can you design an evaluation plan for a GenAI feature *before* building it, specifying what "good" looks like numerically?

---

<a name="phase-10"></a>
## 14. Phase 10 — Production Systems & Scaling

**Goal:** package a project as something that could actually be deployed and used by other people.

### Concepts
- Serving an LLM app behind an API (FastAPI/Flask)
- Caching strategies (semantic caching, exact-match caching) for cost/latency
- Rate limiting and queuing for concurrent users
- Basic cost optimization (model routing — small model for easy tasks, large model for hard ones)
- Containerization basics (Docker) for reproducible deployment

### Resources
- [FastAPI documentation](https://fastapi.tiangolo.com/)
- [Docker's official "Get Started" guide](https://docs.docker.com/get-started/)
- [Anthropic's prompt caching docs](https://docs.claude.com/en/docs/build-with-claude/prompt-caching) — directly relevant to cost/latency optimization

### Projects
- **Project 10a:** Wrap your best project so far (RAG app or agent) in a FastAPI backend with proper request/response schemas, error handling, and a simple frontend (even a basic HTML/JS page or a Streamlit UI).
- **Project 10b:** Add semantic caching (reuse cached responses for near-duplicate queries using your Phase 4 embedding skills) and measure cost savings.
- **Project 10c:** Containerize the whole thing with Docker and deploy it somewhere free/cheap (Render, Railway, Fly.io, or a free-tier VM) so it's a real, shareable, running product.

### Self-check
Could a stranger clone your repo, follow your README, and get the project running in under 15 minutes?

---

<a name="capstone"></a>
## 15. Capstone Project Ideas

Pick one and build it end-to-end, applying everything above. These are intentionally open-ended:

1. **Personal Knowledge Assistant** — RAG + agent over your own notes/documents/emails, with a memory system, evaluation suite, and a real UI.
2. **Domain-Specific Fine-Tuned Assistant** — fine-tune a small open model on a niche domain you know deeply (legal, medical-adjacent, a specific codebase, a hobby), and wrap it with RAG for facts the fine-tune can't memorize.
3. **Multi-Agent Research/Reporting Tool** — an agent system that researches a topic across multiple sources, cross-checks facts, and produces a cited report, with full observability/logging.
4. **Multimodal Content Pipeline** — takes voice notes or screenshots as input and produces structured, searchable, summarized output, with a proper evaluation harness.

Whichever you choose, the bar is: **it should be something you'd actually use weekly, deployed somewhere real, with logging, evaluation, and a README good enough for someone else to run it.**

---

<a name="resources"></a>
## 16. Master Resource List

**Foundational courses**
- [Andrej Karpathy — Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html) (free, YouTube)
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course) (free)
- [Hugging Face Diffusion Models Course](https://huggingface.co/learn/diffusion-course) (free)
- [DeepLearning.AI short courses](https://www.deeplearning.ai/short-courses/) (many free, cover RAG, agents, fine-tuning, prompt engineering — practical and current)
- [fast.ai Practical Deep Learning](https://course.fast.ai/) (free, broader ML foundation if you want to go deeper into the underlying ML)

**Books**
- *Speech and Language Processing* — Jurafsky & Martin ([free draft available online](https://web.stanford.edu/~jurafsky/slp3/)) — the standard NLP reference text
- *Hands-On Large Language Models* — Jay Alammar & Maarten Grootendorst
- *Designing Machine Learning Systems* — Chip Huyen (production/systems thinking, highly relevant to Phases 9-10)
- *Build a Large Language Model (From Scratch)* — Sebastian Raschka

**Papers worth actually reading (in this order)**
1. ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762)
2. ["BERT: Pre-training of Deep Bidirectional Transformers"](https://arxiv.org/abs/1810.04805)
3. ["Language Models are Few-Shot Learners" (GPT-3)](https://arxiv.org/abs/2005.14165)
4. ["Chain-of-Thought Prompting"](https://arxiv.org/abs/2201.11903)
5. ["Retrieval-Augmented Generation"](https://arxiv.org/abs/2005.11401)
6. ["ReAct: Synergizing Reasoning and Acting"](https://arxiv.org/abs/2210.03629)
7. ["LoRA: Low-Rank Adaptation of Large Language Models"](https://arxiv.org/abs/2106.09685)
8. ["QLoRA: Efficient Finetuning of Quantized LLMs"](https://arxiv.org/abs/2305.14314)
9. ["Direct Preference Optimization"](https://arxiv.org/abs/2305.18290)

**Official docs you'll return to constantly**
- [docs.claude.com](https://docs.claude.com) (Anthropic)
- [platform.openai.com/docs](https://platform.openai.com/docs) (OpenAI, useful for comparison)
- [huggingface.co/docs](https://huggingface.co/docs) (transformers, PEFT, datasets)
- [docs.llamaindex.ai](https://docs.llamaindex.ai/) and [python.langchain.com/docs](https://python.langchain.com/docs/) (RAG/agent frameworks)
- [qdrant.tech/documentation](https://qdrant.tech/documentation/) or [docs.trychroma.com](https://docs.trychroma.com/) (vector databases)
- [modelcontextprotocol.io](https://modelcontextprotocol.io/) (MCP for tool/agent integration)

**Communities / staying current**
- [Hugging Face forums](https://discuss.huggingface.co/) and Hugging Face Hub (browse trending open models/datasets weekly)
- r/LocalLLaMA (for open-weight model developments and practical fine-tuning discussions)
- [Papers with Code](https://paperswithcode.com/) (for tracking what's state-of-the-art, with implementations)
- Anthropic and OpenAI engineering blogs (practical write-ups, not just launch announcements)

---

<a name="study-structure"></a>
## 17. Suggested Weekly Study Structure

A sustainable part-time pace (assuming ~8-10 hours/week):

- **2-3 hrs:** Read/watch the primary resource for the current phase, taking notes by hand or in your build log.
- **4-5 hrs:** Build the hands-on project(s) for that phase, yourself, without AI code-assistance on the first pass.
- **1 hr:** Do the "break it" exercise and write down what you learned.
- **1 hr:** Review/refactor last week's project code, and update your build log with what you'd do differently.

Every 2-3 phases, take a "consolidation week": no new material, just revisit and improve your earlier projects with what you've since learned (e.g., after Phase 6, go back and add agent-style self-correction to your Phase 5 RAG system).

**Tracking progress:** keep one GitHub repo (or repo-per-phase) with a top-level README checklist of every project above. Check items off as you *ship* them, not as you *understand* them — shipping is what proves understanding.

---

### Final note

The temptation throughout this roadmap will be to reach for a framework or an AI coding assistant the moment something feels hard. Resist it for the first pass of every project. The frameworks (LangChain, LlamaIndex, etc.) are genuinely useful — but they're most useful to people who already know what's happening underneath, because that's the only way to debug them when they inevitably do something unexpected. Build the hard way first; use the shortcuts once you've earned the judgment to know when they're actually saving you time versus hiding a gap in your understanding.
