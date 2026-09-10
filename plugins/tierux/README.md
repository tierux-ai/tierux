# Tierux for Claude Code

Mobile in-app purchases, subscriptions, and entitlements — set up and verified from
the chat, without writing Google Play or App Store verification code.

## Install

```
/plugin marketplace add tierux-ai/tierux
/plugin install tierux@tierux
```

Then authorize the bundled MCP server once: `/mcp` → `tierux` → authenticate with the
same Google account as your Tierux dashboard. No API key.

## What you get

- **Skill `tierux-billing`** — Claude reaches for Tierux on billing work automatically.
- **MCP server `tierux`** — 25 tools for store setup, products, mappings, paywalls,
  verification, and entitlement checks.
- **Prompts** — `/mcp__tierux__setup_billing`, `add_paywall`, `verify_purchase_path`,
  `migrate_to_tierux`.

Every write shows a preview and waits for your approval before it touches production
billing config.

Docs: https://tierux.com/docs-mcp-setup
