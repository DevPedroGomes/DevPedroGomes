## Pedro Gomes

Salvador, Brazil. I run **Gomio**, a one-person software house.

I build AI agents that do the work instead of just answering: book the appointment,
create the lead in the CRM, issue the payment link, update the order. And I build
everything underneath them, which is usually what decides whether a project survives
contact with production: the API, the database, the billing, the multi-tenancy, and the
infrastructure it all runs on.

Backend in Python and Node is my core. Most of what I ship has a model somewhere in it,
but the hard problems are rarely the model. They are cost that nobody is measuring, a
tenant that can read another tenant's rows, a webhook that gets redelivered and bills
twice, an agent that says "booked!" when nothing was written anywhere.

---

### What I do

**Agents that write into your systems.** Book the appointment, create the lead, issue the
payment link, update the order, then hand off to a person. On WhatsApp, a website widget,
Instagram or voice. The agent executes decisions, it does not invent them: a deterministic
engine built from your written policy decides what is allowed, and the model only phrases
the answer it was authorised to give. Every decision is logged. If the agent would only
answer questions and write nothing anywhere, I will tell you to use Meta's own assistant
instead and charge you nothing for the advice.

**Retrieval you can cite.** Hybrid search over pgvector HNSW and tsvector with reciprocal
rank fusion, cross-encoder reranking, contextual chunk enrichment and multi-query
expansion. Ingest for PDFs, images, audio, video and crawled URLs. Tenant isolation
enforced at retrieval, before ranking, not at the UI.

**Voice, in and out.** Real-time loops with sub-second time to first audible word:
microphone capture, speech to text, model, streaming audio back over SSE into the Web
Audio API. Telephony with Twilio and Deepgram that holds up on a real call, including
live IVR navigation and barge-in.

**The product around it.** Multi-tenant Postgres with row-level security, Stripe and
Mercado Pago billing with idempotent webhooks, metered usage with spend ceilings that
actually refuse, auth, background jobs, and deployment on Docker and Traefik with TLS,
security headers and healthchecks.

---

### Selected work

Everything below is deployed. The demos run behind per-IP rate limits and daily spend
ceilings.

| Project | What it does | Stack |
|---|---|---|
| [**WhatsApp AI Agent · Official Cloud API**](https://agentes.pgdev.com.br) | The service page for agent builds: fixed scope, fixed price, official Meta Cloud API on the client's own Business account. Purchase is gated by a deterministic qualifier, so a project that cannot be delivered honestly does not get sold. | Node, Express, SQLite, Stripe, FastAPI gateway |
| [**Voice AI Assistant (RAG)**](https://voicerag.pgdev.com.br) | Upload PDFs, ask out loud, hear a cited answer streamed back as speech. Session-scoped multi-tenancy, with the vectors evicted when the session expires. | FastAPI, pgvector, FastEmbed (ONNX), Whisper, gpt-4o-mini-tts |
| [**BrainHub · Multimodal RAG**](https://group-documents.pgdev.com.br) | Team Q&A over PDFs, images, audio, video and crawled URLs with cited sources. SSRF-defended URL ingest, tenant-filtered retrieval. | FastAPI, pgvector, Voyage AI, Cohere Rerank, Gemini, Claude |
| [**Metered AI Platform · Transcription**](https://transcripts.pgdev.com.br) | Three pipelines (upload, YouTube, live microphone) behind a hard daily budget: spend reserved before the provider call and reconciled after, a kill switch, and ephemeral provider keys with a 5 second TTL that never reach the browser. | Next.js, Deepgram Nova-3, yt-dlp, OpenRouter |
| [**AI Web Scraping Agent**](https://scraper.pgdev.com.br) | A URL plus a sentence becomes validated JSON. Multi-layer SSRF and DNS-rebinding defense, prompt-injection isolation, bring-your-own-key across 6 providers, per-key cache scoping. | FastAPI, Next.js, Playwright, SQLite |
| [**AI Research Agent**](https://searcher.pgdev.com.br) | Perplexity-style research in five explicit stages: plan, fetch, score the sources, synthesise with mandatory citations, then self-reflect and retry when the evidence is weak. | FastAPI, LangGraph, Tavily, OpenRouter |
| [**AI Voice Phone Agent**](https://portfolio.pgdev.com.br/en/projects/ai-voice-phone-agent) | Places a real phone call, transcribes the menu live, decides which key to press at each level, and bridges you in once it reaches a human. Handles 13 real-world edge cases including voice menus, DTMF rejection and menu loops. | Python, Twilio, Deepgram Nova-3, Claude Haiku, Postgres, Redis |

Also shipped: [Minha Pousada](https://github.com/DevPedroGomes/Reservas_Pousada), a
multi-tenant reservation SaaS with RBAC, staff invites, encrypted CPF and an audit trail.

---

### Gomio

The software house side. What I sell:

- **Audit.** Five days. I map your processes and test whether your systems can actually be
  written to, then send back a scope with a fixed price. If the honest answer is that you
  do not need to hire anyone, the report says so. Credited against a build.
- **Agent build.** An agent that books, quotes, charges or updates inside the systems you
  already run. Scoped by how many of those it writes into, not by how many messages it
  sends.
- **AI phone receptionist.** Answers your business calls, handles your common questions,
  captures the caller's details and transfers to a person when it should. Barge-in,
  voicemail detection, and every call transcribed. You own the accounts from day one.

Scope and pricing at [agentes.pgdev.com.br](https://agentes.pgdev.com.br).

---

### How I work

**Integration before flows.** I prove read and write against the client's sandbox before
building anything on top of it. That is the step where projects like this fail, so it goes
first, while the scope can still change.

**No tool fabricates a result.** If the booking cannot be written, the agent says so and
escalates. It never returns a confirmation code for something that did not happen.

**Spend has a ceiling.** Every paid call is checked against a quota first, with a monthly
allowance and a daily cap present on every plan. The daily cap is there for the
integration loop, not for the customer exceeding their package.

**The client owns the accounts.** Their Meta Business account, their provider keys, billed
to them directly.

---

### Stack

**Languages:** Python, TypeScript, JavaScript

**Backend:** FastAPI, Node, Express, Hono, Pydantic, Drizzle, SQLAlchemy, asyncpg

**Frontend:** Next.js, React, Tailwind, shadcn/ui

**Data:** PostgreSQL with pgvector, Redis, SQLite

**AI:** Anthropic SDK, OpenAI SDK, LangGraph, Voyage AI, Cohere Rerank, Deepgram, Tavily,
FastEmbed

**Payments and auth:** Stripe, Mercado Pago, Better Auth, JWT

**Infrastructure:** Docker, Traefik, GitHub Actions, GHCR

---

### Contact

[Portfolio](https://portfolio.pgdev.com.br) · [Gomio](https://agentes.pgdev.com.br) ·
[LinkedIn](https://linkedin.com/in/devpedrogomes) ·
[devpedrogomes@gmail.com](mailto:devpedrogomes@gmail.com) ·
[Telegram](https://t.me/peugomes)

Available for project work and contracts. The fastest way to know whether I can help: tell
me which systems the agent would have to write into, and paste the link to their API docs.
