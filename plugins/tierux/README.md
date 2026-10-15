# Tierux for Claude Code

Set up and verify mobile in-app purchases, subscriptions, and entitlements for Google
Play (primary), Apple App Store (with limitations), and Stripe web — without writing
store-verification code. Per-feature status: https://tierux.com/docs-platform-support

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
Claude follows each tool's confirmation flow, shows previews or setup steps, and
waits for your approval before it changes production billing config.

Docs: https://tierux.com/docs-mcp-setup
