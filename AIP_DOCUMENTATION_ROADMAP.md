# AIP Documentation Fix Roadmap

Last updated: 2026-03-27

## Purpose

This document consolidates the current documentation problems found in the AIP / AdMesh docs into one place, then turns them into a concrete roadmap to fix.

The goal is to move the docs from:

- conceptually strong but implementation-ambiguous

to:

- conceptually clear
- schema-aligned
- implementable by platforms, operators, and brand agents without guesswork

## Current Assessment

- Concept clarity: strong
- Protocol precision: strong
- Schema / API consistency: mostly aligned
- Implementation readiness for external integrators: close, with one canonical naming decision still open

## Main Problems

This roadmap is now mostly in closure state rather than discovery state.

- Fixed items: 12
- Remaining unresolved items: 1
- Remaining unresolved item: canonical empty outcome name (`no_match`, `no_fill`, or `no_bid`)

## Proposed Roadmap

### Phase 1: Lock canonical vocabulary

Objective:

- make every core object and lifecycle term unambiguous

Tasks:

1. Choose canonical names for:
   - platform request
   - downstream context request
   - empty / no result status
2. Choose canonical event names and money units.
3. Propagate the canonical schema-defined decision-phase taxonomy across docs and examples.
4. Publish a short "v1.0 canonical terminology" page.

Output:

- one source of truth for names and enums

### Phase 2: Align schemas and OpenAPI

Objective:

- make the wire contracts match the narrative docs

Tasks:

1. Remove raw user text from downstream `ContextRequest`.
2. Align event objects and monetary units.
3. Align `Bid` docs, schema docs, and OpenAPI.
4. Normalize identifier names and references.

Output:

- one coherent API and schema surface

### Phase 3: Rewrite role pages against the canonical contracts

Objective:

- ensure human-readable docs match the schemas

Tasks:

1. Rewrite:
   - `operators.mdx`
   - `platforms.mdx`
   - `brand-agents.mdx`
   - `selection.mdx`
   - `auction.mdx`
   - `delegation.mdx`
   - `events.mdx`
2. Replace old field names and deprecated examples.
3. Clarify event ownership and verification responsibility.

Output:

- role pages that are safe to hand to integrators

### Phase 4: Separate protocol from reference implementation

Objective:

- distinguish AIP the protocol from AdMesh the operator

Tasks:

1. Mark which requirements are:
   - protocol normative
   - AdMesh reference implementation
   - optional implementation guidance
2. Move SDK-specific language into AdMesh implementation docs if needed.
3. Clarify whether publisher retrieval and operator monetization are one module or two.

Output:

- cleaner positioning and less architectural confusion

### Phase 5: Add implementer-ready examples

Objective:

- reduce ambiguity for first external integrations

Tasks:

1. Add one end-to-end recommend example.
2. Add one end-to-end delegate example.
3. Add a minimal valid bid example and a full bid example.
4. Add event emission examples with canonical producers.
5. Add one identifier trace example from request to settlement.

Output:

- examples that backend engineers can build from directly

## Suggested Priority Order

### P0

- privacy boundary correction
- event vocabulary and money-unit alignment
- delegation event ownership

### P1

- decision-phase taxonomy cleanup
- bid schema alignment

### P2

- protocol-vs-AdMesh separation
- expanded examples and onboarding docs

## Remaining Questions For You

Please answer these directly. They are the remaining decisions needed to clean this up properly.

1. What should be the single canonical empty outcome: `no_match`, `no_fill`, or `no_bid`?

## Recommended Defaults If You Want Me To Resolve Them

If you want me to make the remaining cleanup decisions for you, my recommendation is:

- empty outcome: `no_match`
- decision phases: use only the schema enum in `intent-taxonomy/agentic-intent-taxonomy.schema.json`: `awareness`, `research`, `consideration`, `decision`, `action`, `post_purchase`, `support`
- settlement paths: `CPX -> CPC -> CPA` for external click-out flows and `CPX -> CPE -> CPA` for delegated-session flows
- settlement paths are billing semantics only, not ranking rules

## Fixed

### Fixed 1. Canonical response naming

Problem:

- The docs used multiple names for the same operator-to-platform response:
  - `SelectionResult`
  - `PlatformResponse`
  - `auction_result`

Fix applied to roadmap:

- Canonical operator-to-platform response name fixed as `PlatformResponse`.
- `SelectionResult` and `auction_result` should be treated as deprecated names during cleanup.

Completed recommended fix:

- Picked one canonical term for the operator-to-platform response: `PlatformResponse`.
- Marked alternate names as deprecated for cleanup across docs, schemas, examples, and API descriptions.

