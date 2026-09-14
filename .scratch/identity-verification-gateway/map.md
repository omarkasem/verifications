Label: wayfinder:map

# Chart the V1 Identity Verification Gateway

## Destination

A decision-complete V1 product and technical specification for a provider-agnostic WordPress identity verification plugin, proven with one verification provider and one end-to-end WooCommerce integration. A developer can plan implementation from it without guessing.

## Notes

- This map plans the product; it does not implement the plugin.
- Use the `grilling` and `domain-modeling` skills for product decisions, `prototype` for experience decisions, and `research` for facts outside this repository.
- V1 supports merchant-created WooCommerce verification rules with nested `AND`/`OR` condition groups, curated WooCommerce lifecycle events, verification policies, and outcomes.
- Multiple matching rules combine their verification requirements. Compatible claims should share one provider session where possible.
- Guest checkout is supported, but email alone cannot authorize reuse of a previous verification.
- Required verification fails closed during provider errors: the order remains blocked and the buyer can retry.
- V1 has one active provider connection. Provider routing and fallback remain V2 concerns.
- WordPress stores verification metadata and audit events, never identity documents, selfies, or biometric data.
- Merchants choose from supported events and outcomes. Developers may extend them in code; merchants do not enter raw PHP hooks or callbacks.
- Use the canonical language in `CONTEXT.md`.

## Decisions so far

## Not yet specified

- How the chosen provider's concrete limits reshape the provider-neutral contract and supported claims.
- Whether claim freshness varies by policy, provider, order context, or buyer risk.
- How rule edits affect orders already evaluated under an older rule version.
- How privacy retention, export, erasure, and audit requirements interact.
- The exact compatibility approach for WooCommerce HPOS, Checkout Blocks, and classic checkout.
- Which emails and merchant notices are essential once the buyer and order state journeys are clear.
- Which extension points must be public after the core event, condition, policy, and outcome models settle.
- Whether WordPress multisite, localization, and accessibility add V1 decisions after the two experience prototypes.
- The acceptance evidence required before the decision-complete specification is handed to implementation planning.

## Out of scope

- Membership plugins, WordPress registration, and protected content: deferred until the WooCommerce integration proves the core model.
- Multiple active providers, provider routing, and provider fallback: planned for V2.
- Marketplace and vendor KYC/KYB integrations: planned for V2.
- Storing identity documents, selfies, or biometric data in WordPress: providers remain responsible for sensitive evidence.
- Implementing the plugin: begins only after this map reaches its destination.
