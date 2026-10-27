# mcprush.com - Catalogue MCP

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Server-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=mcprush&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcprush.com%2Fmcp%22%7D)
[![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install_Server-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=mcprush&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcprush.com%2Fmcp%22%7D&quality=insiders)
[![Install in Visual Studio](https://img.shields.io/badge/Visual_Studio-Install_Server-C16FDE?style=flat-square&logo=visualstudio&logoColor=white)](https://vs-open.link/mcp-install?%7B%22name%22%3A%22mcprush%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcprush.com%2Fmcp%22%7D)

Use your AI assistant to search the [mcprush.com](https://mcprush.com) catalogue of MCP servers and agent skills. Ask for a server that does a task. You'll see what each one does, its security grade, its tools and its price, and you can get the exact command or config block that adds it to the client you use.

mcprush.com is a marketplace for MCP servers and agent skills. Developers publish and sell them there and can host their servers on it; users find, compare and install them. This server is the catalogue itself, as an MCP server your assistant can query.

> **Hosted remote MCP server — nothing to install or run locally.**
> Endpoint: `https://mcprush.com/mcp` (Streamable HTTP) · Sign-in: OAuth 2.1 with your mcprush account (free) · Read-only tools

This repository holds the documentation for the hosted server. mcprush.com operates the server itself, and its source code is not published here. It is listed in the official MCP Registry as [`com.mcprush/catalogue`](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.mcprush/catalogue).

## Tools

| Tool | What it returns |
| --- | --- |
| `search_servers` | Up to 10 MCP servers for a task named in `query` (e.g. "postgres", "calendar", "s3"). Each comes with its id, name, what it does, category, security grade, number of tools and price. Optional: `category`, `free_only`, `limit` (1–10). A very broad query returns fewer rows, marked `narrowed`. |
| `get_server` | One server, by the `id` that `search_servers` returned: what it does, up to 20 of its tools, its security grade, the number of scan findings, and its price. |
| `install_command` | For a `server` id and a `client` (default `claude-code`): the exact command to run, or the config block to write. Clients include `vscode`, `claude-code`, `claude-desktop`, `cursor`, `codex`, `gemini`, `grok`, `openai` (ChatGPT), `zed`, `windsurf` and others. Where a client cannot run that server, the answer says so and why. |
| `search_skills` | Up to 10 agent skills (folders of instructions an agent reads) for a task named in `query`: id, name, what it does and price. |
| `list_categories` | The headings the catalogue files servers under, each with roughly how many live servers it holds. |

All five tools are read-only (`readOnlyHint: true`). Descriptions that publishers wrote (`does`, `about`, `tools[].does`) come back labelled as untrusted data. Any description that addresses the agent with instructions is withheld. Your assistant can treat them as content, not as instructions.

**Security grades.** A scanned server carries a grade. In `search_servers` results, `grade_basis: "source"` means its code was scanned. `"tool-list"` means a remote server was graded from the tools it lists. `get_server` gives the same as `security.basis`. A server not yet scanned comes back with `grade: null`.

## Connect

On first use your client opens mcprush.com in the browser. Sign in (or create a free account) and press **Allow**. There is no key to copy.

### VS Code (GitHub Copilot)

Use an **Install** button above, or add the server from the command line:

```bash
code --add-mcp '{"name":"mcprush","type":"http","url":"https://mcprush.com/mcp"}'
```

You can also run **MCP: Add Server** from the Command Palette, choose **HTTP**, and enter `https://mcprush.com/mcp`. To share it with a workspace, add it to `.vscode/mcp.json`:

```json
{
  "servers": {
    "mcprush": {
      "type": "http",
      "url": "https://mcprush.com/mcp"
    }
  }
}
```

When the server starts, VS Code asks you to sign in. Then open Copilot Chat in **Agent** mode; the five tools appear in the tools picker.

### GitHub Copilot CLI

```bash
copilot mcp add --transport http mcprush https://mcprush.com/mcp
```

If Copilot CLI shows mcprush as needing sign-in, run `/mcp auth mcprush` in a session. It opens mcprush.com in your browser.

The same entry in the portable Copilot format is shared by Copilot CLI and VS Code's Copilot sessions. It goes in `~/.copilot/mcp-config.json`, or in `.mcp.json` at a workspace root:

```json
{
  "mcpServers": {
    "mcprush": {
      "type": "http",
      "url": "https://mcprush.com/mcp",
      "tools": ["*"]
    }
  }
}
```

### Visual Studio, JetBrains IDEs, Eclipse, Xcode (GitHub Copilot)

All four read the same `servers` block as VS Code:

```json
{
  "servers": {
    "mcprush": {
      "type": "http",
      "url": "https://mcprush.com/mcp"
    }
  }
}
```

Where it goes:

- **Visual Studio 2022 (17.14+) and 2026:** use the **Install in Visual Studio** button above. You can also put the block in `%USERPROFILE%\.mcp.json` (all solutions) or in `.mcp.json` in a solution folder, or add it in Copilot Chat under **Agent mode › Tools › +**. Sign in when Visual Studio shows **Authentication Required**.
- **JetBrains IDEs:** in Copilot Chat, go to **Agent mode › tools icon › Add MCP Tools**. Or open the Copilot status-bar icon and go to **Edit Settings › Model Context Protocol › Configure**.
- **Eclipse:** Preferences › GitHub Copilot › MCP › **Server Configurations**, then **Apply**.
- **Xcode:** GitHub Copilot for Xcode › Settings › MCP › **Edit Config**.

If your IDE cannot finish the sign-in, use a buyer key instead (see [Connect with a key](https://github.com/mcprush/mcprush-mcp#connect-with-a-key)). Add it to the `mcprush` entry as `"requestInit": {"headers": {"Authorization": "Bearer <key>"}}`.

### Copilot cloud agent on github.com

The Copilot cloud agent (formerly the coding agent) does not support remote servers that sign in with OAuth. Connect it with a buyer key instead:

1. Issue a key under [Dashboard › Access](https://mcprush.com/dashboard#access) (**Issue a key**). The free tier costs nothing.
2. In the repository, go to **Settings › Secrets and variables › Agents** and add a repository secret named `COPILOT_MCP_MCPRUSH_KEY` that holds the key. Only Agents secrets whose names start with `COPILOT_MCP_` reach MCP servers.
3. In the repository settings, under Copilot › MCP servers, add:

```json
{
  "mcpServers": {
    "mcprush": {
      "type": "http",
      "url": "https://mcprush.com/mcp",
      "headers": { "Authorization": "Bearer $COPILOT_MCP_MCPRUSH_KEY" },
      "tools": ["*"]
    }
  }
}
```

### Other clients

| Client | How to add it |
| --- | --- |
| Claude | [mcprush in the Claude directory](https://claude.ai/directory/mcprush) |
| ChatGPT | [mcprush in ChatGPT](https://chatgpt.com/plugins/mcprush) |
| Cursor | [mcprush for Cursor](https://cursor.directory/plugins/mcprushcom) |
| Claude Code | `claude mcp add --transport http --scope user mcprush https://mcprush.com/mcp`, then pick mcprush in `/mcp` and authenticate |
| Codex CLI | `codex mcp add mcprush --url https://mcprush.com/mcp` opens the sign-in in your browser; `codex mcp login mcprush` signs in later |

Any other client that supports remote MCP servers with OAuth can connect to `https://mcprush.com/mcp` the same way.

### Connect with a key

A client without OAuth can send a buyer key as `Authorization: Bearer <key>`. Issue a key under [Dashboard › Access](https://mcprush.com/dashboard#access). It starts with `mcpr_`, is shown once, and works for a year. A publisher API key from Studio is not accepted. Lines with a key for each client are at [mcprush.com/mcp](https://mcprush.com/mcp).

## Using it

Ask your assistant in plain words. For example:

- "Find an MCP server for Postgres with a good security grade."
- "What tools does that server have, and what can they reach?"
- "Give me the command to add it to VS Code."
- "Is there an agent skill for code review?"
- "Which categories does the mcprush catalogue have?"

A typical run:

1. `search_servers` finds candidates.
2. `get_server` shows one of them in detail.
3. `install_command` with `client: "vscode"` returns the line that adds it.

The assistant shows you that line, and nothing is installed until you run it. In `install_command`, `copilot` means Microsoft Copilot Studio. For GitHub Copilot, use `vscode` (or its alias `github-copilot`), which returns the VS Code line.

## Sign-in and permissions

- OAuth 2.1 with PKCE (S256), following the MCP authorization spec. Clients register with a Client ID Metadata Document (VS Code does this) or by dynamic client registration.
- Scope: `catalogue`. `offline_access` is accepted too; a refresh token is issued either way.
- Discovery: [`/.well-known/oauth-protected-resource/mcp`](https://mcprush.com/.well-known/oauth-protected-resource/mcp) and [`/.well-known/oauth-authorization-server`](https://mcprush.com/.well-known/oauth-authorization-server).
- The app never sees your password. It gets a one-hour access token, renewed while you stay connected. The renewal token lasts 60 days and is replaced each time it is used.
- A connected app can search and read the catalogue as you. It cannot buy anything, install anything on your machine, publish, or change your account settings. Its calls count toward your catalogue allowance and appear in your call log. The first call adds the free mcprush Catalogue listing to your installs. A paid listing comes back with its price and pricing page, but buying happens on mcprush.com, by you.
- Disconnect any time under [Connected apps](https://mcprush.com/oauth/apps); the connection ends at once. Removing the server in your client stops it from calling, but the connection stays listed there until you disconnect it or it goes unused for 60 days, unless the client revokes its tokens.

## Limits and privacy

- Every account gets 500 tool calls a calendar month, 60 a minute and 10 a second (20 on Pro). There are also hourly and daily caps, and a cap on how many different listings are shown in full. A refused call says when to try again.
- For each call, mcprush.com logs the time, the tool, the status, the size and which listings you were shown. It never logs your search words or the answers. See the [privacy policy](https://mcprush.com/privacy) and [terms](https://mcprush.com/terms).

## Troubleshooting

- **Every search is refused:** confirm your email address. Use the link in the sign-up email, or confirm it after signing in on mcprush.com.
- **The client asks you to sign in again:** a password change, or 60 days without use, ends the connection. Sign in again from your client: in VS Code, start the server again; in Copilot CLI, run `/mcp auth mcprush`.
- **A 401 from the cloud agent:** check the `COPILOT_MCP_MCPRUSH_KEY` secret. Also check under [Dashboard › Access](https://mcprush.com/dashboard#access) that the key is still active; keys last a year.
- **The cloud agent cannot reach the server:** if the session log shows a firewall warning for `mcprush.com`, add `mcprush.com` to the Copilot cloud agent's custom allowlist.

## Support

- Documentation: [mcprush.com/mcp](https://mcprush.com/mcp)
- Email: support@mcprush.com

## License

The documentation in this repository is released under the MIT License. See [LICENSE](https://github.com/mcprush/mcprush-mcp/blob/main/LICENSE).
