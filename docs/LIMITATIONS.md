# Scout — Limitations

A credible commercial-intelligence system should document what it does not yet prove.

## Priority Score is not yet outcome-tuned

The current deterministic weighting is based on domain judgment about reachability and commercial value. The system captures downstream deal outcomes, but the supplied evidence does not support claiming that the weights have already been statistically calibrated against a large volume of closed deals.

## Turkey-specific signals are not universal

Organized industrial zones, Turkish province coverage, category vocabulary, export cycles, and the current weighting model are reference-market choices. A new country requires local equivalents and local validation.

## Market portability is architectural, not magical

A new market can reuse Scout's discovery/scoring/outreach/CRM architecture without rebuilding the entire product. It still requires work on data sources, taxonomy, geography, score signals, language, currency, compliance, and market expertise.

## Importer and exporter modes need different commercial semantics

The same workflow can support both orientations, but supplier-fit scoring is not identical to export-readiness scoring. A common engine does not justify a common formula.

## Data-source quality constrains Scout

Company intelligence can only be as complete as the sources available in a region. Source coverage, stale listings, duplicates, missing emails, and inconsistent categorization can affect downstream ranking.

## Provider abstraction does not eliminate provider variance

Different AI providers can vary in tone, multilingual quality, instruction following, latency, and cost. A shared interface reduces lock-in but cannot guarantee identical generated outreach.

## SSE has operational trade-offs

Streaming improves progress visibility but adds connection lifecycle complexity and requires careful handling of dropped clients, partial upstream failures, and state recovery.

## Web-health classification is an opportunity signal, not a diagnosis guarantee

Technical checks can identify likely issues, but remediation scope may require deeper inspection before any commercial promise is made.

## No sustained commercial benchmark is claimed

The supplied evidence does not support generalized claims for:

- email deliverability rate;
- response rate;
- outreach conversion rate;
- closed-deal rate;
- revenue uplift;
- superiority over commercial lead databases.

## Generalization beyond B2B markets is conceptual until piloted

Airtable research maps Scout's architecture onto adjacent domains such as corporate development, economic development, nonprofit prospect research, retail site selection, and recruiting. Those mappings are architectural research, not evidence of deployed pilots in those sectors.

## Public repository limitation

This repository is an engineering evidence artifact. It does not publish the private production source, so reviewers should treat architectural documentation as evidence of the documented system rather than as a source-code audit.
