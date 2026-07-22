# GenAI Hands-On Roadmap — @cyphlia

My learn-by-building path through Generative AI: from tokenizers and attention built by hand, up through RAG, agents, fine-tuning, and a deployed capstone project.

Full methodology, resources, and detailed explanations for every phase live in [`ROADMAP.md`](./ROADMAP.md). This README is the **live progress tracker** — check items off here as you ship them, and commit that checkbox change along with the code.

> Rule: check a box only when the project actually runs and does what it claims, not when you understand it in theory. Shipping is the proof.

---

## Progress Checklist

### Phase 0 — Environment & Tooling
- [ ] Dev environment set up (Python venv, git, VS Code, `.env` handling)
- [ ] Project 0: `ask.py` raw HTTP CLI tool

### Phase 1 — LLM Foundations From Scratch
- [ ] Project 1a: BPE tokenizer from scratch
- [ ] Project 1b: Tiny GPT trained on a custom text corpus
- [ ] Project 1c: Manual sampling strategies (greedy / temperature / top-k / top-p)

### Phase 2 — Working With LLM APIs
- [ ] Project 2a: SDK-based `ask.py` with streaming
- [ ] Project 2b: Multi-turn CLI chatbot with context-window management
- [ ] Project 2c: Retry logic with exponential backoff

### Phase 3 — Prompt Engineering
- [ ] Project 3a: Prompt test harness (zero-shot vs few-shot vs CoT)
- [ ] Project 3b: Structured JSON extractor with schema validation

### Phase 4 — Embeddings & Semantic Search
- [ ] Project 4a: Brute-force cosine similarity search (numpy only)
- [ ] Project 4b: Swap in `faiss`/`chromadb`, benchmark at scale
- [ ] Project 4c: Chunking strategy comparison

### Phase 5 — Retrieval-Augmented Generation
- [ ] Project 5a: "Talk to your PDFs" — hand-built RAG pipeline
- [ ] Project 5b: Add re-ranking
- [ ] Project 5c: RAG evaluation set + LLM-as-judge scoring
- [ ] Project 5d: Rebuild 5a with LangChain/LlamaIndex, compare

### Phase 6 — Agents & Tool Use
- [ ] Project 6a: Single-tool agent, hand-written loop
- [ ] Project 6b: Multi-tool ReAct-style agent
- [ ] Project 6c: Research assistant agent (multi-source + citations)
- [ ] Project 6d: Minimal multi-agent (planner + worker)

### Phase 7 — Fine-Tuning
- [ ] Project 7a: LoRA fine-tune on a custom instruction dataset
- [ ] Project 7b: Before/after evaluation
- [ ] Project 7c: DPO fine-tune with preference pairs

### Phase 8 — Multimodal GenAI
- [ ] Project 8a: Vision-based document/screenshot parser
- [ ] Project 8b: Local Stable Diffusion generation + parameter experiments
- [ ] Project 8c: Voice notes transcription + summarization pipeline

### Phase 9 — Evaluation, Safety & Observability
- [ ] Project 9a: Logging/tracing dashboard for RAG + agent projects
- [ ] Project 9b: LLM-as-judge evaluator, validated against manual scoring
- [ ] Project 9c: Red-team your own system, document + patch findings

### Phase 10 — Production & Scaling
- [ ] Project 10a: FastAPI backend + simple frontend for best project
- [ ] Project 10b: Semantic caching layer
- [ ] Project 10c: Dockerized, deployed, publicly shareable

### Capstone
- [ ] Capstone project selected
- [ ] Capstone shipped and deployed
- [ ] Capstone README good enough for a stranger to run in <15 min

---

## Repo Structure

```
genai-hands-on-roadmap/
├── README.md              ← you are here (progress checklist)
├── ROADMAP.md             ← full methodology, concepts, resources per phase
├── STUDY_LOG.md           ← running build log (what broke, what you learned)
├── phase0-setup/
├── phase1-foundations/
├── phase2-apis/
├── phase3-prompting/
├── phase4-embeddings/
├── phase5-rag/
├── phase6-agents/
├── phase7-finetuning/
├── phase8-multimodal/
├── phase9-evaluation/
├── phase10-production/
└── capstone/
```

Each phase folder has its own README with that phase's project checklist and a place for notes — code for each project lives inside that folder as its own subfolder or script.

## How I Work This Repo

See [`SETUP_GUIDE.md`](./SETUP_GUIDE.md) for full first-time setup in VS Code, and the "Commit Habit" section there for how I keep this repo moving instead of stalling after week one.
