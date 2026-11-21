---
name: tetri
description: Read a user's Tetri workspace and record, categorize, correct or delete transactions through the Tetri MCP tools. Use it when the user asks about their wallets, balances, transactions, budgets, goals, recurring transactions, reports or imports in Tetri, or asks to record, sort, categorize, fix or delete a transaction.
---

# Tetri

The Tetri MCP server gives the rules for money, writes and data in its own
instructions; follow them. This skill adds the workflows.

The server is bound to one workspace. Call `get_workspace_context` first when
you need the workspace name, base currency, plan, granted scopes or token
expiry, or when a tool is missing.

## Money in a write

- Send `foreign_amount` and `foreign_currency_id` together or not at all.
- Dates are ISO datetimes with an explicit UTC offset, for example
  `2026-11-14T09:30:00+04:00`.

## Record a transaction

1. Call `parse_transaction` with the user's text. It computes and writes
   nothing. Resolve wallets, accounts and categories with `list_accounts` and
   `list_categories`. Ask the user when a choice is ambiguous.
2. Show the parsed transaction: type, amount, currency, date, account and
   category. Wait for a clear yes. A question, a correction or silence is not a
   yes.
3. Call `create_transaction` once, with one idempotency key for this intent.
   The key is 16 to 128 characters from letters, digits, `_` and `-`.

## Categorize or correct a transaction

1. Find the transactions with `list_transactions` and read each one with
   `get_transaction`; its `revision` is the guard for the change.
2. To sort uncategorized transactions, list them, take the categories from
   `list_categories` and propose one category per transaction.
3. Show the proposed change for each transaction and wait for a clear yes.
4. Call `update_transaction` with the `id`, the `revision` as
   `expected_revision`, one idempotency key and only the fields that change:
   `category_id` (`null` removes the category), `description`, `date` or
   `account_id`. A transaction with amount splits cannot be updated, and a
   transfer keeps its accounts.
5. On a revision conflict, read the transaction again and show the user what
   changed before you retry.

Never call `delete_transaction` without an explicit request from the user to
delete that transaction. Show it first, wait for the yes, then pass its current
`revision`.

Idempotency:

- Reuse the same key and the same body when you retry after a timeout, a
  dropped connection or any result you could not read.
- Never reuse a key for a different transaction. A completed key replays its
  stored result for 24 hours, then the same key creates a new transaction. Read
  with `list_transactions` before you retry after a long gap.
- On a conflict result, read the transaction with `get_transaction` or
  `list_transactions` first. Do not retry with a new key until the user
  confirms that the first write did not happen.

## Errors

- On `isError` with upgrade text, tell the user which plan the feature needs.
  Do not retry.
- On a scope error, name the missing scope and stop.

## Out of reach

Imports and transaction templates cannot be created through MCP. Send the user
to the Tetri web app for those. To use a second workspace, the user adds a
second server entry with another name and signs in to that workspace.

## Tools

| Need                                   | Tool                                                      |
| -------------------------------------- | --------------------------------------------------------- |
| Workspace, plan, scopes, expiry        | `get_workspace_context`                                   |
| Wallets and their accounts             | `list_wallets`, `get_wallet`                              |
| Current balances                       | `get_balances`                                            |
| Accounts, categories, currencies       | `list_accounts`, `list_categories`, `list_currencies`     |
| Find or read transactions              | `list_transactions`, `get_transaction`                    |
| Draft and record a transaction         | `parse_transaction`, `create_transaction`                 |
| Change or delete a transaction         | `update_transaction`, `delete_transaction`                |
| Budgets, recurring transactions, goals | `get_budget_status`, `list_recurring_rules`, `list_goals` |
| Reports and insights                   | `get_report`, `list_insights`                             |
| Imports                                | `list_imports`, `get_import`                              |
