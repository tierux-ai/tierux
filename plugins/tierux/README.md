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
Claude follows each tool's confirmation flow, shows previews or setup steps, and
waits for your approval before it changes production billing config.

Docs: https://tierux.com/docs-mcp-setup
