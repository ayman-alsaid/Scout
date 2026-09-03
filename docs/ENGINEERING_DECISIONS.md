# Scout — Engineering Decisions

## ADR-01 — One discovery engine, multiple commercial arms

**Decision:** normalize and enrich a company once, then allow multiple downstream opportunity types to consume the same entity.

**Alternative:** separate lead-generation systems for export-marketing and web-development opportunities.

**Reason:** the same business can be relevant to both. Duplicate scraping wastes cost and destroys signal overlap.

**Accepted cost:** the shared company model carries fields that matter to one arm but not another, increasing schema discipline requirements.

---

## ADR-02 — Keep Quality and Priority as separate scores

**Decision:** maintain an independent data-completeness score and business-priority score.

**Alternative:** collapse all signals into one universal lead score.

**Reason:** low data quality and low commercial value require different next actions. A high-value, poorly enriched record may deserve more investigation, not less.

**Accepted cost:** two scores require clearer operator education and UI explanation.

---

## ADR-03 — Deterministic score before learned score

**Decision:** use an explicit weighted Priority formula as the current ranking mechanism.

**Alternative:** claim or deploy a black-box predictive ranking before sufficient closed-deal outcomes exist.

**Reason:** deterministic weights are inspectable and operationally useful while outcome data accumulates.

**Accepted cost:** weights reflect domain judgment and are not yet empirically optimized against conversion outcomes.

---

## ADR-04 — Stream long discovery missions with SSE

**Decision:** expose search missions as Server-Sent Events.

**Alternative:** synchronous batch request returning only after completion.

**Reason:** long scans require observable progress; a silent multi-minute spinner is operationally ambiguous.

**Accepted cost:** connection lifecycle, reconnect behavior, browser backgrounding, and partial failure become explicit engineering concerns.

---

## ADR-05 — Classify web failures into commercial opportunity types

**Decision:** convert web-health observations into named states such as no website, domain expiration, SSL failure, server error, redirect loop, or outdated WordPress.

**Alternative:** expose raw HTTP/network diagnostics only.

**Reason:** business users need actionable opportunity categories, not infrastructure telemetry.

**Accepted cost:** classification rules need maintenance as web technologies and failure patterns evolve.

---

## ADR-06 — Provider-agnostic AI for outreach

**Decision:** support multiple model providers behind the same outreach interface.

**Alternative:** bind the product to one vendor's model and prompt behavior.

**Reason:** price, availability, multilingual quality, and model capabilities change quickly.

**Accepted cost:** prompts must remain more portable and conservative across providers.

---

## ADR-07 — Govern sending instead of maximizing send rate

**Decision:** daily caps, sending windows, spacing between messages, and delayed follow-up.

**Alternative:** send all generated messages immediately.

**Reason:** throughput is not the business objective; deliverability and relationship quality are.

**Accepted cost:** intentionally lower nominal sending speed.

---

## ADR-08 — Treat Turkey as a market adapter, not the architecture

**Decision:** distinguish the Turkish industrial reference configuration from the reusable Scout core.

**Alternative:** encode Turkish geography, OSB signals, and export assumptions as if they define B2B intelligence universally.

**Reason:** different markets require different sources, commercial signals, geography, languages, compliance rules, and score calibration.

**Accepted cost:** a new-market deployment requires explicit adapter work and domain validation rather than being marketed as an instant switch.

---

## ADR-09 — Exporter and importer workflows share the engine, not necessarily the scoring model

**Decision:** preserve the same discovery → enrichment → scoring → outreach → CRM orchestration while allowing scoring semantics to change by commercial orientation.

**Exporter configuration examples:** export readiness, destination-market fit, buyer/distributor signals.

**Importer configuration examples:** supplier fit, certification, capacity, logistics, distributor/manufacturer status.

**Reason:** the operational pipeline generalizes more cleanly than market-specific notions of value.

**Accepted cost:** score validation has to happen separately per use case.

---

## ADR-10 — Capture deal outcomes for future calibration

**Decision:** keep deal stages and values in the same operational system.

**Alternative:** stop the workflow after outreach or export leads to an external spreadsheet.

**Reason:** without downstream outcomes, there is no reliable path to learn whether ranking logic predicts commercial results.

**Accepted cost:** CRM functionality expands the system beyond pure discovery tooling.
