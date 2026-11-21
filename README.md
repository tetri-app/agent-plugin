<p align="center"><img src="assets/logo.png" width="96" height="96" alt="Tetri"></p>

# Tetri for Claude Code and Codex

This plugin connects your Tetri workspace to Claude Code and Codex through the
Tetri MCP server at `https://api.tetri.app/mcp`. It adds one skill with the
workflows for recording transactions and retries. The agent can read wallets, balances,
transactions, budgets, goals, recurring transactions, reports and imports. It
can record a transaction after you confirm it.

You sign in to Tetri in the browser. The plugin holds no token and asks for
none.

Programmatic access is a Pro feature. You can allow the connection earlier. It
starts working once the workspace has Pro.

## Claude Code

```bash
claude plugin marketplace add tetri-app/agent-plugin
```

```bash
claude plugin install tetri@tetri
```

Start Claude Code, run `/mcp`, choose `tetri` and then Authenticate. The
browser opens the Tetri page "Connect an app to Tetri". Sign in, choose the
workspace, check the access and click Allow. Claude Code keeps the access in
the system credential store and renews it by itself.

## Codex

```bash
codex plugin marketplace add tetri-app/agent-plugin
```

```bash
codex plugin add tetri@tetri
```

```bash
codex mcp login tetri
```

The last command opens the same Tetri page in the browser. Add `--no-browser`
to get the address and open it yourself.

Codex asks before each tool call when `default_tools_approval_mode` is
`prompt`. Set a per-tool `approval_mode` of `prompt` for `create_transaction`,
`update_transaction` and `delete_transaction` so every write needs your yes.

## Access

The access covers one workspace and is no wider than your permissions in it.
To give the agent more, sign in again and allow the wider access. To use a
second workspace, add a second server entry with another name and sign in to
that workspace.

Connected apps are listed in Tetri settings, under API & MCP. Revoke closes the
access at once.

## Without a browser

A client that cannot sign in through a browser can use an MCP key instead.
Create the key in Tetri settings, under API & MCP. Tetri then shows the
commands for Claude Code and Codex, with the environment variable
`TETRI_API_TOKEN` in place of the token.

The skill also works on its own. Copy `skills/tetri` into `.agents/skills` of a
repository or into `$HOME/.agents/skills`, and into `.claude/skills` for Claude
Code.

## Use

Ask in plain words: "What is my balance in each wallet?", "Show last month's
spending by category", "Record 12.50 for lunch yesterday, paid from my card".
The agent shows the parsed transaction and waits for a clear yes before it
records anything.

## Versions

| Client      | Tested version |
| ----------- | -------------- |
| Claude Code | 2.1.289        |
| Codex       | 0.160.0        |

The plugin version is in `.claude-plugin/plugin.json` and
`.codex-plugin/plugin.json`. Both files carry the same number. Users get an
update only when the number changes. [CHANGELOG.md](CHANGELOG.md) lists each
change.

## License

[Apache-2.0](LICENSE). The license covers the plugin files. It grants no right
to the Tetri name or logo beyond naming the service the plugin connects to.
