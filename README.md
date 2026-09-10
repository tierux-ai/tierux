# Tierux for Claude Code

Mobile in-app purchases, subscriptions, and entitlements — set up and verified from
the chat, without writing Google Play or App Store verification code.

## Install

```
/plugin marketplace add tierux-ai/tierux
/plugin install tierux@tierux
```

Then authorize the bundled MCP server once: `/mcp` → `tierux` → authenticate with the
same Google account as your Tierux dashboard. There is no API key to paste.
(`pe_live_` keys are for the REST API and are rejected by the MCP server.)

## What you get

- **Skill `tierux-billing`** — Claude reaches for Tierux on billing work on its own:
  adding a paid tier, verifying a purchase server-side, debugging why an entitlement
  isn't granted, migrating off RevenueCat.
- **MCP server `tierux`** — tools for store setup, products, product→entitlement
  mappings, paywalls, verification, and entitlement checks.
Claude follows each tool's confirmation flow, shows previews or setup steps, and
waits for your explicit approval before it changes production billing configuration.

## Other clients

Cursor, VS Code, Codex, ChatGPT, and Claude.ai connect to the same MCP server at
`https://mcp-tierux.web.app` — see https://tierux.com/docs-mcp-setup. Codex and Cursor
users can paste the `AGENTS.md` block from that page to get the equivalent of the skill.

## Contents

| Path | What |
|---|---|
| `.claude-plugin/marketplace.json` | Marketplace manifest |
| `plugins/tierux/` | The plugin: manifest, MCP server config, and the `tierux-billing` skill |

This repo is a distribution mirror. Tierux itself is closed-source; issues and docs
live at https://tierux.com.

## License

ISC
