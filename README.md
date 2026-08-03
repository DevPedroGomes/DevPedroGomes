## Pedro Gomes

Salvador, Brazil. I run **Gomio**, a one-person software house.

I build AI agents that do the work instead of just answering: book the appointment, create the lead in the CRM, issue the payment link, update the order. And I build everything underneath them, which is usually what decides whether the project survives contact with production: the API, the database, the billing, the multi-tenancy, and the infrastructure it all runs on.

Backend in Python and Node is my core. Most of what I ship has a model somewhere in it, but the hard problems are rarely the model. They are cost that nobody is measuring, a tenant that can read another tenant's rows, a webhook that gets redelivered and bills twice, an agent that says "booked!" when nothing was written anywhere.

---

### What I do

**AI agents that write into your systems.** Booking, lead qualification, quotes inside your pricing rules, order updates, collections, support triage. On WhatsApp, a website widget, Instagram or voice. The agent executes decisions, it does not invent them: a deterministic engine built from your written policy decides what is allowed, and the model only phrases the answer it was authorised to give.

**Retrieval that holds up.** Hybrid search over pgvector HNSW and tsvector, reciprocal rank fusion, cross-encoder reranking, contextual chunk enrichment, multi-query expansion. Multi-modal ingest for PDFs, images, audio and video. Tenant isolation enforced at retrieval, not at the UI.

**Voice, in both directions.** Real-time loops with sub-second time to first audible word: microphone capture, speech to text, model, streaming audio back over SSE into the Web Audio API. Telephony with Twilio and Deepgram, including live IVR navigation.

**The full product around it.** Multi-tenant Postgres with row-level security, Stripe and Mercado Pago billing with idempotent webhooks, metered usage with real spend caps, auth, background jobs, and deployment on Docker and Traefik with TLS, security headers and healthchecks.

---

### Selected work

Everything below is deployed and reachable. The demos run behind per-IP rate limits and daily spend ceilings, because a public AI demo is a spending endpoint.

| Project | What it does | Stack |
|---|---|---|
| [**AI agents on WhatsApp**](https://agentes.pgdev.com.br) | The service page for agent builds: fixed scope, fixed price, official Meta Cloud API on the client's own Business account. Purchase is gated by a deterministic qualifier, so a project that cannot be delivered honestly does not get sold. | Node, Express, SQLite, Stripe, FastAPI gateway |
| [**Voice RAG**](https://voicerag.pgdev.com.br) | Upload PDFs, ask out loud, hear a cited answer streamed back as speech. Session-scoped multi-tenancy with automatic eviction of the vectors. | FastAPI, pgvector, FastEmbed (ONNX), Whisper, gpt-4o-mini-tts |
| [**BrainHub**](https://group-documents.pgdev.com.br) | Team Q&A over PDFs, images, audio, video and crawled URLs with cited sources. SSRF-defended URL ingest, tenant-filtered retrieval. | FastAPI, pgvector, Voyage AI, Cohere Rerank, Gemini, Claude |
| [**Metered transcription**](https://transcripts.pgdev.com.br) | Three pipelines (upload, YouTube, live microphone) behind a hard daily budget: spend reserved before the provider call and reconciled after, a kill switch, and ephemeral provider keys with a 5 second TTL that never reach the browser. | Next.js, Deepgram Nova-3, yt-dlp, OpenRouter |
| [**Web scraper agent**](https://scraper.pgdev.com.br) | A URL plus a sentence becomes validated JSON. Multi-layer SSRF and DNS-rebinding defense, prompt-injection isolation, bring-your-own-key across 6 providers, per-key cache scoping. | FastAPI, Next.js, Playwright, SQLite |
| [**AI research agent**](https://searcher.pgdev.com.br) | Perplexity-style research in five explicit stages: plan, fetch, score the sources, synthesise with mandatory citations, then self-reflect and retry when the evidence is weak. | FastAPI, LangGraph, Tavily, OpenRouter |
| [**IVR navigator**](https://portfolio.pgdev.com.br/en/projects/ai-voice-phone-agent) | Places a real phone call, transcribes the menu live, decides which key to press at each level, and bridges you in once it reaches a human. Handles 13 real-world edge cases including voice menus, DTMF rejection and menu loops. | Python, Twilio, Deepgram Nova-3, Claude Haiku, Postgres, Redis |

Also shipped: [Minha Pousada](https://github.com/DevPedroGomes/Reservas_Pousada), a multi-tenant reservation SaaS with RBAC, staff invites, encrypted CPF and an audit trail.

---

### Gomio

The software house side. Three things I sell:

- **Audit, $390.** Five days. I map your processes and test whether your systems can actually be written to, then send back a scope with a fixed price. If the honest answer is that you do not need to hire anyone, the report says so. Credited against a build.
- **Agent build, $980 to $8,500.** Priced by how many of your systems the agent writes into, not by how many messages it sends.
- **Monthly retainer.** Tuning, new flows, monitoring. Always a separate agreement, never bundled into a build price.

Full scope and pricing at [agentes.pgdev.com.br](https://agentes.pgdev.com.br).

---

### How I work

**Official APIs only.** On WhatsApp that means Meta's Cloud API, on the client's own Business account. Never Baileys, Evolution in QR mode, WPPConnect or Z-API. Meta has been removing those connections since January 2026 and the ban is permanent with no appeal. The number is the client's asset, not mine, so I will not put it at that risk to save a hosting fee.

**Integration before flows.** I prove read and write against the client's sandbox before building anything on top of it. That is the step where projects like this fail, so it goes first, while the scope can still change.

**No tool fabricates a result.** If the booking cannot be written, the agent says so and escalates. It never returns a confirmation code for something that did not happen.

**Spend has a ceiling.** Every paid call is checked against a quota first, with a monthly allowance and a daily cap present on every plan. The daily cap is there for the integration loop, not for the customer exceeding their package.

**The client owns the accounts.** Their Meta Business account, their provider keys, billed to them directly. I do not resell usage, and nothing switches off if they stop working with me.

---

### Stack

**Languages:** Python, TypeScript, JavaScript

**Backend:** FastAPI, Node, Express, Hono, Pydantic, Drizzle, SQLAlchemy, asyncpg

**Frontend:** Next.js, React, Tailwind, shadcn/ui, Framer Motion

**Data:** PostgreSQL with pgvector, Redis, SQLite, Supabase

**AI:** Anthropic SDK, OpenAI SDK, LangGraph, Voyage AI, Cohere Rerank, Deepgram, Tavily, FastEmbed

**Payments and auth:** Stripe, Mercado Pago, Pagar.me, Better Auth, Supabase Auth

**Infrastructure:** Docker, Traefik, GitHub Actions, GHCR, Hostinger VPS, Vercel

---

### Contact

[Portfolio](https://portfolio.pgdev.com.br) · [Gomio](https://agentes.pgdev.com.br) · [LinkedIn](https://linkedin.com/in/devpedrogomes) · [devpedrogomes@gmail.com](mailto:devpedrogomes@gmail.com) · [Telegram](https://t.me/peugomes)

Available for project work and contracts. If you have a process that should be running without a person in the loop, tell me which systems it touches and I will tell you honestly whether it is buildable.
