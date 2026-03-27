# Ashish Jain

**Building AI agents that do real work — not demos.**

I design and ship production-grade agentic systems for small businesses: voice receptionists that answer phones, sales reps that source leads and draft emails, chat agents that demo themselves to prospects. Each one runs autonomously with structured human-in-the-loop gates where decisions actually matter.

The entire operation is managed by an AI-native development factory — 37 custom agents that handle code review, security scanning, cross-project learning propagation, and more.

---

## Flagship Projects

### 🎙️ Veya — AI Voice Receptionist

Handles inbound business calls end-to-end. Qualifies leads, books appointments, updates CRM, sends SMS confirmations — without human involvement.

```
Caller dials → Retell AI (voice) → Veya logic engine (<500ms tool execution)
    ├── HubSpot CRM        search, create, update contact + lifecycle stage
    ├── Google Calendar     fetch slots, book 30-min appointments
    ├── Email               ICS confirmation + post-call summary
    └── SMS (Twilio)        appointment reminders
                           ↓
              Post-call intelligence (Claude Haiku)
                  lead scoring 1–10, structured extraction, CRM update
```

- **Hybrid architecture** — Retell's GPT-4.1 handles real-time conversation; Claude Haiku handles post-call intelligence. Latency and cost concerns stay decoupled.
- **Idempotent tool handlers** — every CRM write is keyed on `call_id`. Retries never cause duplicates.
- **Validation circuit breaker** — monitors LLM output failures on a rolling window. Trips at 15%, auto-recovers at 8%.
- **2,240 tests across 174 files** — 94%+ coverage. Full simulation mode, zero real API calls in CI.

**Stack:** Node.js · Fastify · Retell AI · HubSpot · Google Calendar · Twilio · Claude Haiku · Vitest · Langfuse

---

### 🎯 Hunter — AI Sales Rep

Daily autonomous agent — sources leads from 4 APIs, researches prospects via web scraping, drafts personalized cold emails with a writer+critic loop, manages a warmup-aware sending queue, classifies replies, and adjusts strategy.

```
Daily run (LangGraph — 7-node state machine)
  Plan → Source → Research → Draft → Queue → Reply Processing → Reflect
  (LLM)  (4 APIs)  (scrape)   (writer   (warmup    (IMAP + LLM        (strategy
                               +critic)   schedule)   classifier)        learner)
```

- **Writer + Critic loop** — a second LLM scores every draft 0–10. Below 7 gets silently dropped.
- **Warmup-aware queue flusher** — per-domain sending limits ramp over weeks (5/day → 50). MX validation, exponential backoff with jitter.
- **PII masking for observability** — custom `MaskedCallbackHandler` strips personal data from LangSmith traces before they leave the box.
- **1,845 tests across 109 files** — 97% line coverage, 90% branch coverage. Integration tests use real in-memory SQLite with actual migration SQL.

**Stack:** Node.js · LangGraph · Fastify · Groq · SQLite · Drizzle ORM · Vitest · LangSmith

---

### 💬 Clara — AI Chat Receptionist

After Hunter sources a lead, Clara generates a personalized demo link. The prospect chats with their own AI receptionist — seeing exactly what their customers would experience. Books appointments and updates CRM during the conversation.

**Stack:** Next.js · LangGraph · Groq · SQLite · HubSpot · Google Calendar

---

## The Factory — AI-Native Development Infrastructure

The projects above are managed by a custom development factory: 37 AI agents, 22 automated hooks, and cross-project governance that treats the entire workspace as a single system.

| Layer | What it does |
|-------|-------------|
| **22 hooks** | Fire on every code change — secret scanning, type checking, console.log detection, test enforcement, tool audit logging |
| **Quality agents** | Code reviewer, TDD enforcer, chaos agent (mutation testing), eval judge |
| **Security agents** | Weekly vuln sweeps, prompt injection testing, HITL gate audits, secret drift detection |
| **Learning propagation** | Fix a bug in one project → auto-scan all others for the same anti-pattern → apply the fix |
| **Governance by phase** | Live projects (Veya, Hunter) get full enforcement — 90%+ coverage, threat models, staging deploys. Explore projects get lightweight guardrails. |

**4,000+ tests across the workspace.** Coverage thresholds enforced by CI, not honor system.

---

## Other Projects

| Project | What it does | Stack |
|---------|-------------|-------|
| **GramRover** | Instagram location intelligence — surfaces behavioral and geographic patterns from exported data | Next.js · Dexie · Vitest |
| **Appy** | Job application pipeline — fetches, scores, and pre-fills applications on a schedule | Next.js · Ollama · SQLite · Chrome Extension |
| **Scout** | Autonomous thematic equity discovery — finds investment themes across market data | Next.js · LangGraph · Anthropic · SQLite |
| **AI Dose** | Automated Instagram content pipeline — one ready-to-post image and caption per day | Python · Claude · Pillow |
| **SummerCampSync** | AI-powered summer camp finder with smart filtering | Next.js · Gemini |
| **Cracked** | Daily puzzle game with deterministic PRNG seeding | Next.js · Vitest |
| **Crib Sheet** | Upload a room photo, get a personality profile back | Next.js · FastAPI · Moondream 2 |
| **Dashy** | Rhythm platformer game (Geometry Dash-inspired) | Phaser 3 · TypeScript · Vite |

---

## Open Source

### 🩻 [clawdoc](https://github.com/ashishjaingithub/clawdoc)
Diagnoses AI agent sessions from JSONL logs. Detects 14 behavioral patterns — retry loops, context exhaustion, cost spikes, tool misuse — and prescribes fixes. An [OpenClaw](https://github.com/openclaw) skill that works with any agent producing JSONL logs.

**Stack:** Bash · jq

---

## Tech Stack

```
Runtime         Node.js 22  ·  Python 3.12
Frameworks      Fastify  ·  Next.js  ·  LangGraph  ·  LangChain
AI Models       Claude (Haiku/Sonnet)  ·  Groq  ·  Retell AI (voice)  ·  Gemini
Database        PostgreSQL  ·  SQLite  ·  Drizzle ORM
Testing         Vitest (4,000+ tests)  ·  Playwright  ·  in-memory SQLite
Observability   LangSmith  ·  Langfuse  ·  Pino (structured + PII redaction)
Infrastructure  Railway  ·  Docker  ·  GitHub Actions  ·  Husky
```

---

## Get in Touch

Building something that needs an AI agent? Interested in the factory approach?

[LinkedIn](https://linkedin.com/in/ashishjainemail) · [GitHub](https://github.com/ashishjaingithub)