Still open for this area:

- The canonical empty outcome name is not fixed yet. It still needs a decision between `no_match`, `no_fill`, and `no_bid`.

### Fixed 2. Canonical decision-phase taxonomy

Problem:

- Different docs used conflicting decision-phase vocabularies.

Fix applied to roadmap:

- The canonical v1.0 decision-phase taxonomy is now defined only by `intent-taxonomy/agentic-intent-taxonomy.schema.json`.
- The canonical enum is:
  - `awareness`
  - `research`
  - `consideration`
  - `decision`
  - `action`
  - `post_purchase`
  - `support`
- Any alternate vocabulary in docs should be treated as non-canonical and cleaned up against the schema.

Completed recommended fix:

- Replaced the open decision with a schema-first source of truth.
- Recorded the concrete 7-phase taxonomy directly in the roadmap.

### Fixed 3. Canonical CPX / CPC / CPE / CPA settlement semantics

Problem:

- The docs mixed `CPC` and `CPE` language without a canonical rule for when each applies.

Fix applied to roadmap:

- v1.0 supports both `CPC` and `CPE`, alongside `CPX` and `CPA`.
- Non-delegated external click-out flows use:
  - `CPX` when the ad is shown
  - `CPC` when the user clicks through to the advertiser's external website
  - `CPA` when the downstream conversion occurs
- Delegated-session flows use:
  - `CPX` when the ad is shown
  - `CPE` when the user has given consent and the delegated session actually starts
  - `CPA` when the delegated action or conversion is completed
- Docs should describe these as flow-specific settlement paths, not as one universal ordering.

Completed recommended fix:

- Replaced the open `CPC` vs `CPE` decision with one canonical v1.0 rule.
- Recorded separate external click-out and delegated-session settlement paths.

### Fixed 4. Operator SDKs are optional implementation artifacts

Problem:

- Some docs treated operator-provided SDKs as if they were required by the protocol itself.

Fix applied to roadmap:

- Operator SDKs are an AdMesh implementation choice, not a protocol requirement.
- Protocol conformance should depend on the canonical wire contracts, schemas, signatures, and event semantics.
- Operators may provide SDKs as convenience tooling or a reference implementation.
- Platforms should not be required to use an operator-provided SDK to be protocol-compliant.

Completed recommended fix:

- Replaced the open SDK decision with a protocol-vs-implementation boundary.
- Preserved SDK support as optional integration tooling without making it normative.

### Fixed 5. PAG and operator monetization are separate modules

Problem:

- The docs previously mixed publisher retrieval / `aip.json` access governance with the operator monetization flow.

Fix applied to docs:

- PAG is now treated as a separate publisher-access module adjacent to AIP's operator monetization flow.
- Monetization narrative pages no longer use `aip.json` or `editorial_domains` as part of the core operator request flow.
- PAG-facing pages now use more explicit naming, including `Publisher Access Governance (PAG)` and `PAG Publisher Declaration (aip.json)`.
- Navigation now labels the publisher section as `Publisher Access Governance (PAG)` to make the boundary obvious.

Completed recommended fix:

- Separated publisher retrieval from operator monetization in the docs narrative.
- Tightened page naming so readers can distinguish PAG from the commercial participation protocol at a glance.

### Fixed 6. Canonical identifier family

Problem:

- The docs used overlapping identifier names such as `message_id`, `context_id`, `auction_id`, and `response_id`, which weakened traceability.

Fix applied to roadmap:

- The canonical v1.0 identifier family is:
  - `request_id`
  - `context_request_id`
  - `bid_id`
  - `selection_id`
  - `serve_token`
- These identifiers map cleanly to the request, downstream context request, brand response, operator selection, and event/outcome tracking lifecycle.
- Alternate names such as `message_id`, `context_id`, `auction_id`, and `response_id` should be treated as deprecated during cleanup unless they are needed as internal aliases only.

Completed recommended fix:

- Replaced the open identifier decision with one canonical v1.0 naming family.
- Locked the primary traceability chain around `request_id -> context_request_id -> bid_id -> selection_id -> serve_token`.

### Fixed 7. Selection logic is separate from settlement semantics

Problem:

- Some docs described operator ranking as operator-defined, while others implied settlement ladders such as `CPA > CPC > CPX` were the actual selection rule.

Fix applied to docs:

- Selection and auction pages now state that operators define their own ranking and scoring logic.
- Settlement paths are now documented as billing semantics only:
  - external click-out: `CPX -> CPC -> CPA`
  - delegated session: `CPX -> CPE -> CPA`
