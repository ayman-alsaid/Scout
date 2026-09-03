# Scout — Market Adaptation Model

## Reference market vs reusable architecture

Scout's industrial workflow uses **Turkey as the reference market**. That choice gives the platform a concrete geography, industrial taxonomy, business culture, language environment, and export context to design against.

It must not be confused with a product limitation.

The reusable thesis is:

```text
Discover entities systematically
        ↓
Normalize and enrich evidence
        ↓
Score data quality independently from commercial priority
        ↓
Classify opportunity type
        ↓
Add current market context
        ↓
Personalize and govern outreach
        ↓
Track relationship to outcome
```

The Turkish implementation supplies one concrete set of values for the market-specific layer.

## What is market-specific in the Turkish reference configuration

Examples include:

- 81-province geographic subdivision;
- Turkish business-category vocabulary;
- organized industrial zone (`OSB`) signal;
- Turkish export-market news queries;
- Turkish and destination-market outreach language;
- locally relevant trade fairs and seasonal commercial cycles;
- score weights reflecting the initial domain model.

These should be treated as **replaceable configuration and business logic**, not as universal rules.

## What stays reusable

- campaign/discovery orchestration;
- company/entity normalization;
- enrichment lifecycle;
- Quality Score concept;
- independent Priority Score concept;
- opportunity routing;
- streaming progress;
- scheduler;
- market-intelligence assembly;
- multilingual message generation;
- outreach queue and governance;
- CRM/deal stages;
- instrumentation required for later score calibration.

## New-country adaptation checklist

A serious market adaptation should review at least:

1. **Geographic hierarchy** — country → state/province/region → city/industrial zone as appropriate.
2. **Source coverage** — confirm which public/commercial data sources actually cover the market reliably.
3. **Business taxonomy** — local categories and sector terminology.
4. **Company identity rules** — duplicates, legal names, local identifiers.
5. **Commercial signals** — what evidence actually predicts opportunity in that market.
6. **Priority weights** — replace Turkish-specific assumptions and validate locally.
7. **Market intelligence** — local tenders, trade fairs, seasonality, import/export cycles.
8. **Languages and locale** — operator language, lead language, currency, date/number formats, RTL where required.
9. **Outreach policy** — local email/privacy/commercial-contact rules and cultural norms.
10. **Outcome calibration** — use real deal/procurement outcomes to tune scoring after sufficient volume exists.

## Exporter configuration

A country-side exporter workflow can use Scout to discover and rank domestic producers, then connect them with external demand.

```text
Domestic manufacturers
      ↓
Enrichment + export-readiness evidence
      ↓
Quality / Priority ranking
      ↓
Destination-market intelligence
      ↓
Buyer / distributor / importer discovery
      ↓
Multilingual outreach
      ↓
Commercial pipeline
```

Possible local Priority signals may include export registrations, certifications, production indicators, port/logistics proximity, B2B presence, industry-zone membership, or market-specific evidence. These are examples of adapter inputs, not claims about the current Turkish formula.

## Importer / sourcing configuration

An importer or procurement organization can invert the commercial lens:

```text
Supplier universe
      ↓
Supplier discovery
      ↓
Data completeness + sourcing priority
      ↓
Qualification / enrichment
      ↓
Multilingual supplier outreach
      ↓
Procurement / commercial pipeline
```

Potential sourcing signals can include certification, production capability, geography, category match, reputation, contactability, distributor/manufacturer role, or logistics relevance — but these require validation in the target market.

## Distributor and market-entry configuration

Scout can also be oriented toward:

- finding distributors in a target country;
- identifying resellers or channel partners;
- mapping regional market whitespace;
- ranking potential commercial partners;
- building a country-entry prospect pipeline.

Again, the reusable asset is the orchestration. The meaning of a "high priority" target belongs to the market adapter.

## Why this separation matters

A platform that says "our Turkey score works everywhere" would be making a weak generalization.

A platform that says "we can replace the Turkey-specific evidence model while preserving discovery, enrichment, ranking architecture, outreach governance, and outcome tracking" is making a narrower and more defensible engineering claim.

That is Scout's portability model.
