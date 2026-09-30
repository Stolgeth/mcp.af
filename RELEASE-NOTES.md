# mcp.af 1.0

**Independent MCP connector for Affinity, intended for Codex and compatible local ChatGPT workflows.**

Turn instructions into actions inside Affinity. From arranging objects to correcting text across an entire document.

Describe the task. Let your assistant handle the steps.

## What you can do

- Create, move and organise objects and layers.
- Build and arrange artboards.
- Inspect and update multiple text fields.
- Work with vector and raster content through Affinity's SDK.
- Generate previews and export files.

Your work stays in Affinity, using native objects and editable text. Use mcp.af for production tasks, repeated layouts and custom automation.

## Included

- General Affinity MCP connector and seven convenience tools for repeated work.
- Affinity workflow skill, named `mcp-af`.
- Agent-guided installation instructions and the `mcp-af-install` skill.
- Bundled native Node runtime, locked dependencies and integrity checks.
- Explicit desktop profile selection and separate checks for registration and Affinity connectivity.
- New mcp.af icon, wordmark and general workflow starter prompts.

Desktop downloads contain the runtime, installation tools, skills, branding and
required notices. Original development sources, tests, build tools and the full dependency lockfile
remain private. The distributed connector is bundled and minified without source maps. Local profiles and machine-specific
installation paths are not bundled; the installer creates paths for your computer.

This is the first public **mcp.af** release, tagged **v1.0.0**. Future releases will use the same mcp.af plugin identity and documented update process.

## Downloads and installation

| File | Intended computer |
|---|---|
| `mcp.af-1.0.0-windows-x64.zip` | Windows x64 |
| `mcp.af-1.0.0-windows-arm64.zip` | Windows ARM64 |
| `mcp.af-1.0.0-macos-arm64.zip` | Apple Silicon Mac |
| `SHA256SUMS.txt` | Checksums for all three desktop ZIPs |

Use the included `AGENT-PROMPT.txt` in a local Codex task. The agent identifies the desktop profile, verifies the package, runs the bundled installer, and checks registration and connectivity independently. If OS access is denied, run its quoted command in a normal terminal under your desktop account. Restart Codex and open a new task after registration.

No separate Node.js/npm installation is required. Affinity must be installed and its MCP server enabled. Ordinary web chat/file upload does not install local software. A separate ChatGPT-only UI import route is unverified.

## Validation and limits

Windows x64 installation and the live Affinity connection passed automated and local tests. The maintainer also confirms successful native operation on Windows ARM64 and macOS Apple Silicon. See `VALIDATION.md` for evidence and limitations. This release includes no signed graphical installer or automatic update service.

The connector runs locally; instructions and tool results are still processed by the assistant client/provider. Affinity edits are not transactional and interrupted operations may leave partial changes.

## Free personal and commercial use

By **Tomáš Trlíček (@Stolgeth)**. Original additions use the **mcp.af Free Use, No Modification License**, revision 1.0. Free personal and commercial use is permitted; modification of original additions requires written permission. Normal configuration, independent scripts and your creative output remain permitted. Third-party components retain their own terms.

Independent project. Not affiliated with, sponsored by or endorsed by Affinity, Canva or OpenAI.
