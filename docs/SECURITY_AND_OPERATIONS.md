# Scout — Security & Operational Controls

## Authentication and credential handling

The supplied project record describes:

- JWT authentication using HS256;
- 12-hour token expiry;
- bcrypt password hashing;
- secrets stored in environment variables rather than committed source;
- Dockerized services behind reverse proxy and managed TLS.

The technical case study also describes deliberate invalid-token verification using expired and malformed tokens across route categories.

## Outreach governance as an operational safety control

Scout's sending controls are not presented merely as marketing automation features. They constrain an automated system that could otherwise generate and send messages faster than is commercially healthy.

The supplied workflow includes:

- daily send limits;
- a defined 09:00–17:00 send window;
- inter-message gaps;
- follow-up after seven days of silence;
- tracked states such as `pending → sent → opened → replied`.

This is an important architectural boundary: AI generation capacity does not automatically imply permission to maximize message throughput.

## External-provider isolation

The project integrates several external services for discovery, market intelligence, AI generation, and email delivery. Keys are treated as configuration/secrets rather than public repository assets.

Provider abstraction reduces dependence on one AI vendor, but it does not remove third-party operational risks such as:

- provider outages;
- API quota changes;
- pricing changes;
- source coverage changes;
- model behavior differences;
- email-provider reputation policies.

## Long-running job observability

SSE is used to expose live discovery state. This improves visibility during long scans but requires explicit handling for disconnects, partial failures, and degraded upstream behavior.

## Market-specific compliance boundary

Global market portability does not mean global outreach rules are identical.

Any new-country adaptation must separately review:

- privacy/data rules;
- electronic marketing laws;
- B2B outreach permissions;
- opt-out/unsubscribe requirements;
- sender identity requirements;
- local language/cultural expectations;
- source terms of service and allowed usage.

These are market-adapter responsibilities, not assumptions inherited from the Turkish reference configuration.

## Public evidence boundary

This repository intentionally excludes secrets, credentials, production source, internal deployment details, and proprietary operational logic beyond what is needed to make the engineering decisions reviewable.
