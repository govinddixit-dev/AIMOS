# AIMOS – Bubble.io AI Builder Prompt
### Version 1.0 | March 2026

---

## Master Prompt: Build AIMOS on Bubble.io

---

You are an expert Bubble.io developer. I want you to help me build **AIMOS — the AI Marketing Operating System** — a full-stack no-code SaaS platform built entirely on Bubble.io. This platform automates digital marketing for small and medium enterprises (SMEs) using AI. Below is the complete specification. Follow it precisely.

---

### PLATFORM OVERVIEW

AIMOS is a multi-tenant B2B SaaS platform where marketing agencies and SMEs use AI to create, manage, publish, and optimize digital advertising campaigns — without needing a marketing team. The platform replaces the role of a full marketing department by automating content creation, campaign publishing, lead generation, sales conversations, customer engagement, and performance analytics through artificial intelligence.

The platform has three user roles:

1. **Platform Admin** — manages the entire AIMOS platform, controls users, billing oversight, compliance, and platform-wide analytics
2. **Agency / SME Client** — the primary paying user who logs in to create campaigns, generate AI content, capture leads, and view performance reports
3. **End Customer** — the prospect or customer of the SME who interacts with AI-generated landing pages and the AI Sales Agent chatbot

The platform is web-first and must be fully responsive for mobile, tablet, and desktop. All frontend, backend logic, database, and API orchestration are built inside Bubble.io — no custom server code.

---

### CORE REQUIREMENTS

**Authentication & Access Control:**
Build a complete user authentication system using Bubble's native auth. Include email/password registration, email verification, Google OAuth login, and multi-factor authentication (via a plugin). Define three roles: Admin, Agency, Customer. Use Bubble's privacy rules to strictly isolate data between tenants — Agency users must only see their own campaigns, leads, payments, and creatives. End Customers must only see their own records. Admins see everything. Store the user's role in a `role` field on the User data type. Enforce role-based page access using "This page is accessible to" conditions.

**Onboarding Wizard:**
After first login, Agency users must complete a 3-step onboarding wizard: Step 1 collects business profile (company name, industry, website, target audience, monthly ad budget, primary goals). Step 2 links social accounts (Meta Business Manager ID, Google Ads customer ID — these are text fields, not OAuth at this stage). Step 3 sets notification preferences. Store onboarding completion as a boolean on the Agency data type. Redirect to the main dashboard only after onboarding is complete. Allow users to save progress and resume later.

---

### ALL 12 AI MODULES

**Module 1 — AI Business Analyzer:**
Build a form page where Agency users input business details (industry, location, competitors, audience demographics, current challenges). On submit, call GPT-4 via API Connector with a prompt instructing it to return structured JSON containing: recommended advertising platforms, suggested budget allocation per platform, top 3 competitor weaknesses, and 3 strategic campaign recommendations. Display results on a results page with expandable sections. Store output in a `BusinessAnalysis` data type linked to the Agency.

**Module 2 — AI Brand Builder:**
Build a brand kit creation page. Accept: brand name, industry, tone of voice (dropdown), and optional color preference. On submit, call DALL-E 3 to generate a logo and GPT-4 to generate a brand voice guide (tagline, mission statement, tone description, sample ad copy). Store all outputs in a `BrandKit` data type. Display with download buttons for the logo and a PDF export of the brand guide.

**Module 3 — AI Content Studio:**
Build a creative generation pipeline. Accept a campaign brief form (product/service name, target audience, key message, call-to-action, ad format — image / video / voiceover). On submit, create a `CreativeJob` record with status set to "pending". Trigger a Backend Workflow that calls AdCreative.ai API to generate 5 image ad variations, calls Pictory API to generate a 30-second video ad, and calls ElevenLabs API to generate a voiceover. As each job completes, update the `CreativeJob` record with the returned asset URLs and update the status to "complete". The frontend must poll the job record every 5 seconds using a "Do every 5 seconds" workflow and stop polling when status equals "complete". Display all generated variations as cards — images with preview, video with a Bubble video element, audio with a play button. Allow the user to select one variation as approved.

