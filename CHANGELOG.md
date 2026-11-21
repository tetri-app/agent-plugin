# Changelog

Each release names the tool list it ships with. A change to the tool list
raises the version in `.claude-plugin/plugin.json` and
`.codex-plugin/plugin.json` and adds a heading here.

## 0.2.0

- Browser sign-in through OAuth 2.1. The plugin holds no token: Claude Code
  signs in from `/mcp`, Codex with `codex mcp login tetri`.
- Published from `github.com/tetri-app/agent-plugin`.
- Tools: unchanged from 0.1.0.

## 0.1.0

- First release. One skill, `tetri`, and one MCP server entry for each client.
- Tools: `get_workspace_context`, `list_wallets`, `get_wallet`, `get_balances`,
  `list_accounts`, `list_categories`, `list_currencies`, `list_transactions`,
  `get_transaction`, `parse_transaction`, `create_transaction`,
  `get_budget_status`, `list_recurring_rules`, `list_goals`, `list_insights`,
  `get_report`, `list_imports`, `get_import`.
- Token sign-in. Claude Code keeps the token in the system credential store.
  Codex reads `TETRI_API_TOKEN` from the environment.
