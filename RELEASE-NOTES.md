# mcp.af 1.1

**Independent MCP connector for Affinity, built for Codex and compatible local ChatGPT workflows.**

Version 1.1 improves reusable script workflows and editing of SVG artwork already in your document. Your work stays in Affinity as native objects and editable text.

## What changed

- **More dependable script reuse.** The workflow inspects saved source and its inputs before execution, saves self-contained scripts when requested, and reads back saved code to confirm persistence.
- **Clear script-context checks.** Required resources and configuration are checked before editing. Running saved source through MCP does not automatically attach its library bundle or panel context.
- **Representation-aware SVG edits.** Inspect editable vectors and embedded documents before choosing an edit. Whole-artwork transforms and targeted fills preserve hierarchy, clipping, selection and unrelated artwork. Unsupported internal edits to embedded artwork are declined without silently flattening or rebuilding it.
- **Updated validation and packaging.** Windows x64, Windows ARM64 and Apple Silicon packages retain the same plugin identity, explicit desktop-profile installation and bundled runtime. The current repository contains only the three 1.1 downloads; earlier packages remain in historical releases for rollback.

All 18 existing tools remain available. The execution wrapper, native passthrough, serialized calls and no-replay behavior are unchanged. No additional public tool, runtime dependency or background service was added.

## Validation

The bundled Windows x64 connector passed the 21 automated regression entries, real Codex fresh-install and 1.0 → 1.1 update checks, the 57-artboard/text suite, general document/vector/raster/export checks, and the focused script/SVG suite on **Affinity 3.3.3.5004**. Before/after SVG renders were visually inspected.

All three archives pass integrity, architecture, common-payload and privacy checks. Windows ARM64 and Apple Silicon native operation was confirmed by the maintainer for 1.0; the new 1.1 archives were audited here but were not rerun on those devices in this validation session. See [VALIDATION.md](VALIDATION.md) for exact evidence and limits.

## Download and update

| Package | Computer |
|---|---|
| `mcp.af-1.1.0-windows-x64.zip` | Windows x64 |
| `mcp.af-1.1.0-windows-arm64.zip` | Windows ARM64 |
| `mcp.af-1.1.0-macos-arm64.zip` | Apple Silicon Mac |

Compare your download with the release's `SHA256SUMS.txt`. Each ZIP contains the connector, native Node runtime, required dependencies, workflow skill, installation instructions and branding. No separate Node.js, npm or Python installation is needed.

For an existing installation, follow [UPDATING.md](UPDATING.md) and use the same desktop Codex profile and installation root. After verified registration, restart Codex and open a new local task. The plugin is **mcp.af**, its skill is **mcp-af**, and the release version is **1.1.0**.

## Scope

Placed-SVG validation uses equivalent native vector and embedded SVG representations. It does not automate Brand Kit browsing, asset placement or settings. Resource-bundle attachment and arbitrary embedded internal-vector editing are not supported by this workflow. General SDK access remains available for separately verified operations.

Free for personal and commercial use under the existing [mcp.af licence](LICENSE). Copyright © 2026 **Tomáš Trlíček (@Stolgeth)**. Required third-party notices remain included. Original development sources stay private.

Independent project; not affiliated with, sponsored by or endorsed by Affinity, Canva or OpenAI.
