## Pedro Gomes — Backend & AI Engineer

Salvador, Brazil. I ship production AI systems — multi-tenant WhatsApp agents, voice-first interfaces, hybrid-search RAG, and full-stack SaaS — end to end.

Self-hosted on my own VPS with Docker + Traefik + Let's Encrypt. No PaaS, no shortcuts.

---

### What I build

- **Multi-tenant WhatsApp / Telegram / Slack agents** — 1 isolated container per paying client, persistent memory, business rules per workspace, Stripe-gated subscriptions.
- **Voice-first AI** — real-time loops (Whisper STT → LLM → gpt-4o-mini-tts streaming PCM via SSE → Web Audio API), Twilio + Deepgram telephony, sentence-pipelined TTS for sub-second first-audible-word.
- **Advanced RAG** — pgvector HNSW + tsvector GIN with Reciprocal Rank Fusion, Cohere cross-encoder reranking, **Anthropic Contextual Retrieval** with prompt caching, multi-query expansion, Corrective-RAG self-healing, multi-modal ingest (PDF / image / audio / video).
- **Multi-agent orchestration** — LangGraph state machines, OpenAI Agents SDK, Anthropic SDK with tool use + structured outputs + streaming, multi-provider failover (Anthropic ↔ OpenAI ↔ OpenRouter).
- **Full-stack SaaS** — FastAPI + Next.js 16 + Tailwind 4 + shadcn/ui, multi-tenant Postgres, Redis, BRL payments via Pix + Mercado Pago, Stripe for global, Better Auth / Supabase Auth.
- **Vision + automation** — VLM pipelines (Cohere Embed-4, Gemini), document intelligence, design-system extraction, browser-driving QA agents (browser-use + Claude Sonnet).

---

### Featured products (in production)

