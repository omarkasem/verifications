# WordPress Identity Verification Gateway

## Idea

Build a provider-agnostic identity verification plugin for WordPress.

Instead of creating a plugin tied to one service like Persona, the plugin should allow site owners to connect different identity verification providers and use them across WooCommerce, membership plugins, WordPress registration, and protected content.

Possible providers:

- Didit
- Stripe Identity
- Persona
- Veriff
- iDenfy
- Sumsub
- Other verification services later

The main value is that the merchant is not locked into one provider.

They can configure verification rules inside WordPress, while the external provider handles the actual identity checks.

Example:

> If an order is above $2,500, require ID + selfie verification.

Or:

> If a user joins the "Professional" MemberPress plan, require identity verification before activating the membership.

Or:

> If a customer buys an age-restricted product, require 18+ verification.

The merchant should be able to switch verification providers later without rebuilding all of their WooCommerce or membership rules.

---

# Core Features

- Connect multiple identity verification providers
- Create reusable verification policies
- Create conditional verification rules
- Require different verification levels depending on the situation
- Verify a user once and reuse that verification for a configurable period
- Automatically require reverification when verification expires
- Track verification status inside WordPress
- Handle provider webhooks automatically
- Keep an audit/history of verification attempts
- Avoid storing sensitive identity documents inside WordPress

The verification provider should handle documents, selfies, biometrics, and other sensitive identity data.

WordPress should mainly store:

- Verification status
- Provider used
- Verification type
- Provider reference/session ID
- Verification date
- Expiration date

---

# WooCommerce Use Cases

Allow merchants to require verification based on rules such as:

- Specific products
- Product categories
- Order value
- Customer country
- New customer
- Customer role
- Every order
- Only selected orders

Examples:

> Product category = Alcohol  
> Require Age 18+ verification

> Order value > $2,500  
> Require Government ID + selfie

> Customer country = selected countries  
> Require identity verification

Verification could happen:

- Before checkout is completed
- After the order is created but before fulfillment

---

# Membership Use Cases

Integrate with popular membership plugins.

Initial targets:

- MemberPress
- Paid Memberships Pro
- WooCommerce Memberships

Examples:

> Free Membership  
> No verification

> Verified Member  
> Require identity verification

> Professional Membership  
> Require ID verification before activation

Membership can remain pending until verification succeeds.

The plugin should also support periodic reverification.

---

# WordPress Use Cases

Support:

- Registration verification
- User role verification
- Protected pages/content
- Verified-user status
- Verification expiration

Example:

> User registers as "Seller"  
> Require identity verification before account approval

---

# V1

V1 should focus on proving the main concept.

## Verification Providers

- Didit
- Stripe Identity
- Persona

## Integrations

- WooCommerce
- MemberPress
- Paid Memberships Pro
- WooCommerce Memberships
- WordPress registration
- Basic protected-content verification

## Features

- Connect verification providers
- Verification policies
- Conditional verification rules
- WooCommerce verification rules
- Membership verification rules
- Verify once / reuse previous verification
- Verification expiration
- Reverification
- Webhook processing
- Verification history
- Audit log
- Basic email notifications
- Manual retry/revoke verification
- WooCommerce HPOS support
- WooCommerce Checkout Block support
- Classic WooCommerce checkout support

---

# V2

V2 can expand the plugin into a more advanced verification platform.

## More Providers

Add providers such as:

- Veriff
- iDenfy
- Sumsub
- Other services based on customer demand

## Provider Routing

Allow merchants to use different providers for different verification types.

Example:

> Age verification → Didit

> High-value orders → Stripe Identity

> Advanced KYC → Persona

> Business verification → iDenfy

---

## Provider Fallback

Allow a backup provider.

Example:

> Primary provider: Didit

> Fallback provider: Stripe Identity

If the first provider is unavailable or cannot handle the verification, the plugin can use the fallback.

---

## Marketplace Verification

Integrate with marketplace plugins such as:

- Dokan
- WCFM
- WC Vendors

Possible features:

- Seller identity verification
- Vendor KYC
- Business verification / KYB
- Verify vendors before allowing products
- Verify vendors before payouts

---

## Advanced Rules

Add more conditions such as:

- Order risk score
- Payment method
- First order
- High order velocity
- Shipping/billing mismatch
- Fraud plugin signals

---

## Automation

On successful verification:

- Approve order
- Activate membership
- Add user role
- Grant content access
- Add verified-user badge
- Trigger webhook/API

On failure:

- Hold order
- Block access
- Keep membership pending
- Allow retry
- Notify admin

---

## Business Features

Possible V2 additions:

- Advanced reporting
- Public/developer API
- Outbound webhooks
- White-label verification pages
- Agency plans
- WordPress Multisite support
- More membership/form integrations
- Provider affiliate/reseller partnerships where allowed

---

# Positioning

Do not position the plugin as:

> Persona for WooCommerce

Position it as:

> **Identity verification for WordPress without vendor lock-in.**

Or:

> **The identity verification gateway for WooCommerce and WordPress memberships.**

The main advantage is that WordPress controls the rules and workflows while external verification providers handle the identity verification itself.