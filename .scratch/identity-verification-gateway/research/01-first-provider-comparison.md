# Research: Choose the First Verification Provider

Date: 2026-09-14

## Decision

Choose **Didit** as the first Verification Provider for V1.

Didit is the best fit for the first adapter because it combines very low merchant onboarding and usage friction, broad document coverage, first-class identity and minimum-age capabilities, a simple hosted browser flow, a real sandbox, public APIs, and strong deletion/retention controls.

Its main technical weakness is webhook durability. The gateway must treat webhooks as the fast path and reconcile incomplete sessions through the provider API rather than treating webhook delivery as the only source of truth.

Persona is the strongest alternative on product breadth and age assurance, but its commercial entry point is too heavy for the default provider of a WooCommerce extension.

Stripe Identity has excellent developer ergonomics and webhook reliability, but business-location/use-case restrictions make it a worse first adapter for a provider-agnostic WordPress plugin targeting broad merchant adoption.

## Why Didit wins V1

### Merchant adoption friction

A WooCommerce extension should not require a merchant to commit to an expensive identity-platform contract before trying the plugin.

Didit has a much lower-friction commercial entry point and public pay-as-you-go pricing.

Persona is much harder to justify as the default first provider because of its relatively high self-serve platform cost and annual commitment.

Stripe has straightforward per-verification pricing, but merchant availability is constrained by supported business locations and use cases.

### Verification Claims

V1 needs to prove at least:

- identity
- minimum age

Didit supports identity verification and age-related policies directly enough to prove both capabilities without forcing the gateway model to become provider-specific.

Stripe can expose verified date of birth and let the gateway derive minimum-age claims, but this is not identical to a provider-native age-assurance capability.

Persona is very strong on age assurance, but its cost/onboarding profile makes it better suited as a later premium provider.

### Hosted buyer flow

Didit's hosted-session model maps cleanly to WordPress and WooCommerce:

1. WordPress creates a provider session server-side.
2. WordPress stores only its own provider/session reference and normalized state.
3. The Buyer is redirected to the provider-hosted verification flow.
4. WordPress receives provider state through signed webhooks and API reconciliation.
5. Sensitive evidence remains with the Verification Provider.

This matches the product's privacy boundary.

### Webhook durability

Didit's webhook retry behavior is weaker than Stripe's or Persona's.

Therefore V1 must not encode:

`webhook received = provider truth`

The provider-neutral contract must support explicit status retrieval and reconciliation.

WordPress should schedule reconciliation for non-terminal sessions when webhook delivery is missing or ambiguous.

This requirement should be carried directly into:

**Define the Provider Capability Contract**

## Constraints for the provider-neutral contract

The next ticket must account for:

- Provider session creation
- Hosted verification URLs or client tokens
- Provider-specific session expiry semantics
- Status retrieval/reconciliation
- Signed webhook verification
- Non-guaranteed webhook ordering
- Non-guaranteed webhook durability
- Normalized Verification Claims
- Provider-native vs gateway-derived claims
- Provider-specific errors
- Terminal and non-terminal session states
- Deletion/redaction capability
- Retention metadata/capability
- Server-side merchant credentials
- No storage of identity documents, selfies, or biometric evidence in WordPress

## Risks

- Didit is smaller/newer than Stripe or Persona, so provider operational maturity should be revisited after V1 proves the abstraction.
- Reconciliation is mandatory, not optional hardening.
- Sandbox/test environments must never be treated as safe places for real Buyer identity media.
- If the future business model becomes centralized verification resale rather than BYO-provider credentials, provider commercial terms must be reviewed again.

## Primary sources

Didit:

- https://help.didit.me/documents-coverage/supported-documents-and-countries
- https://help.didit.me/getting-started/free-plan
- https://didit.me/pricing/
- https://docs.didit.me/sessions-api/create-session
- https://help.didit.me/integration/webhooks-basics
- https://help.didit.me/getting-started/sandbox-and-testing
- https://docs.didit.me/core-technology/id-verification/overview
- https://docs.didit.me/core-technology/age-estimation/overview
- https://help.didit.me/data-privacy/delete-verification-data
- https://help.didit.me/data-privacy/how-didit-protects-data

Stripe Identity:

- https://docs.stripe.com/identity/use-cases
- https://stripe.com/identity
- https://docs.stripe.com/api/identity/verification_sessions/create
- https://docs.stripe.com/identity/access-verification-results
- https://docs.stripe.com/identity/handle-verification-outcomes
- https://docs.stripe.com/webhooks
- https://docs.stripe.com/api/identity/verification_sessions/redact

Persona:

- https://withpersona.com/pricing/
- https://docs.withpersona.com/hosted-flow
- https://docs.withpersona.com/webhooks
- https://docs.withpersona.com/environments
- https://docs.withpersona.com/2025-10-27/rate-limiting
- https://docs.withpersona.com/api-reference/inquiries/redact-an-inquiry
