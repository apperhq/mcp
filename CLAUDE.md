# Working with the Apper MCP connector

This repo configures the official remote Apper MCP server for use from Claude Code.

The server is remote and authenticates with OAuth 2.0 — the first tool call opens a
browser to sign in to Apper. There is nothing to install or run locally.

## Rules that apply to every workflow

**Read the instruction tools before generating anything.** `get_create_app_instructions`,
`get_edit_app_instructions`, `get_design_directives`, and `get_rls_policy_instructions`
return the current rules for how an Apper app must be built. They change as the
platform changes, so read them rather than relying on what worked last time.

**Get one message id per task.** Call `generate_message_id` once at the start of a
workflow and reuse that id across every subsequent call, so the work is grouped as a
single conversation in the Apper canvas rather than scattered across many.

**Read files before changing them.** Use `get_project_tree` and `read_files` first.
Prefer `patch_files` for targeted edits over `write_files`, which replaces whole files.

**Confirm before anything becomes public or gets destroyed.** `create_app` can create a
publicly reachable app. `write_files`, `patch_files`, and `apply_patch` commit, and
commits can build and deploy. `update_database` can drop data. Every `delete_*` tool is
destructive and should never run as part of a default flow — only when the user asks
for that specific deletion by name.

**Finish the build before reporting success.** Poll `get_build_status` until it
completes. If it fails, read the error, fix it, and check again. Never hand back a
preview URL for a build that did not succeed — say what failed and show the error.

**For app creation or modification workflows, finish with a successful build and
working preview.** Call `preview_app` and give the user the URL. If the app requires a
login, include the credentials the tool returns, taken from the app's actual data
rather than invented.

## Connector version

`check_mcp_version` reports whether the connected client is up to date. If it is not,
the user needs to reconnect the connector to pick up the current version.
