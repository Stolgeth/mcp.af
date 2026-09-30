# Updating mcp.af

mcp.af 1.0 is the first public release. These instructions apply when a newer
mcp.af release becomes available. There is no automatic updater.

1. Download the new ZIP for your native OS and CPU from
   [Stolgeth/mcp.af Releases](https://github.com/Stolgeth/mcp.af/releases).
   Compare it with that release's SHA256SUMS.txt, then extract it into a new folder.
2. Finish tasks currently using mcp.af. Read the new release notes and included
   installation instructions before updating.
3. Run the new package's installer using the same --codex-home as your current
   desktop installation. If you chose a custom --root, supply that same root.
   The default is <codex-home>/mcp-af. Use the dry-run first, then install with
   the same arguments and the supported Codex CLI.
4. Run doctor with those same profile/root arguments. Confirm both actual
   plugin registration and the Affinity connection. A successful direct
   connection alone does not establish that the desktop plugin is registered.
5. Restart Codex and open a new local task. The plugin is mcp.af and the workflow
   skill is mcp-af. Confirm the reported version matches the downloaded release.

The installer uses the stable mcp-af@stolgeth-mcp-af identity and stores each
version in its own release directory. It retains previous mcp.af payloads and
preserves unrelated settings. It rejects different contents under an already
installed version number; corrected release files must have a new version.

If OS access is denied, use the provided quoted command in a normal terminal
under your desktop account. Do not select a sandbox profile to make the command
succeed. Report failed registration as incomplete. Automatic rollback is not
provided; keep diagnostics and use the project's support instructions if an
update cannot be verified.