| Project | What it does | Stack |
|---|---|---|
| [**OpenClaw — WhatsApp AI Receptionist**](https://agentes.pgdev.com.br) | Per-client isolated WhatsApp agent. Each customer gets their own Docker container with persistent memory, business rules, and a tuned voice. Multi-tenant infra serving paying clients. | OpenClaw · Docker · Traefik · Claude Sonnet/Haiku · Baileys |
| [**Voice RAG**](https://voicerag.pgdev.com.br) | Upload PDFs, ask out loud, hear cited answers streamed back as natural speech. End-to-end voice loop with sentence-pipelined TTS, Contextual Retrieval, multi-query expansion. | FastAPI · pgvector · FastEmbed · OpenAI Whisper · gpt-4o-mini-tts · Anthropic SDK |
| [**Multi-Modal Knowledge Bot (Group Docs)**](https://group-documents.pgdev.com.br) | Team-grade Q&A over PDFs, images, audio, video, and crawled URLs with cited sources. Tenant-isolated retrieval, Voyage embeddings, Cohere reranking, Anthropic contextual retrieval, SSRF-defended URL ingest. | FastAPI · pgvector · Voyage AI · Cohere Rerank · Google Gemini · Claude Sonnet |
| [**Web Scraper Agent**](https://scraper.pgdev.com.br) | URL + a sentence becomes validated JSON. Multi-layer SSRF / DNS-rebinding defense, prompt-injection isolation, BYOK across 6 providers, per-key cache scoping. | FastAPI · Next.js 16 · Playwright (stealth) · BeautifulSoup · multi-provider LLM |
| [**Design Extractor**](https://github.com/DevPedroGomes/design-extractor) | Extract complete design systems from any URL — typography, colors, components, layout, motion — into a self-contained HTML report. | Next.js 16 · Firecrawl · Gemini Vision · Better Auth · Drizzle · Postgres |
| [**Minha Pousada**](https://minhapousada.pgdev.com.br) | Multi-tenant SaaS for inn reservations. RBAC, staff invites, encrypted CPF, audit trail, Better Auth, Resend, Drizzle. | Next.js 14 · Express · Drizzle · Postgres · Better Auth |
| [**MeetingsTranscript**](https://transcripts.pgdev.com.br) | Audio + YouTube + live-mic transcription with budget guards and prompt-driven reprocessing. Deepgram Nova-3 + Groq Llama 3.3 70B. | Next.js 16 · Deepgram · Groq · yt-dlp |
| [**AI Web Searcher**](https://searcher.pgdev.com.br) | Perplexity-style research with a 5-stage LangGraph pipeline: query planning → Tavily fan-out → grounded synthesis → self-reflection → optional rewrite. | Next.js 16 · FastAPI · LangGraph · Tavily · OpenRouter |

> Every project ships behind Traefik with HTTPS via Let's Encrypt, security headers, request-size limits, and an internal Docker network so backends are never publicly addressable.

---

### In development

Vertical SaaS and high-niche products I'm building toward production:

- **Navigator** — IVR navigator. User says "call this number, I want to cancel my subscription"; the agent calls, listens to the menu with real-time STT, decides which option to press at each level via LLM, and bridges the user to the right department once reached. Twilio + Deepgram + LLM tool use, with menu cache and DTMF/voice fallback.
- **Video-SaaS** — Brazilian AI video generation. PT-first UX, Pix + Mercado Pago in BRL, fal.ai catalog wrapper (Kling, Seedance, Veo, Sora, Hailuo), per-niche prompt libraries, WhatsApp delivery via Evolution API.
- **GuardAI** — AI security agent. Connects any IP camera (Intelbras, Tapo, Hikvision, Dahua) via ONVIF/RTSP, analyzes frames with multimodal VLM, sends contextual WhatsApp alerts. SaaS R$29-149/month per camera.
- **DietaKit** — AI-assisted meal-plan generator for Brazilian nutritionists. TACO-validated nutrition, branded PDF export, patient-profile aware (anthropometrics, restrictions, clinical conditions).
- **HealthDocs** — Personal Health Record (PHR) for the Brazilian market. Centralizes health data with proper privacy controls.
- **AI QA Agent** — browser-use + Claude Sonnet 4.6 driving a real Chromium against my own production apps. Scenarios written in natural language ("sign up at group-documents, upload a PDF, ask a question").

---

### Stack

**Languages:** Python · TypeScript · JavaScript

**AI / LLM:** Anthropic SDK · OpenAI SDK · OpenAI Agents SDK · LangGraph · CrewAI · Agno · OpenClaw · Voyage AI · Cohere Rerank · Tavily · FastEmbed (ONNX)

**Backend:** FastAPI · Node.js · Express · Flask · asyncpg · Pydantic v2 · structlog · Drizzle ORM · SQLAlchemy

**Frontend:** Next.js 14/15/16 · React 18/19 · Tailwind CSS 3/4 · shadcn/ui · Framer Motion · GSAP · Web Audio API

**Data:** PostgreSQL + pgvector (HNSW) · Redis · Supabase · Firebase · MongoDB

**Voice & vision:** Twilio · Deepgram Nova-3 · Whisper · gpt-4o-mini-tts · Cohere Embed-4 · Google Gemini · Playwright Chromium

**Payments & auth:** Stripe · Mercado Pago (Pix + recurring) · Better Auth · Supabase Auth · Clerk · Resend

**Infrastructure:** Docker · Traefik v3 (TLS/HSTS/security headers/rate limit) · Let's Encrypt · Hostinger VPS · Railway · Vercel · AWS · Digital Ocean · GitHub Actions

---

### Agent coding workflow

My day-to-day shipping is deeply integrated with modern agent coding frameworks — I'm proficient with **Claude Code**, **Codex**, and **Antigravity**, leveraging them to ship production software faster and with higher quality. On the AI integration side, I work directly with the **Anthropic SDK** and **OpenAI SDK** day-to-day: prompt caching, tool use, streaming, structured outputs, the Agents SDK.

---

### Let's connect

[![Portfolio](https://img.shields.io/badge/Portfolio-portfolio.pgdev.com.br-8b5cf6?style=flat-square)](https://portfolio.pgdev.com.br)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Pedro%20Gomes-0a66c2?style=flat-square&logo=linkedin)](https://linkedin.com/in/devpedrogomes)
[![Email](https://img.shields.io/badge/Email-devpedrogomes%40gmail.com-d14836?style=flat-square&logo=gmail)](mailto:devpedrogomes@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-%40peugomes-2ca5e0?style=flat-square&logo=telegram)](https://t.me/peugomes)

Open to **AI engineering, backend, full-stack** roles and contracts. If you have a problem that needs to become working software, I can build it.
