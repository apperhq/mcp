# Apper MCP

The official [Model Context Protocol](https://modelcontextprotocol.io) connector for
[Apper](https://apper.io).

Build and update complete applications in Apper from AI assistants. Describe what you
want, then create, update, publish, and preview it — configure its database, security
and permissions, and add server-side integrations, without setting up a separate
development stack.

## Connect

The connector is a remote MCP server. There is nothing to install or run locally.

```
https://mcp.apper.io/v1/connect
```

Transport is `streamable-http`, and authentication is OAuth 2.0 — your client opens a
browser to sign in to Apper on the first call.

For any MCP client that takes a JSON config:

```json
{
  "mcpServers": {
    "apper": {
      "type": "streamable-http",
      "url": "https://mcp.apper.io/v1/connect"
    }
  }
}
```

### Client setup

- **Cursor** — [apperhq/apper-cursor](https://github.com/apperhq/apper-cursor)
- **ChatGPT** — [Apper for ChatGPT](https://apper.io/learn/docs/mcp/apper-for-chatgpt)
- **Claude Code** — see the plugin below

## Claude Code plugin

This repository also contains a Claude Code plugin that bundles the connector, so it
can be added in one step rather than configured by hand.

See [CLAUDE.md](CLAUDE.md) for the rules Claude follows when working with an Apper
app — reading the instruction tools before generating, confirming before anything is
published or deleted, and finishing the build before reporting success.

## What you can do

The connector exposes tools for searching and creating apps, reading and writing
project files, running and checking builds, generating previews, managing the backend
schema and row-level security policies, and working with edge functions, environment
keys, and secrets.

Full tool documentation lives at
[apper.io/learn/docs/mcp](https://apper.io/learn/docs/mcp).

## Registry

This server is published to the official MCP Registry as
`io.github.apperhq/apper-mcp`.

Verify the current listing:

```bash
curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=apper-mcp"
```

The manifest is [server.json](server.json).

## Links

- [Apper](https://apper.io)
- [Documentation](https://apper.io/learn/docs/mcp)
- [Security policy](SECURITY.md)

## License

[Apache 2.0](LICENSE)
