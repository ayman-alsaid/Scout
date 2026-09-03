# Scout — Testing & Verification

## Verification principle

The supplied project record distinguishes implementation evidence from commercial outcome evidence. This file preserves that distinction.

## Implemented / documented system evidence

The source material supports the following as implemented or directly documented in the project architecture:

- FastAPI backend with roughly 30 endpoints;
- React / TypeScript frontend;
- SSE discovery endpoint;
- company campaigns and persistent company database;
- Quality Score and Priority Score logic;
- 81-province Turkish industrial reference configuration;
- international Web Opportunities configuration covering 51 countries / 5 regions;
- asynchronous website checks with 10 concurrent checks in the supplied design;
- market/news intelligence integration;
- multilingual interface and outreach;
- six AI-provider paths behind provider management;
- governed outreach queue;
- Deals Pipeline;
- scheduler;
- JWT authentication and bcrypt password hashing.

## Test categories described in the technical record

### SSE discovery behavior

The technical case study identifies deliberate handling/testing concerns including:

- connection drops during a scan;
- browser-tab backgrounding / connection behavior;
- partial source failures or rate limiting;
- explicit degraded-state reporting rather than silent failure.

These are presented as the relevant verification categories introduced by the streaming architecture, not as a claim that every networking edge case is permanently solved.

### Authentication

The project case study describes tests using malformed and expired tokens across endpoint categories to verify that downstream routes do not silently accept invalid JWTs.

### Multilingual behavior

The source states that the 8-language interface, including Arabic RTL, was verified by direct development use across language settings.

## What remains unvalidated at scale

The source explicitly does **not** provide enough evidence to claim:

- a statistically reliable outreach-to-deal conversion rate;
- sustained inbox deliverability percentages;
- causal uplift from AI-generated outreach;
- Priority Score calibration against a large sample of closed deals;
- universal market performance outside the reference configurations.

## Market-adaptation verification

A new country or importer/exporter deployment should not inherit the Turkish market formula without testing. Recommended validation areas include:

- source recall/coverage in the target geography;
- duplicate-company normalization quality;
- local category accuracy;
- local contact-data completeness;
- score-feature relevance;
- score-weight calibration;
- local-language outreach quality;
- commercial-contact compliance and deliverability;
- ranking correlation with actual downstream outcomes.

## Evidence discipline

The correct claim is:

> Scout has an implemented configurable commercial-intelligence architecture with a Turkish industrial reference configuration and an international web-opportunity configuration.

The incorrect claim would be:

> Scout has already proven the same lead-scoring formula works in every market.

No such generalized validation is asserted here.
