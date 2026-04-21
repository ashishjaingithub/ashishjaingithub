<div align="center">

# Ashish Jain

**I build AI agents that run real businesses — not demos.**

Voice receptionists that answer phones. Sales reps that source leads and write cold emails.  
Chat agents that demo themselves to prospects. Each one runs autonomously in production.

[ashish@cranial.ai](mailto:ashish@cranial.ai) · [LinkedIn](https://linkedin.com/in/ashishjainemail) · Available for consulting engagements

</div>

---

## Active

### 🎯 Hunter — AI Sales Rep

Daily autonomous agent that sources leads, researches prospects, drafts personalized cold emails with a writer+critic quality gate, manages a warmup-aware sending queue, classifies replies, and adjusts strategy — end to end.

```
Daily run (LangGraph — 7-node state machine)
  Plan → Source → Research → Draft → Queue → Reply Processing → Reflect
  (LLM)  (4 APIs)  (scrape)  (writer   (warmup    (IMAP + LLM      (strategy
                              +critic)  schedule)   classifier)       learner)
```

- **Writer + Critic loop** — a second LLM scores every draft 0–10. Below 7 gets dropped before it reaches the queue.
- **Warmup-aware queue flusher** — per-domain sending limits ramp over weeks (1/day → 55). MX validation, exponential backoff with jitter.
- **PII masking for observability** — custom `MaskedCallbackHandler` strips personal data from Langfuse traces before they leave the process.
- **143 test files · 95%+ line coverage** — integration tests run against real in-memory SQLite with the actual migration SQL.

**Lead sources:** OpenData SF · Yelp Fusion · Google Custom Search · ProxyCurl (LinkedIn enrichment)  
**Stack:** Node.js · LangGraph · Fastify · Groq · SQLite · Drizzle ORM · Langfuse · Vitest

---

## Built — Paused

> Complete and production-tested. Paused to focus on Hunter.

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

- **Hybrid architecture** — Retell's GPT-4.1 handles real-time conversation; Claude Haiku handles post-call intelligence. Latency and cost stay decoupled.
- **Idempotent tool handlers** — every CRM write is keyed on `call_id`. Retries never cause duplicates.
- **Validation circuit breaker** — monitors LLM output failures on a rolling window. Trips at 15%, auto-recovers at 8%.
- **2,240 tests · 174 files · 94%+ coverage** — full simulation mode, zero real API calls in CI.

**Stack:** Node.js · Fastify · Retell AI · HubSpot · Google Calendar · Twilio · Claude Haiku · Vitest

---

### 💬 Clara — AI Chat Receptionist

After Hunter sources a lead, Clara generates a personalized demo link. The prospect chats with their own AI receptionist — seeing exactly what their customers would experience — then books an appointment and updates the CRM mid-conversation.

**Stack:** Next.js · LangGraph · Groq · SQLite · HubSpot · Google Calendar

---

## The Factory — How I Build

The systems above run inside a custom development factory: 37+ AI agents, 22 automated hooks, and cross-project governance that treats the entire workspace as a single system.

| Layer | What it does |
|-------|-------------|
| **22 hooks** | Fire on every code change — secret scanning, type checking, console.log detection, test enforcement, tool audit logging |
| **Quality agents** | Code reviewer, TDD enforcer, chaos agent (mutation testing), eval judge |
| **Security agents** | Weekly vuln sweeps, prompt injection testing, HITL gate audits, secret drift detection |
| **Learning propagation** | Fix a bug in one project → auto-scan all others for the same pattern → apply the fix |
| **Observatory** | Single-pane dashboard (Next.js + Langfuse + Recharts) — cost per call/email, eval scores, SLO traffic lights, trace debugging across all projects |
| **Governance by phase** | Live projects: 95%+ coverage, threat models, staging deploys. Explore projects: lightweight guardrails. |

**4,000+ tests across the workspace.** Coverage thresholds enforced in CI, not on the honor system.

---

## Other Projects

| Project | What it does | Stack |
|---------|-------------|-------|
| **Scout** | Autonomous thematic equity discovery — surfaces investment themes across market data | Next.js · LangGraph · Anthropic · SQLite |
| **Appy** | Job application pipeline — fetches, scores, and pre-fills applications on a schedule | Next.js · Ollama · SQLite · Chrome Extension |
| **AI Dose** | Automated Instagram content pipeline — one ready-to-post image and caption per day | Python · Claude · Pillow |
| **SummerCampSync** | AI-powered summer camp finder with smart filtering | Next.js · Gemini |
| **Cracked** | Daily puzzle game with deterministic PRNG seeding | Next.js · Vitest |
| **Crib Sheet** | Upload a room photo, get a personality profile back | Next.js · FastAPI · Moondream 2 |
| **Dashy** | Rhythm platformer game | Phaser 3 · TypeScript · Vite |

---

## Open Source

### 🩻 [clawdoc](https://github.com/ashishjaingithub/clawdoc)

Diagnoses AI agent sessions from JSONL logs. Detects 14 behavioral patterns — retry loops, context exhaustion, cost spikes, tool misuse — and prescribes fixes. Works with any agent producing JSONL logs.

**Stack:** Bash · jq

---

## Tech Stack

```
Runtime         Node.js 22  ·  Python 3.12
Frameworks      Fastify  ·  Next.js  ·  LangGraph
AI Models       Claude (Haiku / Sonnet)  ·  Groq  ·  Retell AI (voice)  ·  Gemini
Database        PostgreSQL  ·  SQLite  ·  Drizzle ORM
Testing         Vitest (4,000+ tests)  ·  Playwright  ·  in-memory SQLite
Observability   Langfuse  ·  Pino (structured + PII redaction)  ·  Sentry
Infrastructure  Railway  ·  Docker  ·  GitHub Actions
```

---

## Get in Touch

Building something that needs an AI agent? Want the factory approach for your team?

[ashish@getcranial.net](mailto:ashish@getcranial.net) · [LinkedIn](https://linkedin.com/in/ashishjainemail)