- Distributed-auction and cloud quickstart pages no longer describe `CPA > CPC > CPX` as the selection rule.
- The `PlatformResponse` schema docs now include `CPE` as a valid billing model alongside `CPX`, `CPC`, and `CPA`.

Completed recommended fix:

- Removed the ranking-vs-settlement ambiguity from the docs.
- Locked operator-defined ranking and flow-specific billing semantics as separate concepts.

### Fixed 8. Public operator API request contract

Problem:

- The conceptual docs said the platform sends a `PlatformRequest`, but the OpenAPI spec exposed `/aip/context` and accepted `ContextRequest` from the platform.

Fix applied to docs:

- The public operator endpoint now exposes `PlatformRequest` only.
- The public OpenAPI request path is documented as `/aip/request`.
- The request body schema is now `PlatformRequest`.
- `ContextRequest` is documented as operator-generated and downstream-facing only.
- The Python sample now posts a `PlatformRequest` payload to `/aip/request`.

Completed recommended fix:

- OpenAPI now reflects the same message flow as the protocol docs.
- The platform-facing request is `PlatformRequest`.
- `ContextRequest` is treated as an internal/downstream contract.

### Fixed 9. Canonical brand agent payload

Problem:

- The narrative brand-agent docs described a simplified response shape that did not match the canonical `Bid` schema already defined in the schema docs and OpenAPI.

Fix applied to docs:

- The narrative docs now use the richer `Bid` schema consistently.
- `brand-agents.mdx` now describes canonical bid fields such as `pricing`, `budget`, `recommendation.creative_input`, `declared_relevance`, `valid_until`, and `delegation`.
- The end-to-end bid example now matches the existing `Bid` contract.
- The brand-agent FAQ now refers to canonical `Bid` payloads.

Completed recommended fix:

- Chose one canonical bid schema: the existing richer `Bid` schema.
- Rewrote the narrative brand-agent documentation to match the actual schema field names.
- Updated examples to use the canonical bid contract consistently.

### Fixed 10. Downstream raw text prohibition

Problem:

- Some docs implied raw query text or message history might be forwarded downstream through consent-controlled flags, even though the protocol narrative said brand agents should never receive raw user text.

Fix applied to docs:

- Raw query text and message history are now treated as an absolute no for downstream brand-agent sharing in v1.0.
- The `ContextRequest` docs now state this prohibition without exception language.
- The `PlatformRequest` docs no longer expose consent flags for forwarding raw query text or message history downstream.
- The `platform-request.json` schema now removes those downstream forwarding flags and keeps only `allow_identity_downstream`.
- The sample `PlatformRequest` payload now matches the updated rule.

Completed recommended fix:

- Removed raw user text from downstream-facing contracts and documentation.
- Fixed the protocol rule as an absolute prohibition for v1.0 rather than an optional downstream permission.

### Fixed 11. Lifecycle-first event vocabulary and micros-only money units

Problem:

- The narrative docs used lifecycle-first event names, but the OpenAPI wire contract still used billing-first names such as `cpx_exposure`, `cpc_click`, and `cpa_conversion`.
- Monetary fields also drifted across strings, cents, and micros.

Fix applied to docs:

- The OpenAPI event schemas now use lifecycle-first event names on the wire:
  - `exposure_shown`
  - `interaction_started`
  - `delegation_started`
  - `task_completed`
- The OpenAPI event contract now includes a dedicated `delegation_started` event schema.
- Monetary fields in event contracts now use `amount_micros` and `outcome_value_micros`.
- The event examples in `events.mdx` now use lifecycle-first names and micros-only monetary fields.

Completed recommended fix:

- Fixed the wire vocabulary as lifecycle-first in v1.0.
- Fixed `micros` as the only canonical money unit for event contracts in v1.0.

### Fixed 12. Canonical producer for `delegation_started`

Problem:

- The docs disagreed on who should emit `delegation_started`, which made delegated-session ownership ambiguous across platform, operator, and brand-agent implementations.

Fix applied to docs:

- The Operator is now the canonical producer of `delegation_started` in v1.0.
- The delegated-session flow now states that the Platform collects user consent, sends that consent to the Operator, the Operator confirms brand-agent availability, and then the Operator initiates the delegated session.
- The delegation, platform, protocol, state-machine, and end-to-end flow docs now describe the Operator as the mediator and event producer for delegated session start.
- Brand-agent docs now describe brand agents as accepting delegated sessions and submitting `task_completed`, not emitting `delegation_started`.

Completed recommended fix:

- Fixed one canonical producer for `delegation_started`: Operator.
- Separated user-consent collection, operator session initiation, and brand-agent outcome submission into distinct responsibilities.
