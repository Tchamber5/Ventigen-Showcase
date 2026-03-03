# Ventigen — AI-Powered Business Planning

> Turn a business idea into an investor-ready plan in minutes, powered by Claude AI.

[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React_18-61DAFB?style=flat&logo=react&logoColor=black)](https://react.dev/)
[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white)](https://supabase.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![Claude AI](https://img.shields.io/badge/Claude_AI-CC785C?style=flat&logo=anthropic&logoColor=white)](https://www.anthropic.com/)

**[→ Try the live app](https://ventigen.ai)**

---

## What It Does

Most business plan tools give you a blank template. Ventigen gives you a research partner and a writer.

Users start with a research chat powered by Claude — surfacing real market data, competitor names, and cost benchmarks through live web search across 10 structured stages. That context feeds directly into a dual-model generation engine that produces a complete, 9-section investor-ready plan in minutes.

The output isn't a fill-in-the-blank document. It's a coherent narrative built from your actual business context.

---
<img width="1524" height="1260" alt="image" src="https://github.com/user-attachments/assets/ef2f83f3-ffb7-437d-ab13-65c6118b3e8b" />

## Core Features

**AI Research Assistant**
An interactive chat that guides users through 10 structured research stages before any plan is generated. Performs mandatory web searches at every step — surfacing real competitor names, local market data, and cost benchmarks — then feeds that structured context directly into generation. Cold-start generation produces generic output; this doesn't.
<img width="1566" height="1258" alt="image" src="https://github.com/user-attachments/assets/89d568e5-6b9d-4e55-b103-a6ea5c3f6e40" />

**Dual-Model Generation Engine**
The plan generates using two Claude models running in parallel, matched by section type:

| Model | Sections | Rationale |
|---|---|---|
| Claude Sonnet 4 | Executive Summary, Market Analysis, Competitive Analysis, Marketing Strategy | Strategic narrative requiring nuanced reasoning |
| Claude Haiku | Company Description, Products & Services, Financial Projections, Timeline, Funding | Structured, factual output at high speed |

Both calls run in parallel — users wait for the slower one, not both sequentially.

**Guided 6-Step Wizard**
Collects business basics, products/services, target market, competitive landscape, and financials. Auto-saves form state so users never lose progress mid-session.

**Results Editor & PDF Export**
Full inline editing per section, scrollable table of contents, logo upload, and one-click PDF export with markdown rendering, table support, multi-page layout, and Unicode sanitization.

---

## Technical Architecture

```
User → Research Chat (10 stages, live web search)
              ↓
       Structured context collected
              ↓
User → 6-Step Wizard → generate-business-plan (Supabase Edge Function)
                                    │
          ┌─────────────────────────┴──────────────────────────┐
          │                                                      │
   Claude Sonnet 4                                       Claude Haiku
   (Strategic sections)                              (Factual sections)
          │                                                      │
          └─────────────────────────┬──────────────────────────┘
                                    │
                              Results Page
                         (Edit · Export PDF · Share)
```

**Frontend:** React 18 + TypeScript + Vite + Tailwind CSS + shadcn/ui + Framer Motion  
**Backend:** Supabase (PostgreSQL + Auth) + Deno edge functions  
**AI:** Anthropic Claude API (Sonnet 4 + Haiku)  
**PDF:** jsPDF + jsPDF-AutoTable  
**State:** TanStack React Query + React Hook Form + Zod  
**Deployment:** Lovable

---

## Product Design Decisions

**Why two models instead of one?**
Haiku handles structured/factual sections at significantly lower latency, keeping Sonnet's reasoning capacity available where it matters — market analysis, competitive positioning, executive narrative. Running both in parallel means the user waits for the slower one, not both added together.

**Why a research phase before generation?**
Cold-start generation from minimal inputs produces generic, plausible-sounding output that isn't actually useful. The research assistant pre-populates business-specific context — real competitor names, local market data, cost benchmarks — before a single plan section is written. Better context in, better plan out.

**Why enforce competitive analysis structure?**
Free-form competitive analysis is unpredictable to parse and render. A strict prompt format (`COMPETITOR / STRENGTHS / GAPS / ADVANTAGE / MOAT`) makes output reliably renderable as formatted cards in the UI and as structured tables in the PDF — while still reading naturally as prose in the text view.

---

## Built With

- [Claude Code](https://claude.ai/code) — Backend architecture, edge functions, and refactoring
- [Anthropic Claude API](https://www.anthropic.com/) — AI generation and research assistant
- [Supabase](https://supabase.com/) — Database, auth, and serverless edge functions
- [Lovable](https://lovable.dev/) — Frontend iteration and deployment

---

## About

Built by [Tim Chambers](https://timothychambers.me) — AI Product Manager based in Portland, OR.  
Connect on [LinkedIn](https://www.linkedin.com/in/timothy-chambers/).
