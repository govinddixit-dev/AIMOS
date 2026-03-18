# AIMOS – AI Marketing Operating System
## Bubble.io No-Code Development Plan of Action
### Version 1.0 | March 2026

---

## Table of Contents

1. [Document Purpose](#1-document-purpose)
2. [Why Bubble.io for AIMOS](#2-why-bubbleio-for-aimos)
3. [Bubble.io Capability Assessment](#3-bubbleio-capability-assessment)
4. [Scope Definition](#4-scope-definition)
5. [Bubble.io Architecture Design](#5-bubbleio-architecture-design)
6. [Phase-Wise Execution Plan](#6-phase-wise-execution-plan)
7. [Module-by-Module Bubble Implementation](#7-module-by-module-bubble-implementation)
8. [Third-Party API Integrations via Bubble](#8-third-party-api-integrations-via-bubble)
9. [Database Design in Bubble DB](#9-database-design-in-bubble-db)
10. [Team Structure & Resource Allocation](#10-team-structure--resource-allocation)
11. [UI/UX Design Strategy](#11-uiux-design-strategy)
12. [Testing & Quality Assurance Strategy](#12-testing--quality-assurance-strategy)
13. [Deployment & Hosting Strategy](#13-deployment--hosting-strategy)
14. [Security & Compliance Plan](#14-security--compliance-plan)
15. [Risk Assessment & Mitigation](#15-risk-assessment--mitigation)
16. [Milestones & Deliverables Summary](#16-milestones--deliverables-summary)
17. [Cost Estimate (Bubble Plans)](#17-cost-estimate-bubble-plans)
18. [Bubble.io Limitations & When to Migrate](#18-bubbleio-limitations--when-to-migrate)
19. [Success Metrics & KPIs](#19-success-metrics--kpis)
20. [Assumptions & Dependencies](#20-assumptions--dependencies)

---

## 1. Document Purpose

This Plan of Action is a **dedicated execution blueprint for building AIMOS on Bubble.io** — the no-code path recommended in BRD Section 8. It covers how to implement all 12 AI modules using Bubble.io's visual development environment, native database, plugin ecosystem, and API connector — without writing traditional backend code.

> **Scope Note:** This plan assumes Bubble.io handles **both frontend and backend** in a single platform. All business logic, data storage, user management, and API orchestration are built inside Bubble.

**Intended Audience:** Client stakeholders, Product Owner, Bubble.io developers, Project Manager.

---

## 2. Why Bubble.io for AIMOS

### 2.1 BRD Recommendation

The BRD (Section 8) explicitly recommends the following No-Code stack:

| Layer | Recommendation |
|---|---|
| **Frontend + Backend** | Bubble.io (2026 version with built-in AI generator) |
| **Database** | Built-in Bubble DB (PostgreSQL-level) |
| **AI Integrations** | OpenAI/Claude + ElevenLabs + AdCreative.ai + Pictory APIs |
| **Payments** | Stripe |
| **Hosting** | Bubble cloud (99.9% uptime) |

### 2.2 Bubble.io Strengths for This Project

| Advantage | Benefit to AIMOS |
|---|---|
| **Visual full-stack development** | Build UI + logic + database in one place |
| **Built-in user authentication** | RBAC, MFA, social login out-of-the-box |
| **API Connector plugin** | Connect to any REST API (OpenAI, Meta, Google, Stripe) |
| **Workflows (backend logic)** | Define multi-step business logic visually |
| **Scheduled workflows** | Run optimization tasks on a schedule (cron-like) |
| **Repeating Groups** | Dynamic data-driven lists and dashboards |
| **Stripe plugin (native)** | Subscriptions and one-time payments built-in |
| **Responsive design engine** | Mobile-first layouts without code |
| **Fast time-to-market** | MVP achievable in 6–10 weeks vs 12–16 weeks with code |
| **Lower development cost** | Fewer developers needed, faster iteration |

---

## 3. Bubble.io Capability Assessment

Before building, it's critical to understand exactly what Bubble.io **can and cannot** do for AIMOS:

### 3.1 What Bubble.io Handles Natively

| Capability | Bubble.io Support |
|---|---|
| User registration, login, roles | ✅ Built-in |
| Database (data types, relationships) | ✅ Built-in Bubble DB |
| REST API calls (OpenAI, Meta, Stripe, etc.) | ✅ API Connector plugin |
| Multi-step approval workflows | ✅ Workflows engine |
| Scheduled background tasks | ✅ Scheduled workflows / Backend workflows |
| File & media storage | ✅ Built-in (up to plan limits) + S3 plugin |
| Stripe payments & subscriptions | ✅ Official Stripe plugin |
| Email sending | ✅ SendGrid / Postmark plugins |
| Responsive / mobile-first UI | ✅ Responsive design engine |
| Real-time data updates | ✅ Bubble's live data binding |
| Admin panels | ✅ Conditional visibility + admin role |
| Webhooks (inbound) | ✅ API Workflow endpoints |
| Multi-tenant data isolation | ✅ Data privacy rules per role |

### 3.2 What Requires Plugins or Workarounds

| Capability | Approach in Bubble |
|---|---|
| WhatsApp chatbot | Twilio plugin or WhatsApp Business API via API Connector |
| SMS notifications | Twilio plugin |
| AI creative generation (AdCreative.ai, Pictory, ElevenLabs) | API Connector calls + async polling |
| Meta Ads publishing | Meta Marketing API via API Connector |
| Google Ads publishing | Google Ads API via API Connector |
| Background job queue (async) | Backend workflows + Scheduled workflows |
| Advanced analytics charts | Bubble chart elements or ApexCharts plugin |
| PDF/Excel export | PDF Conjurer plugin or external API |

### 3.3 Known Limitations to Plan Around

| Limitation | Mitigation |
|---|---|
| API calls are synchronous by default | Use Backend Workflows + polling pattern for long AI jobs |
| No native task queuing | Scheduled workflows fire at set intervals; use a "jobs" data type |
| Bubble DB query performance degrades at ~500K+ rows | Archive old data; use search constraints carefully |
| Cannot write custom server-side Python/JS logic | All logic must be expressed as Bubble workflows or via API calls |
| Rate limits on API calls depend on Bubble plan | Use Growth/Team plan; implement retry logic in workflows |

---

## 4. Scope Definition

### 4.1 In-Scope (Bubble.io Build)

- All 12 AI Modules implemented as Bubble pages + workflows + API connector calls
- Admin Portal (user management, billing, compliance, analytics)
- Agency/SME Dashboard (mobile-first campaign management)
- End Customer touchpoints (landing pages, chatbot widget)
- AI Creative Generation pipeline (via API calls + async polling)
- Campaign Approval Workflow (multi-step, role-based)
- Payment Processing (Stripe plugin — subscriptions + per-campaign)
- Lead Capture (landing pages, forms, WhatsApp via Twilio)
- AI Sales Agent (chatbot UI + OpenAI API)
- Real-time Analytics Dashboard (charts, KPIs)
- AI Optimization Engine (scheduled workflows)
- GDPR-compliant data handling (privacy rules, data deletion)

### 4.2 Out-of-Scope

- Custom server-side code (no Node.js/Python)
- Native mobile apps (iOS/Android) — Bubble is web only; responsive web covers mobile browser
- TikTok Ads (Phase 2)
- AI Influencer Marketing module
- Predictive ROI ML model (Phase 2 — requires custom code)
- White-label agency portal (Phase 2)

---

## 5. Bubble.io Architecture Design

### 5.1 How Bubble.io Works as Full-Stack

```
┌───────────────────────────────────────────────────────────────────────────┐
│                        BUBBLE.IO PLATFORM                                 │
│                                                                           │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                     FRONTEND (Bubble Pages)                         │  │
│  │                                                                     │  │
│  │  Admin Portal    Agency Dashboard    End Customer Landing Pages     │  │
│  │  (Page: /admin)  (Page: /dashboard)  (Page: /lp/:id)               │  │
│  └──────────────────────────┬──────────────────────────────────────────┘  │
│                             │                                             │
│  ┌──────────────────────────▼──────────────────────────────────────────┐  │
│  │              BACKEND LOGIC (Bubble Workflows)                       │  │
│  │                                                                     │  │
│  │  • User Auth workflows   • Campaign creation workflows              │  │
│  │  • Approval workflows    • Payment workflows (Stripe)               │  │
│  │  • AI call workflows     • Optimization scheduled workflows         │  │
│  │  • Webhook receivers     • Email/SMS trigger workflows              │  │
│  └──────────────────────────┬──────────────────────────────────────────┘  │
│                             │                                             │
│  ┌──────────────────────────▼──────────────────────────────────────────┐  │
│  │              DATABASE (Bubble DB — PostgreSQL-level)                │  │
│  │                                                                     │  │
│  │  Users | Campaigns | Creatives | Leads | Analytics | Brands | etc. │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                             │                                             │
└─────────────────────────────┼─────────────────────────────────────────────┘
                              │ API Connector Calls
        ┌─────────────────────┼──────────────────────────────┐
        ▼                     ▼                              ▼
┌──────────────────┐ ┌────────────────────┐ ┌──────────────────────────────┐
│  AI SERVICES     │ │  AD PLATFORMS      │ │  COMMUNICATION & PAYMENTS    │
│                  │ │                    │ │                              │
│ • OpenAI / Claude│ │ • Meta Marketing   │ │ • Stripe (subscriptions)     │
│ • AdCreative.ai  │ │   API              │ │ • Twilio (SMS + WhatsApp)    │
│ • Pictory        │ │ • Google Ads API   │ │ • SendGrid (email)           │
│ • ElevenLabs     │ │                    │ │ • Calendly (appointments)    │
└──────────────────┘ └────────────────────┘ └──────────────────────────────┘
```

### 5.2 Bubble Page Structure

| Page / Route | User Role | Purpose |
|---|---|---|
| `/` | Public | Marketing landing page + sign-up CTA |
| `/login` | All | Login / MFA |
| `/register` | All | Sign-up wizard |
| `/dashboard` | Agency/SME | Main dashboard — campaigns, leads, ROI |
| `/campaigns/new` | Agency/SME | Campaign creation wizard |
| `/campaigns/:id` | Agency/SME | Campaign detail + performance |
| `/creatives/:id` | Agency/SME | Creative approval screen |
| `/analytics` | Agency/SME | Full analytics dashboard |
| `/leads` | Agency/SME | Lead management |
| `/sales-agent` | Agency/SME | AI Sales Agent configuration |
| `/billing` | Agency/SME | Subscription + invoices |
| `/onboarding` | Agency/SME | Onboarding wizard (3 steps) |
| `/lp/:slug` | End Customer | AI-generated landing pages |
| `/chat/:id` | End Customer | AI Sales Agent chat |
| `/admin` | Admin | Admin control center |
| `/admin/users` | Admin | User management |
| `/admin/analytics` | Admin | Platform-wide analytics |
| `/admin/billing` | Admin | Billing oversight |

### 5.3 Bubble Workflow Architecture Pattern (for AI Jobs)

Since Bubble cannot run true background jobs, AI creative generation (which takes 10–60s) uses this pattern:

```
User triggers "Generate Creatives"
        │
        ▼
Create "Job" record in DB  →  Status: "pending"
        │
        ▼
Trigger Backend Workflow   →  Calls AdCreative.ai / Pictory API
        │
        ▼
API returns result          →  Update "Job" record → Status: "complete" + URLs
        │
        ▼
Frontend polls Job record   →  Page refreshes when Status = "complete"
        │
        ▼
Display creatives to user   →  User selects and approves
```

---

## 6. Phase-Wise Execution Plan (8-Week Sprint)

> **Strategy:** Compress the 34-week standard timeline into 8 aggressive weeks by running parallel workstreams, combining related modules into single sprints, and having full-team focus every day. This requires **all API keys and third-party accounts ready before Day 1**, pre-approved Figma mockups, and zero stakeholder decision delays.

### Prerequisites (Must Be Completed BEFORE Week 1)

| Item | Owner | Status Required |
|---|---|---|
| All API keys collected (OpenAI, Meta, Google, Stripe, AdCreative.ai, Pictory, ElevenLabs, Twilio, SendGrid, Calendly) | Client | ✅ Ready |
| Stripe account verified + test products created | Client | ✅ Ready |
| Meta Business Manager + App Review submitted | Client | ✅ In progress |
| Google Ads Developer Token obtained | Client | ✅ Ready |
| WhatsApp Business API approval submitted | Client | ✅ In progress |
| Figma mockups for all 18 pages approved | Designer | ✅ Ready |
| Bubble.io Team plan ($349/mo) purchased | Client | ✅ Ready |
| Full team (7 members) onboarded and available | PM | ✅ Ready |

---

### Week 1 — Foundation, Auth & Admin Portal

> **Goal:** Platform skeleton fully operational — users can register, log in, and admin can manage users.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | Bubble workspace setup, DB schema (all 20 data types), privacy rules | API Connector setup — OpenAI, AdCreative.ai, Pictory, ElevenLabs | API Connector setup — Meta, Google Ads, Twilio, SendGrid, Calendly | Design system in Bubble (colors, fonts, reusable header/nav/cards) | Test environment setup, create test accounts per role |
| **Tue** | Auth workflows (sign-up, login, email verification, Google OAuth) | Stripe plugin setup (3 plans, webhook endpoint, test products) | MFA plugin setup + user role assignment workflow | Responsive header + sidebar reusable elements | Validate all API Connector "Initialize call" tests pass |
| **Wed** | RBAC logic — page-level access, conditional visibility | Admin dashboard page (platform KPIs, user counts, revenue widget) | User profile page + edit workflow | Login + register page design | Test auth flows — register, verify, login, role-based redirect |
| **Thu** | Admin user management page (list, search, suspend/activate) | Admin analytics page (signup trend chart, revenue chart) | Admin billing page (payment records, filters, plan distribution) | Admin portal pages design polish | Test privacy rules — cross-tenant data isolation per role |
| **Fri** | Code review + workflow cleanup | Bug fixes from QA | Bug fixes from QA | Mobile responsive QA on all pages built | **Week 1 Sign-Off: Auth + Admin fully tested** |

**Week 1 Deliverable:** User auth (email + Google OAuth + MFA), RBAC, Admin portal (4 pages), Stripe configured, all API Connectors tested.

---

### Week 2 — Onboarding, Business Analyzer & Brand Builder

> **Goal:** New Agency user completes onboarding and gets an AI-generated brand kit.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | Onboarding wizard — Step 1 (business profile form + save to Agency data type) | AI Business Analyzer — form page (industry, competitors, audience) | Brand Builder — brand brief form (name, industry, tone, colors) | Onboarding wizard UI (3-step progress indicator, mobile layout) | Write test cases for onboarding, analyzer, brand builder |
| **Tue** | Onboarding wizard — Step 2 (social account linking: Meta BM ID, Google Ads ID) | Business Analyzer — OpenAI API workflow (send prompt → parse JSON → store `BusinessAnalysis`) | Brand Builder — DALL-E API workflow (logo generation) + Claude workflow (brand voice) | Business Analyzer results page design | Test onboarding — save/resume, skip, validation |
| **Wed** | Onboarding wizard — Step 3 (notification preferences) + completion redirect | Business Analyzer — results display page (expandable sections, platform recs, budget ranges) | Brand Builder — brand kit display page (logo, colors, voice, download buttons) | Brand kit page design + download button styling | Test AI API calls — verify prompts, response parsing, error handling |
| **Thu** | Onboarding complete flag + redirect logic | Polish analyzer page + edge case handling | Brand kit PDF export workflow (via plugin or external API) | Responsive QA on all new pages | Full flow test: register → onboard → analyze → brand kit |
| **Fri** | Integration testing + workflow cleanup | Bug fixes | Bug fixes | Design review | **Week 2 Sign-Off: Modules 1 & 2 complete** |

**Week 2 Deliverable:** 3-step onboarding wizard, AI Business Analyzer (Module 1), AI Brand Builder (Module 2).

---

### Week 3 — AI Content Studio & Campaign Builder

> **Goal:** Agency can generate AI creatives (images, videos, voiceovers) and build campaigns.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | Content Studio — creative brief form + `CreativeJob` record creation (async pattern) | Campaign Builder — Step 1 & 2 (objective dropdown + audience targeting form) | Social Media Manager — post composer form + `ScheduledPost` data type | Content Studio creative cards design (variations grid) | Write test cases for content studio, campaign builder |
| **Tue** | Content Studio — AdCreative.ai API call (graphics) + backend workflow + polling every 5s | Campaign Builder — Step 3 & 4 (budget/duration + creative selector) | Social Media Manager — calendar plugin setup + scheduling workflow | Campaign wizard step-by-step UI design | Test async polling pattern with AdCreative.ai |
| **Wed** | Content Studio — Pictory API (video) + ElevenLabs API (voiceover) integration | Campaign Builder — Step 5 (review page) + Meta Marketing API publish workflow | Social Media Manager — Scheduled Workflow (auto-publish at scheduled time via Meta API) | Social media calendar view design | Test video/voiceover generation + display in Bubble |
| **Thu** | Content Studio — variation display (image cards, video player, audio player, approve button) | Campaign Builder — Google Ads API publish workflow + campaign status tracking page | Social Media Manager — hashtag suggestion via OpenAI + engagement tracking | Mobile responsive polish | Full test: brief → generate → approve → create campaign → publish |
| **Fri** | Creative approval workflow (approve/reject/request-changes + email notifications) | Campaign pause/resume workflow + status webhook handling | SM post list view (status column, edit, delete) | Design review | **Week 3 Sign-Off: Modules 3, 4, 5 complete** |

**Week 3 Deliverable:** AI Content Studio (Module 3), AI Campaign Builder (Module 4), AI Social Media Manager (Module 5).

---

### Week 4 — Payments, Lead Capture & Landing Pages

> **Goal:** Stripe payments live, leads captured from AI-generated landing pages.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | Stripe subscription plans page (3 tiers, feature comparison, upgrade/downgrade) | Lead Capture — landing page builder (headline, body, form config, slug) | Landing page template rendering (`/lp/:slug` with dynamic content) | Billing page design (plans, invoices, payment history) | Write test cases for payments + lead capture |
| **Tue** | Per-campaign payment flow (one-time Stripe charge before campaign publish) | Lead Capture — form submission workflow (create `Lead` record, set score, set status) | Lead scoring workflow (score 0–100 based on data completeness + source) | Landing page template designs (3 variants) | Test Stripe flows — subscribe, charge, invoice, webhook |
| **Wed** | Stripe webhook handler (`invoice.payment_succeeded`, `payment_failed`, `subscription.deleted`) | Lead list page (filters: status, score, date range, search) | Lead notification — SendGrid email on new lead + in-app `Notification` record | Lead management page design (list + pipeline) | Test lead capture — form submit → DB record → email → score |
| **Thu** | Invoice history page + Stripe receipt links | CRM pipeline view (Kanban: New → Contacted → Qualified → Converted) | WhatsApp integration — Twilio API (inbound webhook + outbound message workflow) | Kanban pipeline design + mobile layout | Test WhatsApp inbound → lead creation flow |
| **Fri** | Payment lockout logic (lock features after 3 failed payments) | Bug fixes + Polish | SMS notification workflow via Twilio | Design review | **Week 4 Sign-Off: Payments + Module 6 complete** |

**Week 4 Deliverable:** Full Stripe billing (subscriptions + per-campaign), AI Lead Capture System (Module 6), WhatsApp/SMS integration.

---

### Week 5 — AI Sales Agent & Customer Engagement

> **Goal:** AI chatbot qualifies leads and engagement engine nurtures them via email/SMS/WhatsApp.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | AI Sales Agent — chat page (`/chat/:id`), `ChatSession` + `ChatMessage` data types, conversation UI (repeating group) | Engagement Engine — campaign builder form (channel, template, audience filter, schedule) | AI Agent Config page (system prompt, tone, booking toggle, Calendly URL) | Chat widget UI design (bubbles, typing indicator, input bar) | Write test cases for chatbot + engagement |
| **Tue** | AI Sales Agent — OpenAI GPT-4 workflow (build last 10 messages as context → call API → store reply → display) | Engagement Engine — Backend Workflow: fetch matching leads → loop → call SendGrid per lead (email) | Agent Config — save/edit workflow + link to Agency data type | Engagement campaign builder design | Test chatbot conversation flow (multi-turn) |
| **Wed** | AI Sales Agent — typing indicator (show during API call), session handling, conversation reset | Engagement Engine — Twilio SMS loop workflow + WhatsApp loop workflow | Engagement message logging (`EngagementMessage` records with delivery status) | Engagement analytics view design | Test engagement: email + SMS + WhatsApp delivery |
| **Thu** | Lead qualification workflow (detect buying intent keywords → update lead status → trigger Calendly booking link) | Engagement analytics view (delivery stats: sent, failed, open rate) | Product recommendation workflow (based on lead score + chat context) | Mobile responsive polish on chat + engagement pages | Full flow: ad → lead → chat → qualify → book appointment |
| **Fri** | Integration testing across Modules 6–8 | Bug fixes | Bug fixes | Design review | **Week 5 Sign-Off: Modules 7 & 8 complete** |

**Week 5 Deliverable:** AI Sales Agent (Module 7), AI Customer Engagement Engine (Module 8).

---

### Week 6 — Analytics, Optimization & Growth Planner

> **Goal:** Real-time analytics, self-optimizing campaigns, AI growth reports.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | Analytics Engine — Scheduled Workflow (hourly): pull Meta + Google Ads metrics → store in `CampaignMetric` | Optimization Engine — rules config page (metric, operator, threshold, action) + `OptimizationRule` CRUD | Growth Planner — Backend Workflow: on campaign end → collect metrics → build summary | Analytics dashboard design (KPI cards, charts, filters) | Write test cases for analytics, optimization, growth |
| **Tue** | Analytics dashboard — KPI summary cards (impressions, clicks, CTR, spend, conversions, CPL, ROI) | Optimization Engine — Scheduled Workflow (every 4 hours): evaluate rules → call Meta/Google API to pause/boost | Growth Planner — OpenAI GPT-4 call (generate growth report from metrics summary) | Optimization config page design + action log | Test scheduled metric polling + data storage |
| **Wed** | Analytics dashboard — ApexCharts plugin (performance over time, platform comparison, date range filter) | Optimization Engine — `OptimizationLog` records + email notification on action taken | Growth Planner — report display page (what worked, recommendations, next steps, "Plan Next Campaign" CTA) | Growth report page design | Test optimization rules → trigger → API action → log |
| **Thu** | Analytics — CSV export button + per-campaign drill-down views | Optimization action log page (all actions with timestamps) | Growth Planner — pre-fill campaign wizard with AI recommendations | Mobile responsive polish | Full flow: campaign → metrics → optimize → report → next campaign |
| **Fri** | Integration testing Modules 9–11 | Bug fixes | Bug fixes | Design review | **Week 6 Sign-Off: Modules 9, 10, 11 complete** |

**Week 6 Deliverable:** AI Analytics Engine (Module 9), AI Optimization Engine (Module 10), AI Growth Planner (Module 11).

---

### Week 7 — Unified Dashboard, Security & QA

> **Goal:** Final module complete, security hardened, all privacy rules verified, GDPR compliant.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | AI Business Dashboard — unified page (active campaigns, pending approvals, lead funnel, ROI summary, notification feed) | GDPR — consent checkbox on sign-up (store timestamp), "Export My Data" page (compile all records → email JSON via SendGrid) | GDPR — "Delete My Account" cascade workflow (remove all records across all 20 data types for user's agency) | Dashboard design (widget layout, quick action buttons, notification feed) | Begin full regression test across all 12 modules |
| **Tue** | Dashboard — pre-computed aggregate fields (total_leads, active_campaigns_count) updated by backend workflows | Privacy rules audit — log in as each role → verify data isolation → document results | Backend Workflow auth audit — verify all API Workflow endpoints require auth token | Dashboard mobile responsive | Continue regression testing + log all bugs |
| **Wed** | Dashboard — quick action buttons (Create Campaign, View Leads, Generate Creatives) + notification center | Stripe webhook signature verification review | Performance optimization — repeating group pagination, search constraints, reduce unnecessary API calls | Privacy Policy page + Terms of Service page | Test GDPR flows: export data, delete account, consent recording |
| **Thu** | Dashboard — role-conditional views (Agency sees own data, Admin sees platform aggregates) | Security review — test role bypass scenarios, check all sensitive pages for redirect on unauthorized access | Performance testing — simulate multi-user load, monitor Bubble capacity dashboard | Final design polish across all pages | Full end-to-end test: register → onboard → analyze → brand → creative → campaign → lead → chat → analytics → report |
| **Fri** | All critical bug fixes | All critical bug fixes | All critical bug fixes | Final responsive QA | **Week 7 Sign-Off: Module 12 complete, security & GDPR cleared** |

**Week 7 Deliverable:** AI Business Dashboard (Module 12), GDPR compliance, security audit, privacy rules verified, performance optimized.

---

### Week 8 — UAT, Bug Fixes & Launch

> **Goal:** Client accepts the product, go live on production.

| Day | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| **Mon** | Client UAT kickoff — walk through all 12 modules live + hand off test credentials | Support UAT — answer client questions, reproduce reported issues | Support UAT — assist with edge case testing | Prepare demo video / walkthrough recording | Facilitate UAT — track all client-reported issues |
| **Tue** | Fix critical UAT bugs (P0/P1) | Fix medium UAT bugs (P2) | Fix medium UAT bugs (P2) | Fix any design feedback from client | Verify each fixed bug, update regression results |
| **Wed** | Fix remaining UAT bugs + final workflow cleanup | Custom domain setup (DNS CNAME, SSL verification) | Switch all API keys from sandbox/test → production keys | Final screenshots for documentation | Smoke test on production-bound configuration |
| **Thu** | Switch Stripe from test mode → live mode | Deploy Development branch → Live branch (create saved version snapshot first) | Set up UptimeRobot monitoring + Bubble capacity alerts | Handover document: Bubble editor structure, page map, workflow list | Full smoke test on Live branch — every module |
| **Fri** | **🚀 Production Launch** — custom domain live, invite first batch of clients | Monitor Live Bubble capacity logs | Monitor API error rates + Stripe webhooks | Publish marketing landing page at `/` | **Week 8 Sign-Off: AIMOS is LIVE** |

**Week 8 Deliverable:** Client UAT approved, all bugs fixed, production domain live, Stripe live, platform publicly accessible, first clients onboarded.

---

### 8-Week Timeline Summary

```
Week 1  ██████████  Foundation + Auth + Admin Portal
Week 2  ██████████  Onboarding + Business Analyzer + Brand Builder
Week 3  ██████████  Content Studio + Campaign Builder + Social Media
Week 4  ██████████  Payments + Lead Capture + Landing Pages + WhatsApp
Week 5  ██████████  AI Sales Agent + Engagement Engine
Week 6  ██████████  Analytics + Optimization + Growth Planner
Week 7  ██████████  Dashboard + Security + GDPR + Full QA
Week 8  ██████████  UAT + Bug Fixes + Production Launch 🚀
```

### Parallel Workstream Map

| Week | Lead Dev | Mid Dev 1 | Mid Dev 2 | Designer | QA |
|---|---|---|---|---|---|
| 1 | DB + Auth + RBAC | APIs + Stripe | APIs + MFA | Design system | Test env + API validation |
| 2 | Onboarding | Business Analyzer | Brand Builder | Page designs | Flow testing |
| 3 | Content Studio | Campaign Builder | Social Media Mgr | Creative/campaign UI | Module 3–5 testing |
| 4 | Stripe billing | Lead capture + CRM | Landing pages + WhatsApp | Billing + lead UI | Payment + lead testing |
| 5 | AI Sales Agent | Engagement Engine | Agent config + recs | Chat + engagement UI | Module 7–8 testing |
| 6 | Analytics Engine | Optimization Engine | Growth Planner | Analytics/report UI | Module 9–11 testing |
| 7 | Dashboard | GDPR + privacy audit | Security + performance | Dashboard + polish | Full regression |
| 8 | UAT bugs + launch | Domain + prod keys | API prod switch | Docs + demo | UAT + smoke test |

### Key Assumptions for 8-Week Delivery

1. **All API keys, credentials, and third-party accounts are ready before Week 1** — any delay here cascades into subsequent weeks
2. **Figma mockups are pre-approved** — designer implements directly in Bubble, no round-trip design reviews
3. **Full team (7 members) is 100% dedicated** — no part-time allocation or context switching to other projects
4. **Stakeholder decisions are made same-day** — no multi-day approval cycles during the sprint
5. **Bubble.io Team plan ($349/mo) is active from Day 1** — needed for team collaboration and backend workflow capacity
6. **Daily standups are mandatory** — blockers must surface within 24 hours
7. **Scope is locked** — zero new feature requests during the 8 weeks; anything new goes to Phase 2 backlog
8. **Client is available for UAT in Week 8** — no delays in testing and sign-off

---

## 7. Module-by-Module Bubble Implementation

### Module 1: AI Business Analyzer

| Component | Bubble Implementation |
|---|---|
| **UI** | Multi-field form page (industry, location, competitors, audience) |
| **Logic** | Workflow: On submit → Call OpenAI API (prompt: business analysis) → Store result in `BusinessAnalysis` data type |
| **Output Display** | Dynamic text elements showing recommendations, platform suggestions, budget ranges |
| **Data Types** | `BusinessAnalysis` (fields: industry, competitors, strategy_output, platform_recommendations) |

### Module 2: AI Brand Builder

| Component | Bubble Implementation |
|---|---|
| **UI** | Brand brief form, brand kit results page, asset download buttons |
| **Logic** | Workflow: On submit → Call OpenAI DALL-E (logo) + Claude (brand voice) → Store results → Display kit |
| **Data Types** | `BrandKit` (fields: logo_url, colors, brand_voice, template_urls) |
| **Storage** | Bubble File Manager (for logo images), CDN-served |

### Module 3: AI Content Studio

| Component | Bubble Implementation |
|---|---|
| **UI** | Creative brief form, async loading screen (polling), variation cards (image/video/audio), approve buttons |
| **Logic** | Workflow → Create `CreativeJob` record (status: pending) → Call AdCreative.ai/Pictory/ElevenLabs APIs → Update job record with URLs → Frontend polls every 5s until complete |
| **Data Types** | `CreativeJob` (fields: status, type, variations_urls, approved_url, campaign_id) |

### Module 4: AI Campaign Builder

| Component | Bubble Implementation |
|---|---|
| **UI** | Campaign wizard (objective → audience → budget → platform selection → review) |
| **Logic** | Workflow: Save campaign → Call Meta Marketing API (create campaign) + Google Ads API → Store campaign IDs |
| **Data Types** | `Campaign` (fields: objective, budget, status, meta_campaign_id, google_campaign_id, platform, approved_creative_url) |

### Module 5: AI Social Media Manager

| Component | Bubble Implementation |
|---|---|
| **UI** | Calendar view (calendar plugin), post composer, scheduling form |
| **Logic** | Scheduled Workflow: At post time → Call Meta Graph API (publish post) → Update post status |
| **Data Types** | `ScheduledPost` (fields: content, media_url, platform, scheduled_time, status, hashtags) |

### Module 6: AI Lead Capture System

| Component | Bubble Implementation |
|---|---|
| **UI** | Landing page template builder (page with dynamic content), form embed, `/lp/:slug` routing |
| **Logic** | On form submit → Create `Lead` record → Send notification email → Run lead scoring workflow |
| **Data Types** | `Lead` (fields: name, email, phone, source, score, status, campaign_id), `LandingPage` (fields: slug, config, form_fields) |

### Module 7: AI Sales Agent

| Component | Bubble Implementation |
|---|---|
| **UI** | Chat widget page, conversation bubbles (repeating group), typing indicator, input bar |
| **Logic** | On user message → Store in `ChatMessage` → Build conversation history → Call OpenAI GPT-4 API (with context) → Store AI reply → Display |
| **Data Types** | `ChatSession` (fields: lead_id, status), `ChatMessage` (fields: session_id, role, content, timestamp) |

### Module 8: AI Customer Engagement Engine

| Component | Bubble Implementation |
|---|---|
| **UI** | Campaign builder UI, message template editor, schedule picker, analytics view |
| **Logic** | Scheduled Workflow: On trigger date → Loop through contact list → Call SendGrid API (email) / Twilio API (SMS) → Log delivery |
| **Data Types** | `EngagementCampaign` (fields: type, template, scheduled_time, status), `EngagementMessage` (fields: campaign_id, contact, status) |

### Module 9: AI Analytics Engine

| Component | Bubble Implementation |
|---|---|
| **UI** | Charts (ApexCharts plugin), KPI cards, date range picker, campaign filter |
| **Logic** | Scheduled Workflow (hourly): Pull metrics from Meta/Google APIs → Store in `CampaignMetric` records |
| **Data Types** | `CampaignMetric` (fields: campaign_id, date, impressions, clicks, ctr, cpl, conversions, spend, roi) |

### Module 10: AI Optimization Engine

| Component | Bubble Implementation |
|---|---|
| **UI** | Optimization rules config page, action log list, suggestions panel |
| **Logic** | Scheduled Workflow (every 4 hours): For each active campaign → Check CTR/CPL thresholds → If underperforming: Call Meta/Google API to pause → Log action → Notify client |
| **Data Types** | `OptimizationRule` (fields: metric, operator, threshold, action), `OptimizationLog` (fields: campaign_id, action_taken, timestamp) |

### Module 11: AI Growth Planner

| Component | Bubble Implementation |
|---|---|
| **UI** | Growth report page, recommendation cards, next campaign CTA |
| **Logic** | Backend Workflow (triggered on campaign end): Pull campaign metrics → Build summary prompt → Call OpenAI GPT-4 → Store recommendations |
| **Data Types** | `GrowthReport` (fields: campaign_id, best_channel, best_content_type, recommendations_text, next_steps) |

### Module 12: AI Business Dashboard

| Component | Bubble Implementation |
|---|---|
| **UI** | Single dashboard page aggregating data from all modules — active campaigns, pending approvals, lead count, ROI summary, notification feed |
| **Logic** | Dynamic data pulls from all data types with role-based visibility conditions |
| **Performance** | Pre-computed aggregate fields (total_leads, active_campaigns_count) updated by workflows to avoid slow live queries |

---

## 8. Third-Party API Integrations via Bubble

All integrations use Bubble's **API Connector plugin**. Here is the setup guide per service:

| Integration | Setup Method | Auth Type | Key Calls |
|---|---|---|---|
| **OpenAI / Claude** | API Connector | Bearer Token | `POST /chat/completions`, `POST /images/generations` |
| **AdCreative.ai** | API Connector | API Key header | `POST /creative/generate`, `GET /creative/{id}/status` |
| **Pictory** | API Connector | API Key header | `POST /video/create`, `GET /video/{id}/status` |
| **ElevenLabs** | API Connector | API Key header | `POST /text-to-speech/{voice_id}` |
| **Meta Marketing API** | API Connector | OAuth 2.0 Bearer | `POST /campaigns`, `POST /adsets`, `POST /ads`, `GET /{campaign_id}/insights` |
| **Google Ads API** | API Connector | OAuth 2.0 | `POST /campaigns:mutate`, `GET /googleAds:searchStream` |
| **Stripe** | Stripe Official Plugin | Stripe keys | Subscriptions, charges, webhooks (built into plugin) |
| **Twilio (SMS + WhatsApp)** | API Connector | Basic Auth | `POST /Messages.json` |
| **SendGrid** | API Connector or Plugin | Bearer Token | `POST /mail/send` |
| **WhatsApp Inbound** | API Workflow Endpoint | Webhook | Receive messages, trigger lead workflows |
| **Calendly** | API Connector | Bearer Token | `POST /scheduling_links`, `GET /scheduled_events` |
| **Google Calendar** | API Connector | OAuth 2.0 | `POST /events`, `GET /events` |

### API Connector Configuration Steps (Per Integration)

```
1. Go to Plugins → API Connector in Bubble editor
2. Add new API → Enter base URL + auth headers
3. Define each call (method, URL, headers, body parameters)
4. Mark dynamic parameters (those passed per workflow call)
5. Click "Initialize call" to test with sample data
6. Shared headers (Bearer token) set once at API level
```

---

## 9. Database Design in Bubble DB

### 9.1 Core Data Types

| Data Type | Key Fields |
|---|---|
| **User** | email, name, role (Admin/Agency/Customer), agency_id, mfa_enabled, subscription_status |
| **Agency** | name, industry, website, logo, brand_kit, onboarding_complete |
| **BusinessAnalysis** | agency_id, industry, audience, competitors, strategy_output, platforms_recommended |
| **BrandKit** | agency_id, logo_url, color_palette, brand_voice, templates |
| **Campaign** | agency_id, name, objective, budget, status, platform, meta_campaign_id, google_campaign_id, creative_id, approval_status |
| **CreativeJob** | campaign_id, type (graphic/video/voice), status, variations_urls (list), approved_url |
| **CampaignApproval** | campaign_id, submitted_by, reviewed_by, status, notes |
| **ScheduledPost** | agency_id, content, media_url, platform, scheduled_at, posted_at, status, hashtags |
| **Lead** | agency_id, campaign_id, name, email, phone, source, score, status, created_at |
| **LandingPage** | agency_id, slug, title, headline, form_config, campaign_id, active |
| **ChatSession** | lead_id, agency_id, status (active/closed), agent_config_id |
| **ChatMessage** | session_id, role (user/assistant), content, created_at |
| **AIAgentConfig** | agency_id, system_prompt, tone, booking_enabled, calendar_url |
| **EngagementCampaign** | agency_id, type (email/sms/whatsapp), template, scheduled_at, status, audience_filter |
| **CampaignMetric** | campaign_id, date, impressions, clicks, ctr, cpl, conversions, spend, roi |
| **OptimizationRule** | agency_id, metric, operator, threshold, action, active |
| **OptimizationLog** | campaign_id, rule_id, action_taken, old_value, new_value, timestamp |
| **GrowthReport** | campaign_id, agency_id, best_channel, best_content_type, recommendations, next_steps |
| **Payment** | agency_id, amount, type (subscription/campaign), stripe_charge_id, status, created_at |
| **Notification** | user_id, type, message, read, created_at |

### 9.2 Data Privacy Rules (RBAC in Bubble)

| Data Type | Admin | Agency User | End Customer |
|---|---|---|---|
| User | All fields | Own record only | Own record only |
| Campaign | All | Own agency campaigns | None |
| Lead | All | Own agency leads | Own record |
| CampaignMetric | All | Own agency metrics | None |
| Payment | All | Own agency payments | None |

---

## 10. Team Structure & Resource Allocation

### 10.1 Recommended Team Composition

| Role | Count | Responsibility |
|---|---|---|
| **Project Manager** | 1 | Sprint planning, stakeholder communication, risk management |
| **Product Owner** | 1 | Backlog prioritization, acceptance criteria, client alignment |
| **Lead Bubble Developer** | 1 | Architecture decisions, complex workflow design, API connector setup, code review |
| **Mid-Level Bubble Developer** | 2 | Page building, workflow implementation, plugin configuration |
| **UI/UX Designer** | 1 | Figma mockups, design system, responsive design implementation in Bubble |
| **QA Tester** | 1 | Manual testing, workflow validation, cross-device testing |

**Total Core Team: 7 members** *(vs. 12+ for a code-based build)*

### 10.2 Bubble Developer Skill Requirements

Bubble developers should be proficient in:
- Bubble.io editor (pages, reusable elements, workflows, backend workflows)
- API Connector plugin (REST API setup, dynamic parameters, authentication)
- Bubble DB design (data types, relationships, privacy rules)
- Responsive design engine (column/row layout, breakpoints)
- Plugin ecosystem (Stripe, SendGrid, ApexCharts, Toolbox, etc.)
- Scheduled workflows & backend workflows (for async operations)
- Performance optimization (search constraints, client-side vs server-side workflows)

### 10.3 Resource Allocation by Phase

| Phase | Key Resources | Focus |
|---|---|---|
| Phase 0 (Discovery) | PM, PO, Lead Dev, Designer | Setup, API config, DB schema, design system |
| Phase 1 (Auth + Brand) | Full team | User system, onboarding, AI brand builder |
| Phase 2 (Campaigns) | Full team | Campaign creation, payment, publishing |
| Phase 3 (Leads + Agent) | Full team | Lead capture, sales agent, engagement |
| Phase 4 (Analytics) | Lead Dev + QA | Analytics, optimization engine, dashboard |
| Phase 5 (Hardening) | Lead Dev + QA | Security, GDPR, performance |
| Phase 6 (Launch) | Full team (reduced) | Deployment, monitoring, fixes |

---

## 11. UI/UX Design Strategy

### 11.1 Design Principles

1. **Mobile-First:** All pages designed for 390px (iPhone) first, scaled to desktop
2. **10-Minute Campaign:** Wizard-style UX — 5 steps max to launch a campaign
3. **Consistency:** Bubble reusable elements (header, nav, cards, buttons) for uniform UI
4. **AI-Guided:** AI recommendations pre-fill fields to reduce cognitive load
5. **Fast Load:** Minimize data loaded on page open; use pagination and lazy loading

### 11.2 Bubble Design System Setup

| Element | Configuration |
|---|---|
| **Colors** | Set brand colors in Bubble's design tab (primary, secondary, success, warning, error) |
| **Typography** | Google Fonts via Bubble settings; define heading/body/caption styles |
| **Buttons** | Reusable element: Primary, Secondary, Danger, Ghost variants |
| **Cards** | Reusable element: Campaign card, Lead card, Analytics card |
| **Navigation** | Reusable header (role-conditional menu items) |
| **Forms** | Consistent input styles with validation states |
| **Loading States** | Spinner reusable element + conditional visibility during API calls |

### 11.3 Key Screen Flows

**Campaign Creation Wizard (5 Steps):**
```
Step 1: Campaign Objective (dropdown + description)
     ↓
Step 2: Target Audience (age, location, interests)
     ↓
Step 3: Budget & Duration (input + slider)
     ↓
Step 4: AI Creative Generation (auto-triggered, async polling)
     ↓
Step 5: Review & Approve (creative preview, edit, approve button)
```

**Creative Approval Screen:**
- Side-by-side cards for 3–10 variations
- Each card: thumbnail/preview + Play button (video/audio) + "Approve" button
- Selected variation highlighted
- "Proceed to Payment" CTA at bottom

---

## 12. Testing & Quality Assurance Strategy

### 12.1 Testing Approach in Bubble

Since Bubble has no unit testing framework, QA is primarily **manual with structured test plans** + Postman for API connector validation.

| Test Type | Method | Coverage |
|---|---|---|
| **Workflow Testing** | Bubble's built-in "debug mode" + run step-by-step | All workflows (happy path + error path) |
| **API Connector Testing** | Bubble's "Initialize call" + Postman for raw API validation | All external API calls |
| **Role-Based Access** | Log in as each role and verify visible data & actions | Admin, Agency, Customer |
| **Privacy Rules** | Search for other users' data while logged in as different role | All sensitive data types |
| **Stripe Payment Flows** | Stripe test mode with test card numbers | Subscribe, charge, invoice, webhook |
| **Responsive Design** | Chrome DevTools device simulation + real iPhone/Android | Mobile, tablet, desktop |
| **Load Testing** | Bubble's capacity monitor + manual multi-user simulation | Key workflows under concurrent use |
| **Webhook Testing** | ngrok for local testing; Stripe CLI for Stripe webhooks | Stripe, Meta, WhatsApp webhooks |

### 12.2 Quality Gates Per Phase

Each phase must pass before proceeding:
- [ ] All workflows complete without errors in debug mode
- [ ] Privacy rules validated — no cross-tenant data leaks
- [ ] All API connector calls return expected responses
- [ ] Key pages render correctly on mobile and desktop
- [ ] Stripe test payments succeed end-to-end
- [ ] No broken references or missing data in conditions

---

## 13. Deployment & Hosting Strategy

### 13.1 Bubble Hosting & Plans

| Bubble Plan | Price | Key Features | Recommended For |
|---|---|---|---|
| **Starter** | ~$29/mo | 1 app, basic capacity, bubble.io subdomain | Development only |
| **Growth** | ~$119/mo | Custom domain, 10x capacity, API workflow | MVP / Beta launch |
| **Team** | ~$349/mo | Collaboration, version control, higher capacity | Production with team |
| **Production** | ~$849/mo | Dedicated capacity, priority support, SLA | Full commercial launch |

> **Recommendation:** Start on **Growth** for beta launch; upgrade to **Team** or **Production** before full commercial launch based on user growth.

### 13.2 Custom Domain Setup

```
1. Purchase domain (e.g., aimos.io)
2. In Bubble: Settings → Domain/Email → Add custom domain
3. Update DNS: Add CNAME record pointing to Bubble's servers
4. SSL is automatically provisioned by Bubble (Let's Encrypt)
5. Set up subdomain for client landing pages: lp.aimos.io
```

### 13.3 Version Control & Backups

- Bubble has built-in **version history** (Development vs Live branches)
- Always develop on **Development** branch; deploy to **Live** after testing
- Create a **saved version** (snapshot) before every major release
- Enable **automatic database backups** (Team plan and above)

### 13.4 Environment Strategy

| Environment | Branch | Access |
|---|---|---|
| **Development** | Bubble Development branch | Dev team |
| **Staging (UAT)** | Bubble Development branch shared with client | Dev + Client |
| **Production** | Bubble Live branch | End users |

---

## 14. Security & Compliance Plan

### 14.1 Security Controls in Bubble

| Control | Bubble Implementation |
|---|---|
| **Authentication** | Bubble's built-in auth (email/password + Google OAuth + MFA via plugin) |
| **RBAC** | User roles field + privacy rules per data type + conditional page visibility |
| **API Key Security** | Store all API keys in Bubble's **Environment Variables** (never hardcoded in workflows) |
| **HTTPS** | Automatic on all Bubble-hosted apps (TLS) |
| **Payment Security** | Stripe plugin — card data never touches Bubble DB; only Stripe tokens stored |
| **Backend Workflow Auth** | All API Workflow endpoints require authentication token |
| **Webhook Signature Verification** | Stripe webhook signature checked in workflow; Meta hub verify token |
| **Data Privacy Rules** | Per data type — restrict who can view/edit/create/delete each record |

### 14.2 GDPR Compliance in Bubble

| Requirement | Bubble Implementation |
|---|---|
| Consent Management | Checkbox on sign-up form; store consent timestamp in User record |
| Right to Access | "Export my data" page — search all data types by user → format as CSV → email link |
| Right to Erasure | "Delete account" workflow — cascade delete all user data across all data types |
| Data Minimization | Only collect required fields; no unnecessary data types |
| Data Processing Notice | Privacy Policy page linked from footer and sign-up |

---

## 15. Risk Assessment & Mitigation

| # | Risk | Likelihood | Impact | Mitigation Strategy |
|---|---|---|---|---|
| R1 | **Bubble plan capacity limits hit** (server-side workload exceeds tier) | Medium | High | Monitor capacity dashboard; upgrade plan proactively; optimize workflows to reduce server calls |
| R2 | **Third-party AI API downtime** (OpenAI, AdCreative, etc.) | Medium | High | Implement error handling in workflows; show user-friendly fallback messages; retry logic in workflows |
| R3 | **Meta/Google API rate limits** | Medium | Medium | Cache API responses in DB; throttle scheduled workflows; implement exponential backoff pattern |
| R4 | **AI-generated content quality inconsistency** | Medium | Medium | Manual approval step (already in workflow); prompt engineering in API calls; multiple variation generation |
| R5 | **Bubble platform downtime** | Low | Critical | Monitor Bubble status page; communicate to clients; design for graceful degradation |
| R6 | **Data privacy rule misconfiguration** | Low | Critical | Thorough QA of privacy rules; test as every user role; independent security review |
| R7 | **Scope creep — adding features not in Bubble's capability** | High | Medium | Strict change control; document Bubble limitation boundaries clearly; escalate to custom code only if absolutely necessary |
| R8 | **Slow page loads with large data sets** | Medium | Medium | Implement pagination; use server-side search constraints; pre-compute aggregate counts |
| R9 | **Bubble vendor lock-in** | Low | Medium | Document all data types and API integrations thoroughly; maintain data export capability for future migration |

---

## 16. Milestones & Deliverables Summary

| Milestone | Target Date | Deliverables | Success Criteria |
|---|---|---|---|
| **M0 — Foundation Ready** | End of Week 1 | Auth, RBAC, Admin portal, all API connectors tested, Stripe configured, DB schema live, design system | Users can register/login, admin can manage users, all APIs initialized |
| **M1 — Onboarding + AI Modules 1–2** | End of Week 2 | Onboarding wizard, AI Business Analyzer, AI Brand Builder | New user can onboard and receive AI-generated brand kit |
| **M2 — Campaign MVP (Modules 3–5)** | End of Week 3 | Content Studio, Campaign Builder, Social Media Manager | Agency can generate creatives, build campaign, and publish to Meta/Google |
| **M3 — Payments + Leads (Module 6)** | End of Week 4 | Stripe billing (subscriptions + per-campaign), Lead Capture, Landing Pages, WhatsApp/SMS | Payments live; leads captured from landing pages and WhatsApp |
| **M4 — Sales + Engagement (Modules 7–8)** | End of Week 5 | AI Sales Agent chatbot, Customer Engagement Engine | AI qualifies leads via chat; email/SMS/WhatsApp campaigns automated |
| **M5 — Analytics + Optimization (Modules 9–11)** | End of Week 6 | Analytics Engine, Optimization Engine, Growth Planner | Self-optimizing campaigns with AI-generated growth reports |
| **M6 — Dashboard + Security (Module 12)** | End of Week 7 | Unified Dashboard, GDPR compliance, security audit, full QA regression | All 12 modules complete; privacy rules verified; no data leaks |
| **M7 — Production Launch 🚀** | End of Week 8 | UAT approved, production domain live, Stripe live, first clients onboarded | Platform publicly accessible with all 12 modules functional |

---

## 17. Cost Estimate (Bubble Plans)

### 17.1 Bubble Platform Cost

| Phase | Bubble Plan | Monthly Cost | Notes |
|---|---|---|---|
| Development (Weeks 1–7) | Team | $349/mo | Required for team collaboration + backend workflow capacity |
| Production Launch (Week 8+) | Team or Production | $349–$849/mo | Upgrade to Production based on user growth |

### 17.2 Third-Party API Costs (Estimated Monthly at Scale)

| Service | Model | Estimated Cost |
|---|---|---|
| OpenAI GPT-4 | Per token (~$0.03/1K tokens) | $200–$1,000/month based on usage |
| AdCreative.ai | Per creative generation | $300–$800/month based on campaigns |
| Pictory | Per video | $99–$299/month (plan-based) |
| ElevenLabs | Per character | $22–$99/month (plan-based) |
| Meta/Google Ads API | Free API; ad spend billed separately | API: Free |
| Stripe | 2.9% + $0.30 per transaction | Scales with revenue |
| Twilio (WhatsApp/SMS) | Per message (~$0.005–$0.05) | $50–$300/month |
| SendGrid | Per email (Free up to 100/day) | $0–$89/month |

> **Total estimated running cost (at launch):** ~$1,500–$3,500/month platform + API costs, before ad spend.

---

## 18. Bubble.io Limitations & When to Migrate to Custom Code

### 18.1 When Bubble.io Will Be Sufficient

- Up to ~**5,000 active users** (with proper plan)
- Standard workflow logic (approvals, triggers, notifications)
- API orchestration via API Connector
- Moderate database operations (well-optimized searches)

### 18.2 Signals That Migration to Custom Code May Be Needed

| Signal | Threshold | Action |
|---|---|---|
| Concurrent users causing capacity issues | > 500 concurrent users consistently | Upgrade plan or evaluate migration |
| DB performance degrading | > 500K records in key data types | Optimize or migrate to PostgreSQL |
| Complex AI logic needed (custom Python models) | When off-the-shelf APIs are insufficient | Add a FastAPI microservice alongside Bubble |
| Real-time features needed (WebSockets) | Live leaderboards, instant chat at scale | Add a Node.js/FastAPI service |
| Regulatory requirement for on-premise data | HIPAA, data residency laws | Migrate to custom code on your own infra |

### 18.3 Hybrid Architecture Option (Best of Both)

If/when Bubble hits limits, consider a **hybrid approach**:

```
Bubble.io  (Frontend + basic workflows)
     │
     ▼ calls
FastAPI Microservice  (Heavy AI processing, custom Python logic)
     │
     ▼ reads/writes
PostgreSQL  (Migrated from Bubble DB)
```

This lets you keep Bubble for rapid UI development while offloading compute-heavy AI logic to Python/FastAPI — giving you the speed of Bubble with the power of custom code.

---

## 19. Success Metrics & KPIs

### 19.1 Platform Performance KPIs

| KPI | Target | How to Measure in Bubble |
|---|---|---|
| Campaign creation time | < 10 minutes | Timestamp fields on campaign creation vs. approval |
| AI creative generation time | < 60 seconds | Job creation timestamp vs. job completion timestamp |
| SME onboarding time | < 5 minutes | Onboarding start vs. complete timestamps |
| Platform uptime | > 99.9% | Bubble status page + external uptime monitor (UptimeRobot) |
| Page load time | < 3 seconds | Chrome DevTools + PageSpeed Insights |

### 19.2 Business KPIs (6 Months Post-Launch)

| KPI | Target | Measurement |
|---|---|---|
| Active paying clients | 100+ | Stripe dashboard |
| Campaigns launched | 500+ | Campaign data type count |
| Client retention rate | > 85% | Subscription cancellation rate |
| Average campaign ROI for clients | > 3x | Analytics engine calculations |
| NPS Score | > 50 | Quarterly in-app survey (Typeform plugin) |

---

## 20. Assumptions & Dependencies

### 20.1 Assumptions

1. Client will use Bubble.io's official hosted cloud (not self-hosted)
2. All AI capabilities will be delivered via third-party APIs (no custom model training)
3. The platform is web-first; native mobile apps are not in scope
4. Bubble.io's Growth or Team plan will be sufficient for launch capacity
5. Client provides all API credentials and sandbox accounts for third-party services
6. Design mockups will be provided in Figma and implemented in Bubble by the dev team
7. GDPR is the primary compliance framework in scope

### 20.2 External Dependencies

| Dependency | Owner | Impact if Delayed |
|---|---|---|
| Meta Marketing API App Review | Meta / Client | Campaign publishing blocked |
| Google Ads API Developer Token | Google / Client | Google campaign publishing blocked |
| AdCreative.ai API Access | AdCreative.ai | Graphics generation blocked |
| Pictory API Access | Pictory | Video generation deferred |
| ElevenLabs API Access | ElevenLabs | Voiceover generation deferred |
| Stripe Account Setup | Client | Payment processing blocked |
| WhatsApp Business API Approval | Meta / Client | WhatsApp features deferred |
| Custom Domain & DNS Access | Client | Custom domain setup blocked |
| OpenAI API Key & Billing | Client | All AI modules blocked |

---

## Appendix: Communication & Governance

### Meeting Cadence

| Meeting | Frequency | Participants | Purpose |
|---|---|---|---|
| Sprint Planning | Bi-weekly (Monday) | Full team | Plan Bubble development sprint |
| Daily Standup | Daily (15 min) | Dev team | Progress & blockers |
| Sprint Demo | Bi-weekly (Friday) | Team + Client | Live Bubble demo of completed features |
| Steering Committee | Monthly | PM + Client | Milestone review, strategic decisions |

### Reporting

- **Weekly Status Report:** Progress, blockers, Bubble capacity metrics, API usage
- **Milestone Sign-Off:** Client approves each milestone with screen recording of working features

---

## Appendix: Glossary

| Term | Definition |
|---|---|
| **Bubble.io** | No-code full-stack platform (frontend + backend + database) |
| **API Connector** | Bubble plugin that connects to any external REST API |
| **Backend Workflow** | Server-side Bubble workflow (not triggered by user UI action) |
| **Scheduled Workflow** | Bubble workflow that runs automatically on a time interval |
| **Privacy Rules** | Bubble's row-level security for each data type |
| **Reusable Element** | Bubble's equivalent of a UI component (header, modal, card) |
| **Live Branch** | The deployed production version of a Bubble app |
| **Development Branch** | The editable staging version of a Bubble app |
| **Capacity** | Bubble's compute resource allocation per plan tier |
| AIMOS | AI Marketing Operating System |
| SME | Small and Medium Enterprise |
| BRD | Business Requirements Document |
| RBAC | Role-Based Access Control |
| GDPR | General Data Protection Regulation |

---

*Document prepared by the Engineering Team — March 2026*
*This document is confidential and intended for stakeholder review only.*
