## Pedro Gomes

Backend & AI Engineer in Salvador, Brazil, working remotely. I run **Gomio**, a one-person
software house.

I build software that gets work done in your existing systems: full-stack development,
integrations and business rules enforced in code, with AI where it solves the problem. From
prototype to production, including the API, the database, the multi-tenancy and the
infrastructure it runs on.

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

**Voice agents.** [Sofia](https://voz.gomio.com.br/en), Gomio's voice receptionist, runs on
LiveKit Agents with Deepgram Flux and Cartesia. Refusals and the check for known
prompt-injection phrases run in code before the model is called, and next to the
conversation each decision shows the written rule behind it. 751 tests, and 9 scripted
conversations in each language must pass before every release.

**The product around it.** Multi-tenant Postgres with row-level security, metered usage
with spend ceilings that actually refuse, auth, background jobs, and deployment on Docker
and Traefik with TLS, security headers and healthchecks.

---

### Selected work

Everything below is deployed. The demos run behind per-IP rate limits and spend
ceilings.

| Project | What it does | Stack |
|---|---|---|
| [**Clínica Aurora · Agent that writes to a real schedule**](https://aurora.pgdev.com.br) | Open demo, no signup: you talk, and the row changes in the clinic system live. Five business rules are decided by a deterministic engine instead of the model, every decision is logged with the rule code that produced it, and a button takes the clinic system down to prove the agent escalates rather than inventing a confirmation. | FastAPI, Claude Sonnet 5, deterministic policy engine, server-sent events |
| [**Clínica Aurora MCP server**](https://portfolio.pgdev.com.br/en/projects/aurora-mcp) | The same clinic schedule, published as an MCP server where every write takes two calls: the first returns a proposal and a confirmation token, the second commits it. A write the policy refuses comes back with the rule code, and two tools return the rules and the session's decision log. | MCP (streamable HTTP), FastAPI, Python 3.12 |
| [**Sofia · Voice receptionist**](https://voz.gomio.com.br/en) | Answers callers, takes their name, phone and reason, and refuses what the business does not do. Next to the conversation, each decision shows the written rule behind it. Talk to her through your microphone, or replay ready-made cases without one. 751 tests, and 9 scripted conversations in each language must pass before every release. | LiveKit Agents, Deepgram Flux, Gemini 3.1 Flash Lite, Cartesia Sonic 3.5, Next.js |
| [**Chancela · Does the AI recommend this business?**](https://chancela.gomio.com.br/en) | Ask an AI "best dentist in Salvador" and a real business either shows up or it does not. Measures which, question by question and live in front of you, and reports only what came back. Detection is exact matching, never fuzzy: a false positive would cost the audit the one thing it sells, which is being checkable. | FastAPI, PostgreSQL queue, OpenRouter (GPT, Gemini), Next.js |
| [**Gomio operation demos · Real estate, accounting, law firm**](https://portfolio.pgdev.com.br/en/projects/gomio-operation-demos) | A real estate agency you operate from both sides: write as a tenant, then approve the reply in the owner's panel. The same engine runs an accounting firm and a law firm. Code answers the facts and the model only drafts text a person approves. Each visitor gets an isolated copy, and every demo passed a 60-message evaluation with the real model before release. 572 tests. In Portuguese, built for Brazilian businesses. | FastAPI, SQLite, Claude Haiku 4.5, Claude Sonnet 5.5, Playwright |
| [**BrainHub · Multimodal RAG**](https://group-documents.pgdev.com.br) | Team Q&A over PDFs, images, audio, video and crawled URLs with cited sources. SSRF-defended URL ingest, tenant-filtered retrieval. | FastAPI, pgvector, Voyage AI, Cohere Rerank, Gemini, Claude |
| [**AI Web Scraping Agent**](https://scraper.pgdev.com.br) | A URL plus a sentence becomes validated JSON. Multi-layer SSRF and DNS-rebinding defense, prompt-injection isolation, bring-your-own-key across 6 providers, per-key cache scoping. | FastAPI, Next.js, Playwright, SQLite |

Also shipped: [agent-ops](https://github.com/DevPedroGomes/agent-ops), a Python library extracted from a
document Q&A platform running in production: a spend cap that refuses when it cannot read itself, an
audit trail that answers "why did it decide that" with a rule code, and a durable queue that survives a
redeploy. And [Diária](https://diaria.pgdev.com.br), a complete product I built alone:
reservations for small inns, Pix payments, and a different role for each property a person works
at ([code](https://github.com/DevPedroGomes/Reservas_Pousada)).

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

### Gomio

The software house side. What I sell:

- **Audit.** Five days. I map your processes and test whether your systems can actually be
  written to, then send back a scope with a fixed price. If the honest answer is that you
  do not need to hire anyone, the report says so. Credited against a build.
- **Agent build.** An agent that books, quotes, charges or updates inside the systems you
  already run. Scoped by how many of those it writes into, not by how many messages it
  sends.
- **AI phone receptionist.** Answers your business calls, handles your common questions,
  captures the caller's details and transfers to a person when it should. Barge-in and
  every call transcribed. You own the accounts from day one.

Scope and pricing at [gomio.com.br](https://gomio.com.br).

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

**AI:** Anthropic SDK, OpenAI SDK, LangGraph, MCP, LiveKit Agents, Cartesia, Voyage AI, Cohere
Rerank

**Auth:** Better Auth, JWT

**Infrastructure:** Docker, Traefik, GitHub Actions, GHCR

---

### Contact

[Portfolio](https://portfolio.pgdev.com.br/en) · [Gomio](https://gomio.com.br) ·
[LinkedIn](https://linkedin.com/in/devpedrogomes) ·
[contato@pgdev.com.br](mailto:contato@pgdev.com.br) ·
[Telegram](https://t.me/peugomes)

Available for project work and contracts. The fastest way to know whether I can help: tell
me which systems the agent would have to write into, and paste the link to their API docs.
