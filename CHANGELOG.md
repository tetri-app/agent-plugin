# Changelog

Each release names the tool list it ships with. A change to the tool list
raises the version in `.claude-plugin/plugin.json` and
`.codex-plugin/plugin.json` and adds a heading here.

## 0.3.0

- Needs the Tetri server release that ships these tool shapes.
- `get_report` lists every report and its parameters in its input schema and
  rejects a parameter the chosen report does not take.
- Tool results drop display-only and audit fields (icons, colors, authors,
  sort order, sparklines) and trailing zeros in amounts, so a page fits the
  client's output limit.
- `get_balances` returns balances only: per account, per wallet and workspace
  totals. `list_wallets` keeps the wallet details.
- `list_insights` leaves out onboarding tips.
- `parse_transaction` reads the language of the text when no `locale` is
  given and the text has no letters of the user locale's script, and an exact currency, date or category match is no longer taken
  over by a similar-sounding wallet or account.
- `get_report` `cash_flow_summary` returns a `null` savings rate when income
  is zero or below 1% of expenses.
- The server introduces itself as Tetri with its icon and gives the agent the
  money, write and data rules; the skill keeps the workflows.
- New tools `update_transaction` (category, description, date, account) and
  `delete_transaction`, each guarded by the `revision` read with
  `get_transaction`, behind the new scopes `transactions:update` and
  `transactions:delete`. The skill sorts uncategorized transactions with them.
- Codex shows the Tetri logo, description and example prompts. Licensed under
  Apache-2.0.
- Tools: the names of 0.1.0 plus `update_transaction` and
  `delete_transaction`.

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
