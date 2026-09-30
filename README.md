<p align="center">
  <img src="assets/mcp-af-wordmark.svg" alt="mcp.af" width="420">
</p>

# mcp.af

**Independent MCP connector for Affinity. Built for Codex and compatible local ChatGPT workflows.**

Turn instructions into actions inside Affinity. From arranging objects to correcting text across an entire document.

**Describe the task. Let your assistant handle the steps.**

mcp.af connects a compatible local AI assistant to Affinity, allowing it to inspect documents, run scripts and perform edits. Your work stays in Affinity, using native objects and editable text.

## Useful across your design workflow

- Create, move and organise objects and layers.
- Build and arrange artboards.
- Inspect and update multiple text fields.
- Work with vector and raster content through Affinity's SDK.
- Generate previews and export files.

Use it for production tasks, repeated layouts and custom automation. General Affinity tools and SDK scripting remain available alongside shortcuts for repeated artboard and text operations.

Try asking:

- “Check my document for typos and suggest corrections.”
- “Arrange my artboards and align their positions to whole pixels before export.”
- “Organise my layers, put them in order, and give them clear names.”

## mcp.af 1.0

**Free for personal and commercial use.** Includes the connector, an Affinity workflow skill and agent-guided installation instructions. Each desktop ZIP also includes its native Node runtime and required dependencies; no separate Node.js, npm or Git installation is needed.

Download from [GitHub Releases](https://github.com/Stolgeth/mcp.af/releases).

| Package | Computer | Validation |
|---|---|---|
| `mcp.af-1.0.0-windows-x64.zip` | Windows x64 | Native installation and Affinity connection tested |
| `mcp.af-1.0.0-windows-arm64.zip` | Windows ARM64 | Native operation tested; confirmed by maintainer |
| `mcp.af-1.0.0-macos-arm64.zip` | Apple Silicon Mac | Native operation tested; confirmed by maintainer |

See [VALIDATION.md](VALIDATION.md) for the exact checks and remaining limits.

## Install with your assistant

1. Download and extract the ZIP for your computer's native operating system and CPU.
2. Open a local Codex task on the computer running Affinity. Attach the ZIP or provide its local path.
3. Paste [AGENT-PROMPT.txt](AGENT-PROMPT.txt). The agent reads the included installation skill, verifies the package and runs the installer.
4. Confirm the desktop Codex profile selected by the agent. If the operating system denies access, run the supplied command in a normal terminal under your desktop account.
5. After verified registration, restart Codex and open a new task. Look for **mcp.af** and the **mcp-af** workflow skill.

The ZIP does not execute itself when attached. The installer registers the connector and skill together and separately verifies registration and the Affinity connection. It preserves unrelated settings and plugins. Default location: `<selected-codex-home>/mcp-af`.

Read the [installation guide](INSTALL.md) for Windows and macOS commands, profile selection, diagnostics and removal. See [updating mcp.af](UPDATING.md) when a new release becomes available.

## Codex and ChatGPT compatibility

mcp.af is intended for **Codex and compatible local ChatGPT desktop plugin environments**. The tested installation route uses the Codex plugin CLI. A separate ChatGPT-only UI import route has not been validated.

You need Affinity running locally with its built-in MCP server enabled, a desktop assistant environment that can launch local MCP processes, and permission to write to its selected profile. Ordinary ChatGPT web chat or a cloud-only task cannot install and launch this connector on your computer by uploading a ZIP alone. Affinity and assistant accounts/features have their own requirements and terms.

## Local connection, editable output

The connector runs locally and connects to Affinity's local MCP server. Your documents remain native Affinity documents. Instructions and tool results are still processed by your assistant client and provider; a local connection does not make the AI conversation offline.

mcp.af adds no hosted relay, background service or telemetry. Existing-document batch operations check their targets and read back results. Mutating calls are not automatically replayed after a lost connection. An interrupted Affinity operation may leave partial edits; compound commands provide undo grouping, not transactional rollback.

The workflow skill checks document units before correcting fractional pixel placement and preserves surrounding text formatting when applying targeted corrections. See the [technical reference](docs/TECHNICAL.md) for tool scope and configuration.

## Licence

Copyright © 2026 **Tomáš Trlíček (@Stolgeth)**.

Free personal and commercial use under the [mcp.af Free Use, No Modification License](LICENSE). Modification of original additions requires written permission. Normal configuration, your independent scripts, and documents or artwork you create remain permitted. This is proprietary software, not an open-source project. Original development sources are kept private.

**Independent project. Not affiliated with, sponsored by or endorsed by Affinity, Canva or OpenAI.** Affinity, ChatGPT and Codex names belong to their respective owners.

## Support

Report issues at [Stolgeth/mcp.af](https://github.com/Stolgeth/mcp.af/issues), including OS/CPU, Affinity and desktop client versions, and reproduction steps. Remove private document content and credentials from diagnostics.

This public repository contains documentation and branding. Download the desktop packages from GitHub Releases. They include a bundled, minified connector without source maps, installation tools, human-readable workflow instructions and the required third-party runtime components. Minification is a distribution format, not a guarantee against inspection or reverse engineering.

These packages use local plugin installation. They are not a public OpenAI directory listing.
