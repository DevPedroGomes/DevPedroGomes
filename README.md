## Pedro Gomes

Salvador, Brazil. Backend and AI engineer, open to remote roles and contracts.

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
payment link, update the order, then record the request for a person on your team to follow
up. On WhatsApp, a website widget, Instagram or voice. The agent executes decisions, it does
not invent them: a deterministic engine built from your written policy decides what is
allowed, and the model only phrases the answer it was authorised to give. Every decision is logged.

**Retrieval you can cite.** Hybrid search over pgvector HNSW and tsvector with reciprocal
rank fusion, cross-encoder reranking, contextual chunk enrichment and multi-query
expansion. Ingest for PDFs, images, audio, video and crawled URLs. Tenant isolation
enforced at retrieval, before ranking, not at the UI.

**Voice, in and out.** Telephony that holds up on a real call, including live phone-menu
navigation and barge-in. [Gom](https://voz.gomio.com.br/en), Gomio's own voice agent,
checks refusals and known prompt-injection phrases in code before the model is
called, and next to the conversation each decision shows the written rule behind it. 751
tests, and 9 scripted conversations in each language must pass before every release.
To measure this kind of agent I built
[voice-latency-probe](https://github.com/DevPedroGomes/voice-latency-probe), a synthetic
caller that splits each turn into end of turn, agent and delivery, and is calibrated in CI
against an agent with known delays.

**The product around it.** Multi-tenant Postgres with row-level security, Stripe billing
with idempotent webhooks, metered usage with spend ceilings that actually refuse, auth,
background jobs, and deployment on Docker and Traefik with TLS, security headers and
healthchecks.

---

### Selected work

Everything below is deployed. The demos run behind per-IP rate limits and spend
ceilings.

| Project | What it does | Stack |
|---|---|---|
| [**Clínica Aurora · Demo agent that writes to a schedule**](https://aurora.pgdev.com.br) ([code](https://github.com/DevPedroGomes/clinica-aurora)) | Demo with a fictional clinic, open, no signup: you talk, and the row changes in the clinic system live. Five business rules are decided by a deterministic engine instead of the model, every decision is logged with the rule code that produced it, and a button takes the clinic system down to prove the agent escalates rather than inventing a confirmation. | FastAPI, Claude Sonnet 5, deterministic policy engine, server-sent events |
| [**Clínica Aurora MCP server**](https://portfolio.pgdev.com.br/en/projects/aurora-mcp) | The same fictional clinic schedule, published as an MCP server where every write takes two calls: the first returns a proposal and a confirmation token, the second commits it. A write the policy refuses comes back with the rule code, and two tools return the rules and the session's decision log. | MCP (streamable HTTP), FastAPI, Python 3.12 |
| [**Gom · Gomio's voice agent**](https://voz.gomio.com.br/en) | Answers callers, takes their name, phone and reason, and refuses what the business does not do. Next to the conversation, each decision shows the written rule behind it. Talk to her through your microphone, or replay ready-made cases without one. 751 tests, and 9 scripted conversations in each language must pass before every release. | LiveKit Agents, Gemini 3.1 Flash Lite, Cartesia Sonic 3.5, Next.js |
| [**Chancela · Does the AI recommend this business?**](https://chancela.gomio.com.br/en) | Ask an AI "best dentist in Salvador" and a real business either shows up or it does not. Measures which, question by question and live in front of you, and reports only what came back. Detection is exact matching, never fuzzy: a false positive would cost the audit the one thing it sells, which is being checkable. | FastAPI, PostgreSQL queue, OpenRouter (GPT, Gemini), Next.js |
| [**Gomio operation demos · Real estate, accounting, law firm**](https://portfolio.pgdev.com.br/en/projects/gomio-operation-demos) | A real estate agency you operate from both sides: write as a tenant, then approve the reply in the owner's panel. The same engine runs an accounting firm and a law firm. Code answers the facts and the model only drafts text a person approves. Each visitor gets an isolated copy, and the demos were evaluated with the real model before release. In Portuguese, built for Brazilian businesses. | FastAPI, SQLite, Claude Haiku 4.5, Claude Sonnet 5.5, Playwright |
| [**BrainHub · Multimodal RAG**](https://group-documents.pgdev.com.br) ([code](https://github.com/DevPedroGomes/brainhub-rag)) | Team Q&A over PDFs, images, audio, video and crawled URLs with cited sources. SSRF-defended URL ingest, tenant-filtered retrieval. | FastAPI, pgvector, Voyage AI, Cohere Rerank, Gemini, Claude |
| [**AI Web Scraping Agent**](https://scraper.pgdev.com.br) | A URL plus a sentence becomes validated JSON. Multi-layer SSRF and DNS-rebinding defense, prompt-injection isolation, bring-your-own-key across 6 providers, per-key cache scoping. | FastAPI, Next.js, Playwright, SQLite |

Also shipped: [agent-ops](https://github.com/DevPedroGomes/agent-ops), a Python library extracted from a
document Q&A platform running in production: a spend cap that refuses when it cannot read itself, an
audit trail that answers "why did it decide that" with a rule code, and a durable queue that survives a
redeploy. And [Diária](https://diaria.pgdev.com.br), a multi-tenant reservation SaaS with RBAC,
staff invites, encrypted CPF and an audit trail ([code](https://github.com/DevPedroGomes/Reservas_Pousada)).

---

### Client work

Backend for a consumer telephony app, not launched yet, that maps a company's phone menu by
calling it, then walks that menu on a live call for the user and hands the call over at the
destination. I was responsible for most of the core service: the engine that maps each
company's phone menu and the one that navigates it live during a call. The work is under NDA,
so there is no demo, no code and no vendor names.
[The case](https://portfolio.pgdev.com.br/en/projects/phone-menu-navigation) has the
architecture and what was measured: in the final validation round of live test calls, all 17
sessions reached the requested destination.

---

### How I work

**Integration before flows.** I prove read and write against the client's sandbox before
building anything on top of it. That is the step where projects like this fail, so it goes
first, while the scope can still change.

**Confirmed means written.** The agent only confirms after the target system confirmed the
write. If it was not written, the agent says so and escalates.

**Spend has a ceiling.** Every paid call is checked against a quota first, with a monthly
allowance and a daily cap.

---

### Stack

**Languages:** Python, TypeScript, JavaScript

**Backend:** FastAPI, Node, Express, Hono, Pydantic, Drizzle, SQLAlchemy, asyncpg

**Frontend:** Next.js, React, Tailwind, shadcn/ui

**Data:** PostgreSQL with pgvector, Redis, SQLite

**AI:** Anthropic SDK, OpenAI SDK, LangGraph, MCP, LiveKit Agents, Cartesia, Voyage AI, Cohere
Rerank

**Payments and auth:** Stripe, Better Auth, JWT

**Infrastructure:** Docker, Traefik, GitHub Actions, GHCR

---

### Contact

[Portfolio](https://portfolio.pgdev.com.br) ·
[LinkedIn](https://linkedin.com/in/devpedrogomes) ·
[devpedrogomes@gmail.com](mailto:devpedrogomes@gmail.com) ·
[Telegram](https://t.me/peugomes)

Open to remote backend and AI roles, and to contracts.
