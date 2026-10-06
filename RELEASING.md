# Releasing

How to publish a new version of the Apper MCP connector to the
[MCP Registry](https://registry.modelcontextprotocol.io).

The Registry entry is [server.json](server.json). It is published under the
`io.github.apperhq/*` namespace, which requires authenticating as an **Owner** of the
`apperhq` GitHub organization.

## Prerequisites

Install the `mcp-publisher` CLI once:

- **macOS / Linux (Homebrew):** `brew install mcp-publisher`
- **macOS / Linux (binary):**

  ```bash
  curl -L "https://github.com/modelcontextprotocol/registry/releases/latest/download/mcp-publisher_$(uname -s | tr '[:upper:]' '[:lower:]')_$(uname -m | sed 's/x86_64/amd64/;s/aarch64/arm64/').tar.gz" | tar xz mcp-publisher && sudo mv mcp-publisher /usr/local/bin/
  ```

- **Windows:** download `mcp-publisher_windows_amd64.tar.gz` from the
  [latest release](https://github.com/modelcontextprotocol/registry/releases/latest),
  extract it, and put `mcp-publisher.exe` somewhere on your PATH.

Verify with `mcp-publisher --help`.

## Release steps

1. **Update the version.** Edit the `version` field in [server.json](server.json) to
   match the connector version being released.

2. **Validate.**

   ```bash
   mcp-publisher validate
   ```

   Fix any errors until this passes. Note that `description` has a 100-character limit.

3. **Authenticate.**

   ```bash
   mcp-publisher login github
   ```

   You must authenticate as an Owner of the `apperhq` organization, and the
   authenticated session needs permission to read organization membership — the
   Registry checks your role to grant the `io.github.apperhq/*` namespace. If publish
   later fails with a 403 naming only your personal namespace, that permission is what
   is missing.

4. **Publish.**

   ```bash
   mcp-publisher publish
   ```

5. **Verify the listing is live.**

   ```bash
   curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=apper-mcp"
   ```

   Confirm the returned entry shows the expected name, version, description, and the
   `https://mcp.apper.io/v1/connect` endpoint.

6. **Commit the version bump** so this repository matches the published entry.

## Checking the current listing

- **API:** https://registry.modelcontextprotocol.io/v0.1/servers?search=apper-mcp
- **Registry site:** https://registry.modelcontextprotocol.io

## Notes

Only `server.json` is published to the Registry. Changes to the README, `CLAUDE.md`,
the Claude Code plugin, or any other file in this repository do not require a new
Registry release — commit and push them as normal.

Publishing is additive: each release adds a new version, and the most recent one is
marked as latest. Published versions cannot be withdrawn.

Registry publication can be automated through GitHub Actions later using
`mcp-publisher login github-oidc`, which authenticates from the workflow itself.
