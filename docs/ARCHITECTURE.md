# Scout — Architecture

## System topology

```text
React / TypeScript Frontend
  ├─ Dashboard
  ├─ Live Scout
  ├─ Campaigns
  ├─ Companies
  ├─ Analytics
  ├─ Web Opportunities
  ├─ Outreach
  ├─ Deals
  ├─ Scheduler
  ├─ Export Center
  ├─ AI Providers
  └─ Settings / i18n
        │
        │ REST + Server-Sent Events
        ▼
FastAPI Backend
  ├─ Authentication
  ├─ Discovery & enrichment
  ├─ Quality scoring
  ├─ Priority scoring
  ├─ Market intelligence
  ├─ Web health classification
  ├─ Outreach generation
  ├─ Outreach sending governance
  ├─ Deals pipeline
  └─ Scheduled jobs
        │
        ├─ Google Places / Details
        ├─ Tavily market/news context
        ├─ AI provider abstraction
        ├─ Resend email delivery
        └─ SQLAlchemy data model
```

## Core data entities

The supplied project record describes six central tables:

- `scout_campaigns` — discovery missions and coverage;
- `scout_companies` — normalized company records and scores;
- `web_opportunities` — website health findings;
- `scout_scheduled_jobs` — recurring discovery jobs;
- `outreach_queue` — message lifecycle and tracking;
- `deals_pipeline` — CRM stages, values, and next actions.

## Discovery architecture

Scout models discovery as a mission rather than a single lookup. The `/scout/search` endpoint streams progress through SSE, allowing the frontend to surface batch progress and intermediate results.

The discovery layer should be conceptually separated into:

```text
market geography
   ↓
category / target definition
   ↓
source query
   ↓
company identity
   ↓
details enrichment
   ↓
normalization
   ↓
score + persistence
```

This separation is what makes a new market adapter feasible: the core pipeline does not need to know that a geographic unit is called a Turkish province, a German Land, a US state, a Canadian province, or a Gulf industrial city.

## Scoring architecture

Two independent scoring functions are applied to the normalized company record.

```text
company evidence
   ├─→ Quality Score  (record completeness / actionability)
   └─→ Priority Score (commercial ranking for a market configuration)
```

The first can remain broadly reusable. The second is intentionally market-specific.

For the Turkish reference model, organized industrial-zone membership is a meaningful priority signal. For another country, the equivalent feature may be an industrial park, trade registry classification, customs/export data, procurement category, distributor authorization, certification, or another locally validated indicator.

## Opportunity architecture

A company can produce more than one opportunity type.

```text
company
  ├─ industrial/export opportunity
  ├─ web/digital opportunity
  └─ future market-specific opportunity types
```

This is why the normalized company entity remains upstream of commercial arms.

## Market adapter boundary

A portable deployment should isolate these components as configuration/adapters:

- country and geographic hierarchy;
- discovery-source connectors;
- company categories and local vocabulary;
- industrial-cluster rules;
- score features and weights;
- exporter/importer orientation;
- market-news queries;
- seasonal/trade-cycle cards;
- supported languages;
- currency and locale;
- outreach policies and legal/compliance requirements.

The rest of the architecture — discovery orchestration, enrichment state, two-axis scoring concept, opportunity routing, outreach queue, and CRM — remains stable.

## Importer / exporter modes

The same system can invert the commercial lens.

### Export intelligence

```text
local supplier/manufacturer
  → export-readiness evidence
  → target-market opportunity
  → distributor/buyer outreach
  → deal pipeline
```

### Import / sourcing intelligence

```text
target suppliers/manufacturers
  → supplier completeness + fit ranking
  → enrichment
  → procurement outreach
  → supplier/deal pipeline
```

These are architectural adaptations, not claims that the current Turkish Priority formula is directly reusable.

## External dependencies

The project source identifies integrations including Google Places, Tavily, Resend, and six AI-provider paths. These integrations are replaceable components around Scout's internal decision flow rather than the core intellectual asset themselves.

## Deployment boundary

The public evidence repository intentionally omits production source and secrets. The supplied project record describes Dockerized services behind reverse proxy / managed SSL infrastructure.
