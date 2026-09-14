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
