Type: grilling
Status: resolved
Blocked by: 02

# Define Claims and Verification Policy Composition

## Question

Which provider-neutral verification claims exist in V1, how do merchants combine them into a verification policy, and how are unsupported or incompatible claim combinations handled?


## Answer

Model a **Verification Policy** as a reusable, declarative, unordered AND-set of **Claim Requirements**. A policy describes what must be established about a Buyer and the acceptable assurance/freshness constraints; it does not prescribe provider workflow, session count, screens, or provider-specific configuration.

### Verification Claims

A **Verification Claim** is a provider-neutral fact, not a verification technique.

V1 core claim types are:

- **identity** — the Buyer's identity has been established.
- **minimum_age(N)** — the Buyer is at least the configured age threshold.

`minimum_age` is parameterized rather than split into separate claim types. Its implication rule is monotonic: `minimum_age(N)` satisfies `minimum_age(M)` when `N >= M`.

Identity and minimum age are independent claims. Neither implies the other.

The claim model is extensible through trusted code. A registered claim type defines a stable identifier, parameter schema, validation, implication/satisfaction semantics, composition/merge behavior, and compatibility rules. Merchants do not invent arbitrary claim strings.

### Verification Assurance

A **Verification Assurance** constrains how strongly or by what acceptable class of method a Verification Claim must be established without embedding a provider-specific workflow in the policy.

V1 core assurance concepts are:

- **Government Document Evidence**
- **Biometric Holder Match**

Implementation details such as a selfie, NFC, liveness mode, or a provider workflow/template are not business claims.

### Claim Requirements

A **Claim Requirement** consists of:

- one Verification Claim;
- zero or more Verification Assurance constraints;
- an optional Freshness Constraint.

All assurance constraints attached to one Claim Requirement are AND requirements. Assurance types may define their own implication, composition, and compatibility semantics.

A plain claim requirement may be satisfied whether the Established Claim was provider-native or safely derived. Native-vs-derived provenance remains capability/audit information. If the merchant cares how a fact was established, that requirement belongs in Verification Assurance.

### Verification Policy composition

All Claim Requirements inside one Verification Policy must be satisfied. V1 does not support merchant-authored OR between claims.

When requirements combine:

- identical claims collapse to one effective requirement;
- parameterized claims normalize to the strongest compatible requirement, e.g. `minimum_age(18)` plus `minimum_age(21)` becomes `minimum_age(21)`;
- compatible assurance constraints combine toward the stricter effective requirement;
- independent requirements coexist;
- logically contradictory requirements make the policy invalid.

Logical invalidity and provider incompatibility are distinct:

- **invalid policy** — the requirements contradict each other according to claim/assurance semantics;
- **unsupported by Provider Connection** — the policy is logically valid but the active Provider Connection cannot fulfill it.

Requirements are never silently discarded or weakened.

### Provider support

Policies remain provider-neutral. Provider-specific API settings, workflow IDs, templates, and similar knobs belong to Provider Connection / Provider Adapter configuration, not Verification Policies.

Policies are validated against the effective capabilities of the active Provider Connection when merchants configure or enable them. Unsupported policies cannot be enabled.

If provider capabilities later change, the gateway must not silently weaken an existing policy. Required verification fails closed at runtime and surfaces a clear diagnostic.

A Verification Policy does not require all Claim Requirements to be fulfilled by one Verification Session. Compatible requirements should share a session where possible, but provider capabilities and later lifecycle decisions determine whether one or more sessions are needed.

### Freshness

A **Freshness Constraint** may be specified per Claim Requirement as a maximum acceptable age for previously established evidence of that claim.

If a Claim Requirement has no Freshness Constraint, the policy imposes no additional freshness limit. This does not mean the claim is valid forever and does not override provider expiry, revocation, invalidation, or the safe-reuse rules defined elsewhere.

A merchant freshness requirement can shorten acceptable reuse but can never extend a shorter provider-imposed validity or expiry.

### Established Claims

An **Established Claim** stores normalized metadata sufficient for later policy evaluation and reuse decisions, including:

- the normalized Verification Claim;
- whether it was successfully established;
- when it was established;
- achieved Verification Assurance;
- provider-imposed validity/expiry when known;
- originating Verification Provider and Verification Session references;
- provider-native vs derived provenance.

It does not contain the underlying identity evidence such as documents, selfies, biometrics, raw date of birth, document numbers, or addresses.

### Policy validity

A Verification Policy must contain at least one Claim Requirement. An empty policy is invalid; situations that require no verification should simply not require a Verification Policy.