**Module 4 — AI Campaign Builder:**
Build a 5-step campaign creation wizard: (1) objective, (2) target audience (age, gender, location, interests), (3) budget and duration, (4) select approved creative from Module 3, (5) review and confirm. On confirm, call Meta Marketing API to create the campaign/adset/ad and Google Ads API for a parallel campaign. Store both platform campaign IDs in the `Campaign` data type.

**Module 5 — AI Social Media Manager:**
Build a social media calendar page using a calendar plugin. Build a post composer form where users write or AI-generate post content (call GPT-4 for caption and hashtag suggestions), attach media, select platforms, and schedule a date/time. Store posts in `ScheduledPost` with status "scheduled". Create a Scheduled Workflow (every 15 minutes) that publishes due posts via Meta Graph API and updates each status to "posted".

**Module 6 — AI Lead Capture System:**
Build a landing page builder where Agency users define a headline, subheadline, hero image URL, bullet points, and a customizable lead capture form. Auto-generate a unique slug served at `/lp/[slug]`. On form submission, create a `Lead` record, run a scoring workflow (0–100 based on data completeness and source), and send the Agency user an email via SendGrid. Build a CRM-style lead list page with status filters and a Kanban pipeline view (New, Contacted, Qualified, Converted).

**Module 7 — AI Sales Agent:**
Build an AI chatbot chat page at `/chat/[session_id]`. The chat UI must show a conversation thread (repeating group of `ChatMessage` records), a typing indicator (shown during API call), and a text input with a send button. When a user sends a message, store it as a `ChatMessage` with role "user". Build a conversation history string by concatenating the last 10 messages. Call OpenAI GPT-4 API with a system prompt from the `AIAgentConfig` data type (which the Agency user configures — business name, product info, tone, qualification questions, booking link). Store the GPT-4 reply as a `ChatMessage` with role "assistant". After each reply, run a qualification check workflow — if the lead mentions buying intent keywords, update lead status to "qualified" and optionally call Calendly API to generate a booking link and send it to the user in the next message.

**Module 8 — AI Customer Engagement Engine:**
Build an engagement campaign builder where users pick a channel (email/SMS/WhatsApp), write a template with placeholders ({{first_name}}, {{product}}), filter an audience by lead status/score, and schedule a send time. Trigger a Backend Workflow that fetches matching leads and calls SendGrid (email), Twilio (SMS), or Twilio WhatsApp API per lead. Personalize each message using lead data and log delivery status in `EngagementMessage` records.

**Module 9 — AI Analytics Engine:**
Build an analytics dashboard with a date range picker, platform filter, KPI summary cards (impressions, clicks, CTR, spend, conversions, CPL, ROI), and performance trend charts (ApexCharts plugin or native Bubble charts). Create a Scheduled Workflow (hourly) that pulls latest metrics from Meta and Google Ads APIs and stores them in `CampaignMetric` records. Add a CSV export button for all campaign metrics.

**Module 10 — AI Optimization Engine:**
Build a rules configuration page where users create rules: select metric (CTR/CPL/ROI), operator (less/greater than), threshold value, and action (pause campaign / increase budget 10% / send alert). Store in `OptimizationRule`. Create a Scheduled Workflow (every 4 hours) that evaluates all active rules against live campaign metrics, executes matching actions via Meta/Google API, logs each action in `OptimizationLog`, and emails the Agency user.

**Module 11 — AI Growth Planner:**
Trigger a Backend Workflow when a campaign ends that collects all `CampaignMetric` records for that campaign, calculates totals and averages, identifies the best-performing platform and lowest CPL period, then calls GPT-4 to generate a strategic growth report (what worked, what underperformed, 3 recommendations, next 90-day budget suggestion). Store in `GrowthReport` and display on a report page with a "Plan Next Campaign" CTA that pre-fills the campaign wizard with AI recommendations.

