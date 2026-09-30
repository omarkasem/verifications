Type: grilling
Status: resolved
Blocked by: 01

# Define the Provider Capability Contract

## Question

What provider-neutral capabilities, inputs, outputs, errors, and lifecycle operations must the V1 gateway contract expose without pretending every provider behaves the same way?


## Answer

Define a strict **Provider Adapter** boundary between the gateway and each Verification Provider.

### Boundary

The Provider Adapter owns provider-facing concerns:

- provider API calls and authentication;
- provider-specific request and response translation;
- provider-specific status and error interpretation;
- webhook authentication/signature verification;
- provider-specific capability and recovery metadata.

The gateway owns product and workflow concerns:

- WooCommerce events and order behavior;
- Verification Rules and Verification Policies;
- verification reuse decisions;
- gateway lifecycle transitions;
- retry scheduling, queues, reconciliation scheduling, and persistence;
- audit decisions.

A Provider Adapter must not depend on WooCommerce orders, merchant rules, or gateway lifecycle state.

### Core contract

Every Provider Adapter must support these provider-neutral operations:

1. **Connection diagnostics** — validate as much of the configured Provider Connection as the provider allows, and return normalized diagnostics including checks that cannot be performed proactively.
2. **Capability discovery** — return effective capabilities for the configured Provider Connection, not merely theoretical provider capabilities.
3. **Create or continue a Verification Session** — accept a purpose-built provider-neutral request containing required Verification Claims, a gateway correlation/idempotency key, Session Handoff context, Verification Session Intent, and only the buyer/context data genuinely required by the provider.
4. **Retrieve/reconcile a Verification Session** — fetch current provider truth independently of webhook delivery.
5. **Normalize provider observations and failures** — translate provider-native results into gateway-understood observations and normalized errors while preserving sanitized provider-native diagnostics.

### Capabilities

Capabilities use a two-level model:

- a typed, structured core for concepts the gateway understands;
- namespaced provider-specific extension capabilities for features unique to one provider.

Core capability metadata may describe:

- supported Verification Claims and whether support is provider-native or safely derivable;
- supported Session Handoff mechanisms;
- webhook support;
- session continuation/reverification relationships;
- native creation idempotency guarantees;
- provider-side cancellation;
- data deletion/redaction;
- retention information availability;
- connection diagnostic abilities;
- retry/recovery hints.

Provider-specific extensions do not automatically influence gateway business logic. A provider-specific concept is promoted into the core contract only when the gateway itself needs to reason about it across providers.

### Verification Session requests and handoff

The gateway decides **what must be verified**; the Provider Adapter decides **how that intent is expressed to the provider**.

A Verification Session request must not contain WooCommerce or rule objects. It may contain:

- required Verification Claims;
- gateway-generated correlation and idempotency identifiers;
- return/redirect context;
- webhook context where required;
- optional locale;
- only the buyer information genuinely needed by the provider;
- provider-supported configuration derived from the Verification Policy;
- Verification Session Intent;
- a prior provider session reference when the requested intent and provider capability require one.

Verification Session Intent can represent provider-neutral meanings such as:

- new verification;
- retry;
- continuation;
- reverification;
- additional claims.

The adapter maps that intent to the provider's native mechanism when supported.

Session creation returns a typed **Session Handoff**, such as a hosted redirect or client token, rather than assuming one universal redirect flow.

### Claims

Adapters expose claim support as:

- **native** — the provider directly establishes the Verification Claim;
- **derivable** — sufficient provider-verified information exists for the adapter/gateway boundary to establish the claim safely;
- unsupported.

Derived support must never be represented as provider-native support.

The normal adapter contract passes normalized Verification Claim observations rather than unnecessary raw verified identity attributes. Identity documents, selfies, biometrics, document numbers, raw date of birth, addresses, and similar sensitive evidence must not cross the boundary merely to support claim derivation. Provider-native diagnostic metadata must be sanitized so it cannot become a backdoor for sensitive identity evidence.

### Provider Observation and disposition

Webhook processing and API reconciliation converge on the same normalized **Provider Observation** shape.

A Provider Observation describes provider truth; it never commands gateway or WooCommerce behavior.

It includes normalized information the gateway can reason about plus sanitized provider-native identifiers/status information useful for diagnostics and audit.

The Provider Adapter normalizes provider status only to a coarse **Provider Disposition**:

- non-terminal;
- succeeded;
- failed;
- cancelled;
- expired;
- unknown.

Provider Disposition is deliberately separate from the gateway Verification lifecycle, which is decided elsewhere.

### Errors and recovery

A failed identity check is a valid Provider Observation, not an exception.

Operational failures cross the adapter boundary as normalized categories such as:

- configuration;
- authentication;
- invalid request;
- unsupported;
- rate limited;
- temporary provider failure;
- provider rejected;
- malformed provider response;
- unknown provider failure.

Errors may include sanitized provider-native codes/messages and recovery metadata.

The Provider Adapter owns provider-specific knowledge about retryability, provider Retry-After information, idempotency guarantees, and recovery hints. The gateway owns the actual retry policy, queueing, backoff, reconciliation schedule, persistence, and maximum-attempt policy.

Every session-creation request carries a gateway-generated idempotency key. Adapters use native provider idempotency when available and explicitly declare the guarantees the configured provider connection actually offers.

### Webhooks

Webhook support is a declared capability rather than a universal assumption.

When supported, the Provider Adapter is responsible for:

- authenticating/verifying the webhook;
- identifying the referenced Verification Session;
- extracting provider event identifiers and timestamps when available;
- translating the payload into a Provider Observation.

The gateway later owns deduplication storage, replay handling, event ordering recovery, retry policy, and reconciliation behavior.

### Optional provider lifecycle and privacy capabilities

Provider-side cancellation is optional and capability-driven. Gateway cancellation must not depend on it.

Deletion and redaction are modeled as explicit data-disposal capabilities because providers may have different semantics. The contract must not pretend redaction and deletion are equivalent.

Retention information is exposed when reliably known and may explicitly be unknown. The adapter must not manufacture retention guarantees or dates the provider does not expose.

Reverification is not forced into one universal provider method. The gateway expresses Verification Session Intent and relationship context; the adapter uses whatever provider-native mechanism is supported, including a new session, continuation, linked reverification, or incremental/additional claims.
