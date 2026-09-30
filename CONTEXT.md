# Identity Verification Gateway

This context describes how merchants require and reuse identity checks across WordPress commerce workflows without tying their rules to one verification provider.

## Language

**Merchant**:
The person or organization that configures verification rules for a WordPress site.
_Avoid_: Customer, site owner, administrator

**Buyer**:
The person whose WooCommerce order may require verification.
_Avoid_: Customer, shopper

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
The set of verification claims a buyer must satisfy.
_Avoid_: Rule, verification level

**Verification Claim**:
A distinct fact established by a provider, such as identity or minimum age.
_Avoid_: Check, verification type

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