**Module 12 — AI Business Dashboard:**
Build the main unified dashboard as the first page after login. Show: active campaigns with status badges, pending creative approvals with quick-approve buttons, total leads this month with a sparkline, total ad spend vs. estimated ROI, a notification feed, and quick action buttons (Create Campaign, View Leads, Generate Creatives). All data must be role-conditional. Use pre-computed aggregate fields on the Agency data type — updated by workflows on every relevant create/update action — rather than live search queries on page load.

---

### DATABASE DESIGN

Create the following Bubble data types. All Agency-owned types must include an `agency_id` (Agency) field for multi-tenant isolation.

| Data Type | Key Fields |
|---|---|
| **User** | email, name, role (text), agency_id, subscription_status, mfa_enabled (yes/no) |
| **Agency** | name, industry, website, logo_url, onboarding_complete (yes/no), subscription_plan, total_leads (number), active_campaigns_count (number) |
| **BusinessAnalysis** | agency_id, industry, audience_data, strategy_output, platform_recommendations, created_date |
| **BrandKit** | agency_id, logo_url, color_palette, brand_voice, tagline |
| **Campaign** | agency_id, name, objective, budget (number), status, platform, meta_campaign_id, google_campaign_id, creative_id (CreativeJob), approval_status, start_date, end_date, is_active (yes/no) |
| **CreativeJob** | campaign_id, type, status, variation_urls (list of texts), approved_url, created_date |
| **Lead** | agency_id, campaign_id, name, email, phone, source, score (number), status, created_date |
| **LandingPage** | agency_id, slug (unique), headline, body_text, form_config, campaign_id, active (yes/no) |
| **ChatSession** | lead_id, agency_id, status, agent_config_id (AIAgentConfig) |
| **ChatMessage** | session_id (ChatSession), role (user/assistant), content, created_date |
| **AIAgentConfig** | agency_id, system_prompt, tone, booking_enabled (yes/no), calendly_url |
| **ScheduledPost** | agency_id, content, media_url, platform, scheduled_at, status, hashtags |
| **EngagementCampaign** | agency_id, type (email/sms/whatsapp), template, scheduled_at, status, audience_filter |
| **CampaignMetric** | campaign_id, date, impressions, clicks, ctr, cpl, conversions, spend, roi (all numbers) |
| **OptimizationRule** | agency_id, metric, operator, threshold (number), action, active (yes/no) |
| **OptimizationLog** | campaign_id, rule_id, action_taken, old_value, new_value, timestamp |
| **GrowthReport** | campaign_id, agency_id, best_channel, recommendations, next_steps, created_date |
| **Payment** | agency_id, amount (number), type, stripe_charge_id, status, created_date |
| **Notification** | user_id (User), type, message, is_read (yes/no), created_date |

Privacy rules: Agency users can only find/view/modify records where `agency_id = Current User's agency_id`. End Customers can only view their own Lead and ChatSession records. Admins have unrestricted access to all data types.

---

### API INTEGRATIONS

Configure all integrations in Bubble's **API Connector plugin**. Store every API key as a Bubble **Environment Variable** — never hardcode credentials in workflows.

| Integration | Base URL / Method | Auth | Key Calls |
|---|---|---|---|
| OpenAI | `https://api.openai.com/v1` | Bearer Token | `POST /chat/completions`, `POST /images/generations` |
| AdCreative.ai | Per docs | API Key header | `POST /creative/generate`, `GET /creative/{id}/status` |
| Pictory | Per docs | API Key header | `POST /video/create`, `GET /video/{id}/status` |
| ElevenLabs | `https://api.elevenlabs.io/v1` | API Key header | `POST /text-to-speech/{voice_id}` |
| Meta Marketing API | `https://graph.facebook.com/v18.0` | Bearer Token | Create campaign/adset/ad, fetch insights |
| Google Ads API | Per docs | OAuth 2.0 | Campaign mutate, performance stream |
| Twilio (SMS + WhatsApp) | `https://api.twilio.com/2010-04-01/...` | Basic Auth | `POST /Messages.json` |
| SendGrid | `https://api.sendgrid.com/v3` | Bearer Token | `POST /mail/send` |
| Calendly | `https://api.calendly.com` | Bearer Token | `POST /scheduling_links` |
| **Stripe** | Official Stripe Bubble plugin | Stripe keys | Subscriptions, charges, webhooks |

