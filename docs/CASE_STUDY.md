# Scout — Technical Case Study

## Problem

Most lead-generation systems optimize for a narrow sequence: find companies, send messages, track replies. Scout was designed around a broader problem: the same discovered company may carry several commercial signals at once, and the value of the system comes from preserving those signals through discovery, enrichment, scoring, outreach, and deal tracking rather than fragmenting them across separate tools.

The reference industrial workflow uses Turkey as its working market model, scanning across 81 provinces and applying Turkey-specific signals such as organized industrial zones and export-oriented market context. That configuration is intentionally separated from the reusable architecture.

## Core Flow

```text
Discover → Enrich → Quality Score → Priority Score
         → Opportunity Classification
         → Market Intelligence
         → Personalized Outreach
         → Governed Sending
         → Deals Pipeline
```

## Why one engine serves multiple commercial arms

The project source describes two commercial arms:

1. Industrial intelligence: export-marketing lead discovery across the Turkish reference market.
2. Web opportunities: identifying companies with missing or broken digital presence across a wider international configuration.

Both start from the same company discovery and enrichment layer. The engineering decision was to preserve one shared company entity rather than re-scrape the same source for every offer.

This matters because a manufacturer can simultaneously be a strong trade prospect and a strong digital-transformation prospect. Splitting those signals into different systems loses the overlap.

## Dual-axis scoring

Scout separates data completeness from commercial value.

### Quality Score

Measures how actionable the company record is: phone, email, website, review count, address/description.

### Priority Score

Measures commercial priority using a weighted deterministic formula. The supplied Turkish reference configuration includes rating, reachability, factory-type indicators, review volume, and organized-industrial-zone membership.

The distinction enables an important business state:

> high commercial value + low data completeness

That record should not be treated the same as a low-value company. It may deserve enrichment rather than rejection.

The current weights are documented as domain judgment, not as statistically validated conversion predictors.

## Streaming market discovery

Long discovery missions use Server-Sent Events so the operator can observe progress, batches, running statistics, and degraded states during multi-minute scans.

This trades implementation simplicity for operational transparency. Persistent streams require connection handling and partial-failure logic, but avoid the trust problem of a silent long-running batch request.

## Web presence as structured commercial evidence

Scout converts technical website conditions into actionable categories such as no website, expired domain, SSL failure, server error, redirect loop, and outdated WordPress indicators.

The significance is not the HTTP check itself. It is the classification layer that translates technical evidence into a sales or remediation reason.

## Market intelligence

The system combines stored company evidence with market context such as news, seasonal opportunities, trade cycles, and geographic white spots.

In Turkey, this can be oriented toward export cycles and industrial coverage. In another market, the same layer can use local trade fairs, tender cycles, import seasons, procurement windows, or sector-specific events.

## Multilingual outreach

Scout's outreach layer is provider-agnostic and can generate personalized messages from company context. The supplied project supports multiple AI providers and multilingual output.

Sending is intentionally governed by daily limits, send windows, message gaps, and follow-up timing. The design optimizes for deliverability and relationship quality rather than maximum throughput.

## Outcome capture

The Deals Pipeline tracks opportunities through commercial stages, creating the instrumentation needed to evaluate future scoring quality against actual outcomes.

This is important because a deterministic score designed from domain judgment can become evidence-tuned only after enough outcomes exist.

## Market portability

The architecture can be represented as:

```text
Reusable Core
  discovery
  entity normalization
  enrichment
  independent scoring
  opportunity classification
  outreach orchestration
  governance
  CRM

Market Adapter
  geography
  source connectors
  business taxonomy
  score features and weights
  importer/exporter orientation
  trade-cycle intelligence
  language/currency/locale
  outreach/compliance rules
```

A new-country implementation therefore does not require rebuilding the entire product, but it does require meaningful local calibration. The Turkish scoring formula itself is not claimed as universal.

## Exporter vs importer orientation

### Exporter-side

Discover manufacturers or suppliers → assess readiness/reachability → connect destination-market context → outreach to buyers/distributors → track commercial relationships.

### Importer-side

Discover suppliers/manufacturers → assess reliability and fit → enrich → contact → move candidates through procurement/business-development stages.

The entity pipeline remains stable while score inputs and commercial semantics change.

## Current evidence boundaries

Supported by the supplied project record:

- shared discovery/intelligence architecture;
- dual scoring;
- Turkish 81-province reference model;
- 51-country web opportunity configuration;
- SSE search flow;
- approximately 30 API endpoints;
- multi-provider AI management;
- multilingual interface/outreach design;
- outreach governance;
- CRM outcome capture.

Not yet supported as generalized claims:

- conversion-rate uplift;
- statistically outcome-tuned Priority weights;
- sustained deliverability benchmark;
- universal applicability of Turkish score factors;
- validated commercial performance in every market adaptation.

## Conclusion

Scout's strongest contribution is not a particular lead list or a Turkey-specific score. It is the integration of discovery, independent scoring, market context, multilingual outreach governance, and deal outcomes into one configurable B2B intelligence architecture.
