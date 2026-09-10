---
name: tierux-billing
description: Add, configure, or debug paid subscriptions and in-app purchases in a mobile or web app — Google Play Billing, Apple StoreKit 2 / App Store Connect, or Stripe web checkout — including receipt and purchase-token verification, entitlement checks ("is this user pro?"), product-to-entitlement mapping, paywalls and win-back offers, subscription lifecycle webhooks (RTDN / App Store Server Notifications), and migrating off RevenueCat, Adapty, Qonversion, or hand-rolled receipt validation. Use whenever a user wants to charge for an app, gate a premium feature, verify a purchase server-side, or asks why an entitlement is not being granted.
argument-hint: "[what you want to set up, e.g. 'monthly pro tier on Android']"
---

# Tierux billing

Tierux verifies purchases server-side and turns them into entitlements your backend
can check. Use the `tierux` MCP tools instead of hand-writing Android Publisher API,
App Store Connect API, or StoreKit 2 JWS validation code.

## Before anything else

Check the `tierux` MCP server is connected — try `list_apps`.

- **Not connected / unauthorized?** The plugin ships the server config, so the user
  only needs to authorize it: tell them to run `/mcp`, pick `tierux`, and
  authenticate (Google sign-in, same account as their Tierux dashboard). There is no
  API key to paste. A `pe_live_` key is REST-only and will be rejected here.
- **No apps yet?** Propose `create_project`, using the package name / bundle id you
  can read out of the repo (`build.gradle`, `Info.plist`, `app.json`) rather than
  asking the user to retype it.

## Approval protocol — do not skip

Every write tool returns a **preview** plus a `confirmToken` and persists nothing on
that first call. Show the preview, get an explicit yes, then call the **same tool
again with identical arguments plus `confirmToken`**.

Never fabricate a token, never reuse one across tools, and never auto-confirm.
`create_play_product` and `create_app_store_product` write to the real store and are
irreversible.

## Setting up billing

1. `list_apps` → get the `appId` (all app-scoped tools take an optional `appId`;
   omitted means the token's default app). Never invent an id.
2. Connect the store: `setup_google_play`, `setup_app_store`, or `setup_stripe_web`.
   If the user does not have credentials to hand, call `setup_google_play` with **no
   arguments** — it returns an interactive 7-step guide you walk them through.
3. Create the SKU: `create_play_product` / `create_app_store_product`.
4. `create_product_mapping` — map `productId` → entitlement (`pro_monthly` → `pro`).
5. `generate_sdk_snippet` — read the repo first to decide which platforms to ask for,
   then wire the returned code in. Do not write the HTTP calls by hand.
6. `set_webhook` if the app needs subscription lifecycle events.

## Checking it works

`verify_purchase` (Google Play token) or `verify_apple_purchase` (StoreKit 2 signed
transaction JWS), then `check_entitlement` for that user. Both are safe to run
repeatedly.

## Paywalls

`create_paywall` → `update_paywall` (edits stay a **draft**) → `publish_paywall`
(this is what end users see — confirm before publishing). Also `translate_paywall`
for locales and `create_winback_offer` for churn-triggered offers.

## Debugging "the entitlement isn't granted"

Work down this list rather than guessing:

1. `get_product_mappings` — is the purchased `productId` actually mapped?
2. `verify_purchase` with the real token — read the returned `status` and `expiresAt`.
3. `check_entitlement` for the same `userId` used at verification time.
4. Confirm the client is sending the *same* `userId` the backend checks. `userId`,
   `productId`, and `entitlementId` are opaque keys matching `^[A-Za-z0-9._:-]+$` —
   emails and anything with `/`, `@`, or spaces are rejected with `400
   INVALID_REQUEST`. This is a common cause.

## Migrating off another provider

Inventory the existing integration in the repo first (SDK imports, receipt
validation, entitlement checks, webhook handlers) and show the user before changing
anything. Then recreate each product → entitlement pair with
`create_product_mapping`, replace the client and backend code with
`generate_sdk_snippet` output, and give a cutover plan that keeps existing
subscribers entitled.

## Errors

Failures return `isError: true` and `structuredContent.{ok, error, message}`. `error`
is a stable machine code; show the user `message`. Do not retry blindly — most
failures are missing credentials or an unmapped product.

## Known limits — say so rather than improvising

Apple support is implemented but not at parity with Google Play: S2S notifications,
win-back, and pay-what-you-want are not wired for iOS, and
`update_app_store_product` / `delete_app_store_product` are no-ops because the App
Store Connect API does not support programmatic edits after creation. App Store
Connect product setup remains partly manual.