For Stripe, use the official plugin only — never use API Connector. Configure three subscription plans: AIMOS Starter ($49/mo), AIMOS Growth ($149/mo), AIMOS Scale ($399/mo). Store only `stripe_customer_id` and `stripe_subscription_id` on the Agency record.

---

### PAYMENT & BILLING

Use the Stripe Bubble plugin to implement:
- A subscription plans page (`/billing`) showing the three AIMOS plans with feature comparison
- An upgrade/downgrade flow using Stripe's customer portal URL
- Per-campaign payment option for pay-as-you-go users (one-time Stripe charge)
- A webhook endpoint (API Workflow, no auth required, verify Stripe signature) that handles `invoice.payment_succeeded` (activate subscription), `invoice.payment_failed` (send dunning email, lock features after 3 failures), and `customer.subscription.deleted` (downgrade to free tier)
- An invoice history page showing all Payment records with amounts, dates, and Stripe receipt links

---

### ADMIN PORTAL

Build an admin section (pages prefixed `/admin/`) accessible only to users with role = "Admin". Redirect non-admins to `/dashboard` on page load.

- **`/admin`** — Platform KPIs: total agencies, total active campaigns, total revenue this month, top 5 agencies by spend
- **`/admin/users`** — Full user list with search, role filter, activate/suspend actions
- **`/admin/analytics`** — Platform-wide charts: new signups per week, revenue trend, module usage frequency
- **`/admin/billing`** — All Payment records with date/status filters and subscription plan distribution chart

---

### SECURITY REQUIREMENTS

1. All API keys in Bubble Environment Variables — never hardcoded in workflows
2. All Backend Workflow API endpoints must require an authentication token
3. Stripe webhook endpoint must verify the Stripe webhook signature before processing
4. Every sensitive page must check user role on page load and redirect unauthorized users immediately
5. HTTPS is automatic on Bubble cloud — verify SSL certificate is active before launch
6. Sign-up form must include a GDPR consent checkbox; store consent timestamp on the User record
7. Build a "Delete My Account" cascade workflow removing all records tied to the user's agency
8. Build a "Export My Data" workflow that emails the user a compiled JSON of all their records via SendGrid

---

### DEPLOYMENT STRATEGY

Maintain two Bubble branches: **Development** (all active work and testing) and **Live** (production users — deploy only after QA sign-off). Before go-live, add a custom domain in Bubble Settings, configure DNS CNAME, verify SSL, and switch all API keys and Stripe from test to production mode.

Plan tiers: **Growth plan** ($119/mo) for beta launch → **Team plan** ($349/mo) for 20+ concurrent clients → **Production plan** ($849/mo) for full commercial launch with dedicated capacity and SLA.

---

### RESPONSIVE DESIGN RULES

All pages must use Bubble's responsive layout engine with the following breakpoints:
- Mobile: 320–767px — single-column layout, hamburger menu, stacked cards, full-width buttons
- Tablet: 768–1023px — two-column layout, simplified nav
- Desktop: 1024px+ — sidebar navigation, multi-column dashboards, full data tables

Define app-wide styles in Bubble's Design tab before building any pages. Create reusable elements for: the top navigation bar (with role-conditional menu items), the sidebar (agency dashboard navigation), campaign card component, lead card component, notification item component, and the full-screen loading overlay (shown during AI API calls).

---

Build this application systematically: start with the database schema and privacy rules, then build authentication and onboarding, then the Admin portal, then each AI module in the order listed (Modules 1 through 12), then payment processing, then analytics and optimization, and finally the unified dashboard. At every step, test workflows in debug mode before moving on. Validate privacy rules by logging in as each user role and confirming data isolation.

---

*This prompt defines the complete AIMOS platform. Follow every specification precisely to deliver a production-ready no-code SaaS application on Bubble.io.*
