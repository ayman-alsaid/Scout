# Scout — B2B Intelligence from Market Discovery to Deals

![B2B Intelligence](https://img.shields.io/badge/B2B%20Intelligence-5B6F8A?style=flat-square)
![Dual--Axis Scoring](https://img.shields.io/badge/Dual--Axis%20Scoring-6B6F86?style=flat-square)
![Multilingual Outreach](https://img.shields.io/badge/Multilingual%20Outreach-5F7A72?style=flat-square)
![Market--Portable Architecture](https://img.shields.io/badge/Market--Portable%20Architecture-7A6F86?style=flat-square)

> **What if a company-discovery system did more than produce a lead list — what if it continuously turned market data into ranked commercial opportunities, governed outreach, and a trackable deal pipeline?**

Scout is a B2B intelligence platform built around a complete operational loop:

**Discover → Enrich → Score → Detect Opportunity → Reach → Track → Close**

It combines systematic company discovery, deterministic scoring, web-opportunity detection, live market intelligence, multilingual AI-assisted outreach, scheduling, and CRM stages in one platform.

The most important architectural distinction is this:

> **Turkey is the reference market used to model and exercise the industrial-discovery workflow. It is not the architectural boundary of Scout.**

The Turkish industrial configuration covers all **81 provinces** and uses Turkey-specific signals such as organized industrial zones (OSB) and export-oriented market context. Those are **market configuration**, not the core engine. The reusable engine is the discovery/scoring/outreach/deal architecture around them.

A deployment for another country can replace the geographic hierarchy, data sources, industrial-zone equivalents, scoring features, seasonal intelligence, language, currency, and outreach rules while preserving the core workflow. The same architecture can be configured from the **exporter side** (find producers/suppliers and foreign demand) or the **importer side** (find qualified suppliers, distributors, manufacturers, or market-entry targets).

This does **not** mean a new market is a zero-work toggle. Each market requires source validation, scoring calibration, local business taxonomy, outreach policy, and domain review. What transfers is the engineering architecture — not every Turkish constant.

---

## The Business Problem

Traditional lead-generation workflows fragment commercial intelligence across separate tools:

- one system discovers companies;
- another enriches contacts;
- spreadsheets rank prospects;
- a separate tool checks websites;
- an AI writer produces outreach;
- email software sends it;
- CRM records what happened afterward.

Scout treats these as one connected decision system.

A company is not just a row in a database. It is an entity whose commercial relevance changes as evidence accumulates:

```text
public-market signal
      ↓
company discovered
      ↓
data enriched
      ↓
quality measured
      ↓
business priority scored
      ↓
opportunity type identified
      ↓
message personalized
      ↓
outreach governed
      ↓
relationship tracked
      ↓
deal outcome captured
```

That final outcome matters because Scout's current Priority Score is deliberately documented as **domain judgment**, not as a machine-learned predictor already validated against large closed-deal volume. The CRM creates the feedback infrastructure required to calibrate those weights later.

---

## One Intelligence Core, Multiple Commercial Uses

The original reference deployment uses one shared discovery layer to support two commercial arms:

```text
                    SHARED DISCOVERY
              Google Places + enrichment
                         │
                         ▼
                  COMPANY RECORD
                         │
                ┌────────┴────────┐
                ▼                 ▼
        INDUSTRIAL INTELLIGENCE   WEB OPPORTUNITIES
        export-oriented leads     digital-presence leads
        Turkey reference model    multi-country scanning
```

The industrial arm uses the Turkish market as a concrete operational model: province-by-province discovery, factory/business signals, organized industrial-zone context, market news, and export-oriented outreach.

The Web Opportunities arm already demonstrates that the same underlying company intelligence does not need to stay inside one geography: the supplied project record defines scanning across **51 countries in 5 regions** for companies with missing or broken websites.

The deeper principle is:

> **One discovered company can contain multiple commercial signals. Do not force each signal into a separate scraping universe.**

A manufacturer may simultaneously be:

- a strong export prospect;
- under-documented and worth manual enrichment;
- located in a high-value industrial cluster;
- operating with a broken website;
- reachable in a language different from the operator's;
- appropriate for a future deal pipeline.

Duplicating discovery for each commercial use wastes API calls and destroys cross-signal context.

---

## The Intelligence Layer: Two Scores, Two Different Questions

Scout deliberately refuses to compress two independent questions into one number.

### Quality Score — `0–10`

**Question:** *How complete and actionable is the data we currently have about this company?*

Reference weighting:

```text
+3.0 verified phone
+3.0 email
+2.0 website
+1.0 reviews > 5
+1.0 address / description
```

### Priority Score — `0–100`

**Question:** *Given the evidence and the market model, how commercially valuable is this company to pursue?*

Reference Turkish industrial configuration:

```text
Quality Score × 2
Google rating × 4 (capped at 20)
+15 website
+7  email
+10 factory-type business
+5  reviews > 10
+10 organized-industrial-zone match
```

Companies scoring **80+** are flagged as high-priority in the supplied implementation.

### Why not one score?

Because these cases are not equivalent:

| Data quality | Business priority | Interpretation |
|---|---|---|
| High | High | strong, reachable opportunity |
| Low | High | potentially valuable but under-enriched — investigate |
| High | Low | well-documented but poor fit — deprioritize |
| Low | Low | weak evidence and weak opportunity |

A blended score can hide the most interesting case: **high-value + low-data**, where the data gap may itself represent an information advantage.

The separation also makes market adaptation cleaner. A different country can retain the Quality Score concept while replacing Priority Score features such as `OSB` with locally meaningful signals: industrial parks, customs/export registrations, sector certifications, distributor status, port proximity, trade-fair participation, import licenses, procurement relevance, or other validated market evidence.

---

## Turkey as a Reference Market — Not a Product Limitation

Scout's industrial workflow was modeled against Turkey because it provides a concrete, heterogeneous commercial landscape with:

- **81 geographic provinces**;
- industrial clusters and organized industrial zones;
- manufacturers serving domestic and export markets;
- multilingual trade relationships;
- strong relevance for both supplier discovery and export-market outreach.

The platform's architecture, however, can be viewed as two layers:

### Reusable core

```text
Discovery
  → Entity normalization
  → Enrichment
  → Quality scoring
  → Priority scoring
  → Opportunity classification
  → Market intelligence
  → Personalized outreach
  → Sending governance
  → CRM / deal tracking
```

### Market adapter

```text
Geography model
Source connectors
Business taxonomy
Sector vocabulary
Market-specific score signals
Exporter/importer orientation
Language / currency / locale
Seasonal events and trade cycles
Outreach policy and compliance
```

For example:

**Exporter-oriented deployment**

`domestic manufacturers → rank export readiness → identify destination-market signals → multilingual outreach → distributor/importer pipeline`

**Importer-oriented deployment**

`foreign/domestic supplier discovery → rank supplier relevance + data reliability → enrich → outreach → procurement/deal pipeline`

**Market-entry deployment**

`target-country company discovery → score distributors/partners/customers → detect whitespace → outreach → business-development pipeline`

The critical engineering claim is therefore not "the Turkish scoring formula works everywhere." It does not. The claim is that **the system separates reusable market-intelligence infrastructure from market-specific judgment sufficiently that the latter can be replaced without rebuilding the whole product.**

---

## Streaming Discovery Instead of a Silent Batch Job

A market scan may run for minutes and touch many companies. Scout exposes discovery through Server-Sent Events rather than forcing the operator to wait for one final response.

```text
POST /scout/search
        │
        ├── mission started
        ├── batch progress
        ├── company discovered
        ├── enrichment update
        ├── score update
        ├── running statistics
        └── completion / degraded state
```

This decision exists for operational trust, not visual novelty. A multi-minute spinner cannot tell an operator whether the system is progressing, rate-limited, partially failing, or dead.

SSE introduces its own engineering costs: persistent connections, disconnect handling, browser backgrounding, partial failures, and state recovery. Those trade-offs are documented rather than treated as free complexity.

---

## Web Opportunities: Turning Technical Failure into Commercial Signal

Scout's second commercial arm treats weak web presence as structured business intelligence.

The supplied implementation classifies website conditions including:

| Classification | Meaning | Potential action |
|---|---|---|
| `no_website` | no site present | new build |
| `domain_expired` | DNS resolution failure | domain + rebuild |
| `ssl_expired` | certificate failure | trust / SSL remediation |
| `server_error` | server/timeout problem | hosting or technical audit |
| `redirect_loop` | broken redirect behavior | technical remediation |
| `old_wp` | outdated WordPress signal | modernization |

Batch checking is asynchronous with up to **10 concurrent checks** in the supplied design.

This is a useful example of the system's broader philosophy: **a technical observation becomes a commercial signal only after it is classified into an actionable reason.**

---

## Market Intelligence Is Connected to the Workflow

Scout combines database state with current-market context rather than treating "market intelligence" as a static report.

The supplied system includes:

- Tavily-powered market/news retrieval;
- category-aware news context;
- seasonal opportunity cards;
- trade-fair and commercial-cycle context;
- white-spot mapping against geographic coverage;
- database insight cards such as high-priority / needs-enrichment / low-priority.

For the Turkish reference market, examples include export cycles and regional industrial coverage. In another market, these inputs are adapter-level configuration: local trade fairs, procurement seasons, import cycles, tender windows, harvest/production cycles, or industry-specific renewal periods.

---

## Multilingual, Provider-Agnostic Outreach

Scout separates outreach orchestration from one AI vendor.

The supplied project describes support for six model/provider paths including Claude, OpenAI, OpenRouter, DeepSeek, Gemini, and GLM, with provider keys managed and tested from the interface.

AI is used to generate personalized messages from actual company context such as:

- company identity;
- category;
- known web status;
- detected language;
- commercial angle.

The platform itself supports **8 interface languages**, including Arabic with RTL. Outreach generation is described for Arabic, English, Turkish, German, French, and Spanish.

Provider abstraction matters commercially because model quality, pricing, availability, and multilingual behavior change faster than a B2B workflow should have to be rewritten.

---

## Outreach Governance: Faster Is Not Always Better

Scout does not treat maximum sending speed as a success metric.

The supplied outreach workflow includes:

```text
generate
   ↓
queue
   ↓
daily limit
   ↓
09:00–17:00 sending window
   ↓
inter-message gap
   ↓
send
   ↓
pending → sent → opened → replied
   ↓
follow-up after 7 days of silence
```

This is a deliberate boundary around automation. A system capable of generating messages faster than a human can review them also needs a mechanism preventing that speed from damaging sender reputation and deliverability.

The repository does **not** claim a sustained deliverability benchmark or conversion-rate improvement; the source material explicitly states that sufficient production volume does not yet exist for those claims.

---

## Deals CRM: Intelligence Must Reach an Outcome

The workflow does not end at "email sent."

Scout tracks opportunities through a deal pipeline:

**New Lead → Contacted → Interested → Proposal → Closed**

Deal records can include value, currency, contact details, next action, and stage. This closes the data loop needed for future score calibration.

The strategic reason is important:

> A scoring system should eventually be evaluated against outcomes, not only against how plausible its weights looked when designed.

The current Priority Score is a reasoned deterministic model. It is **not yet claimed as empirically outcome-tuned**. Closed-deal history is the path from the first state to the second.

---

## Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│ React 18 · TypeScript · Vite · Tailwind · Radix · Recharts │
│ 13 modules · 8 languages · RTL · SSE live discovery        │
└───────────────────────────┬─────────────────────────────────┘
                            │ REST + SSE
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ FastAPI · SQLAlchemy · Pydantic · JWT                      │
│                                                             │
│ Discovery ─ Enrichment ─ Dual Scoring ─ Analytics           │
│ Scheduler ─ Web Scanner ─ Outreach ─ Deals CRM              │
└────────┬──────────────┬──────────────┬──────────────┬────────┘
         │              │              │              │
         ▼              ▼              ▼              ▼
 Google Places     AI Providers       Tavily        Resend
 + details         / OpenRouter       market news   email
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│ SQLAlchemy data model                                      │
│ campaigns · companies · web opportunities · jobs           │
│ outreach queue · deals pipeline                            │
└─────────────────────────────────────────────────────────────┘
```

The supplied project record identifies roughly **30 API endpoints** covering authentication, discovery, company records, analytics, web opportunities, outreach, deals, scheduler, and AI provider management.

---

## Key Engineering Decisions

### 1. One discovery engine, multiple commercial arms

**Chosen:** reuse discovery/enrichment around one company entity.

**Rejected:** separate scraping pipelines for export intelligence and web opportunities.

**Why:** eliminates duplicate data acquisition and preserves overlap between commercial signals.

**Trade-off:** shared schema becomes more complex as arm-specific fields grow.

### 2. Quality and Priority remain orthogonal

**Chosen:** two scores answering different questions.

**Rejected:** one universal lead score.

**Why:** preserves actionability — bad fit and missing data are not the same problem.

**Trade-off:** two scores are harder to communicate and require disciplined UI explanation.

### 3. SSE for long discovery missions

**Chosen:** stream progress and intermediate state.

**Rejected:** silent request/response batch job.

**Why:** improves operational observability and user trust.

**Trade-off:** connection lifecycle and recovery complexity.

### 4. Provider-agnostic AI

**Chosen:** interchangeable model providers behind one outreach layer.

**Rejected:** hard dependency on one vendor.

**Why:** resilience to pricing, quality, availability, and language shifts.

**Trade-off:** prompts must remain portable across different model behavior.

### 5. Govern outreach instead of maximizing throughput

**Chosen:** send windows, daily limits, delays, follow-ups.

**Rejected:** immediate bulk sending.

**Why:** the commercial objective is inbox delivery and relationship quality, not raw send count.

**Trade-off:** lower nominal throughput.

### 6. Market-specific signals stay outside the reusable thesis

**Chosen:** treat Turkey/OSB/export context as a reference market configuration.

**Rejected:** represent Turkish industrial assumptions as universally valid B2B intelligence.

**Why:** a German industrial cluster, Gulf importer network, Canadian distributor market, or African sourcing market requires different evidence and weights even when the workflow architecture is identical.

---

## Engineering Evidence Map

| Claim | Evidence status |
|---|---|
| Shared discovery/intelligence architecture | **IMPLEMENTED** in supplied project record |
| Quality Score + Priority Score algorithms | **IMPLEMENTED** |
| 81-province Turkish industrial reference configuration | **IMPLEMENTED / DOCUMENTED** |
| 51-country web-opportunity configuration | **IMPLEMENTED / DOCUMENTED** |
| SSE discovery endpoint | **IMPLEMENTED** |
| ~30 API endpoints | **IMPLEMENTED** route surface in supplied record |
| 8-language interface / Arabic RTL | **IMPLEMENTED / VERIFIED BY DEVELOPMENT USE** per source |
| multi-provider AI configuration | **IMPLEMENTED** |
| rate-governed outreach workflow | **IMPLEMENTED** |
| CRM outcome capture | **IMPLEMENTED** |
| Priority weights predict closed deals better | **NOT YET VALIDATED** |
| sustained outreach conversion rate | **NOT YET VALIDATED** |
| sustained email deliverability benchmark | **NOT YET VALIDATED** |
| Turkish Priority formula transfers unchanged to other countries | **NOT CLAIMED** |
| core architecture can be adapted to other markets | **ARCHITECTURAL GENERALIZATION**; requires market-specific validation |

---

## Why This Matters to Companies

Scout is relevant beyond teams looking for "more leads." Its architecture addresses a broader revenue-operations question:

> **How do we convert fragmented public-market evidence into a disciplined sequence of commercial decisions?**

Potential organizational applications include:

- export departments discovering distributors, buyers, or commercial partners;
- importers and procurement teams discovering and ranking suppliers;
- manufacturers identifying under-covered regional markets;
- international business-development teams building country-entry pipelines;
- trade and economic-development organizations mapping businesses systematically;
- agencies serving multiple offers against overlapping company datasets;
- corporate development teams building acquisition-target discovery workflows;
- sales operations teams requiring explainable prioritization before outreach.

The adaptation work belongs in the market adapter. The engine remains:

**Discover → Score → Personalize → Reach → Track**

---

## What Scout Is Not

Scout is not presented here as:

- a universal lead-scoring formula;
- proof that an `80+` score predicts a closed deal in every market;
- a substitute for local market expertise;
- a promise that one data provider covers every country equally;
- a guarantee of inbox placement or commercial conversion;
- a claim that exporter and importer workflows use identical score features.

Its strongest engineering claim is narrower and more useful: **the platform turns entity discovery, independent scoring, opportunity detection, governed multilingual outreach, and outcome tracking into one configurable commercial-intelligence architecture.**

---

## Documentation

- [Technical Case Study](docs/CASE_STUDY.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Engineering Decisions](docs/ENGINEERING_DECISIONS.md)
- [Market Adaptation Model](docs/MARKET_ADAPTATION.md)
- [Testing & Verification](docs/TESTING_AND_VERIFICATION.md)
- [Security & Operational Controls](docs/SECURITY_AND_OPERATIONS.md)
- [Limitations](docs/LIMITATIONS.md)
- [Evidence Index](evidence/README.md)

---

## Public Portfolio Boundary

This repository is an **Engineering Evidence / Technical Case Study** for Scout. Production implementation remains private. The purpose of the public repository is to make architecture, decision logic, scope, evidence boundaries, and limitations reviewable without exposing proprietary production code or credentials.

**Live:** https://scout.agentcraft.info

Built by **Ayman Alsaid** under **AgentCraft**.
