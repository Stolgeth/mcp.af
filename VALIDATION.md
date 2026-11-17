# mcp.af 1.2.0 validation

17 November 2026. Windows x64 native testing used Affinity 3.3.3.5004. The desktop ZIPs bundle Node 24.21.0.

| Check | Evidence |
|---|---|
| Automated suite | 25 test entries passed. Coverage includes cold recovery, page failure and retry, raw schema forwarding, SDK search, missing capabilities, SSE and Streamable HTTP, installer safeguards, Unicode story paging and duplicate node names. |
| Native SDK and documents | Live checks passed for documentation search, native arguments, spread and selection inspection, nested children, raster and vector edits, PNG and SVG export, save and reopen. |
| Artboards and text | The 57-board suite passed with 171 fields, targeted and linked-story corrections, Unicode, undo and redo, explicit history guards and cropped previews. |
| Installation | The Windows installer completed a 1.1.0 to 1.2.0 upgrade in an isolated desktop profile. Registration, cache, retained prior release, repeat install and uninstall/reinstall were checked. |
| Distribution | Public allowlist, archive contents, SHA-256, executable architecture, cross-platform common payload and private-source exclusion are checked during release preparation. |

The Windows ARM64 and macOS Apple Silicon packages receive archive checks. Native-device evidence on those targets comes from 1.0. The transport test uses a controlled Streamable HTTP server. Live Affinity testing used its SSE endpoint. Compatibility with additional Affinity builds depends on the native capabilities they expose.
