# Identity Verification Gateway

This context describes how merchants require and reuse identity checks across WordPress commerce workflows without tying their rules to one verification provider.

## Language

**Merchant**:
The person or organization that configures verification rules for a WordPress site.
_Avoid_: Customer, site owner, administrator

**Buyer**:
The person whose WooCommerce order may require verification.
_Avoid_: Customer, shopper

**Verified Subject**:
The merchant/site-scoped, provider-neutral identity anchor representing the human to whom Established Claims belong. A Verified Subject may exist for a logged-in or guest Buyer and is distinct from any WordPress user, WooCommerce order, Verification Session, or provider-native subject identifier. It contains no underlying identity evidence.
_Avoid_: WordPress user, order identity, provider subject, verification record

**Subject Binding**:
A durable relationship between a Verified Subject and an external principal that can participate in establishing that a current Buyer is that same Verified Subject. A Subject Binding is stronger than an order association or a matching contact identifier and does not, by itself, imply that every later interaction has established continuity.
_Avoid_: Order association, email match, phone match, generic link

**Subject Continuity**:
A current determination that the Buyer in an interaction is the same human represented by an existing Verified Subject, established through an acceptable active use of a Subject Binding. Subject Continuity allows that subject's Established Claims to be considered for reuse but does not itself make any claim eligible.
_Avoid_: Email match, same account data, claim validity, automatic reuse

**Subject Conflict**:
A state where evidence from a trusted verification journey indicates that an external principal already bound to one Verified Subject may instead be acting for a different human. Subject conflicts fail closed: existing bindings are not silently replaced, merged, or reused until the conflict is explicitly resolved.
_Avoid_: Duplicate subject, automatic rebind, account mismatch

**Verification Provider**:
An external service that performs identity checks and returns their results.
_Avoid_: Vendor, identity service

**Provider Adapter**:
The gateway boundary that translates provider-neutral verification operations and results to and from one Verification Provider. It does not own merchant rules, WooCommerce behavior, verification reuse decisions, or audit decisions.
_Avoid_: Provider integration, connector

**Verification Rule**:
A merchant-defined statement that connects a WooCommerce event and condition group to a verification policy and its outcomes.
_Avoid_: Automation, trigger

**WooCommerce Event**:
A supported point in an order's lifecycle at which verification rules are evaluated.
_Avoid_: Hook, trigger

**Condition Group**:
A nested set of order conditions joined with `AND` or `OR` logic.
_Avoid_: Filter, query

**Verification Policy**:
A reusable, declarative, unordered set of Claim Requirements that a Buyer must satisfy together. It describes required facts and assurance, not provider workflow order or session count.
_Avoid_: Rule, verification level, workflow

**Verification Claim**:
A provider-neutral fact the merchant requires to be established about a Buyer, such as identity or a configurable minimum age. A claim describes what must be true, not the verification method used to establish it.
_Avoid_: Check, verification type, verification method

**Verification Assurance**:
A provider-neutral constraint on how strongly or by what acceptable class of method a Verification Claim must be established, without exposing one provider's workflow as the business requirement.
_Avoid_: Claim, verification type, provider workflow

**Claim Requirement**:
A Verification Claim together with any Verification Assurance and freshness constraints that must be satisfied for that claim. Verification Policies are composed from Claim Requirements.
_Avoid_: Check, verification step

**Freshness Constraint**:
A maximum acceptable age for previously established evidence of a Verification Claim. It constrains reuse without extending any shorter provider-imposed validity or expiry.
_Avoid_: Expiration, session timeout

**Established Claim**:
The normalized historical fact that a Verification Claim was established for exactly one Verified Subject, including when it was established, achieved Verification Assurance, provider validity or expiry when known, origin references, and whether it was provider-native or derived. Later expiry, revocation, or newer evidence may make it ineligible for reuse without changing the fact that it was established. It does not contain underlying identity evidence.
_Avoid_: Identity evidence, verification document

**Claim Eligibility**:
Whether an Established Claim can satisfy a Claim Requirement at the time of evaluation, considering claim satisfaction semantics, required Verification Assurance, Freshness Constraint, and any known provider validity or expiry. Eligibility is evaluated per Claim Requirement.
_Avoid_: Claim existence, subject continuity, verification history

**Verification Outcome**:
The merchant-selected order behavior for a passed, failed, pending, or expired verification.
_Avoid_: Action, callback

**Verification Session**:
A provider-side attempt to establish one or more Verification Claims for a buyer, referenced by the gateway without storing sensitive identity evidence.
_Avoid_: Check, verification record

**Provider Observation**:
A normalized report of what a Verification Provider currently says about a Verification Session and its claims. It describes provider truth; it does not command gateway or WooCommerce behavior.
_Avoid_: Provider event, callback action

**Provider Disposition**:
A coarse provider-neutral description of whether a Verification Session is still in progress or has reached a provider-side terminal result. It is not the gateway's Verification lifecycle state.
_Avoid_: Verification status, order status

**Session Handoff**:
The provider-neutral information needed to send a buyer into a Verification Session, such as a hosted URL or client token.
_Avoid_: Redirect URL

**Provider Connection**:
A merchant-configured relationship between the gateway and one Verification Provider, including the credentials and provider-specific configuration needed to use that provider.
_Avoid_: Provider account, integration

**Verification Session Intent**:
The provider-neutral reason a Verification Session is being created or continued, such as a new verification, retry, continuation, reverification, or additional claims. A Provider Adapter translates that intent into provider-native behavior when supported.
_Avoid_: Session type, action
