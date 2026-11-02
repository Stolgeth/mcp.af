# mcp.af 1.1 validation

Release **1.1.0**, built and tested **2 November 2026**. Maintainer: Tomáš Trlíček (Stolgeth).

Windows live tests used **Affinity 3.3.3.5004 Win32 (Oct 30 2026)**, the packaged **Node 24.21.0** runtime and **Codex CLI 0.166.0-alpha.5.1**. Tests ran against the bundled/minified connector, not just the development sources.

## Results

| Check | Result |
|---|---|
| Automated execution, transport and installer suite | 21 test entries passed against source and packaged connector |
| General MCP forwarding | Tools, unknown tool names, prompts, resources, templates, pagination and recovery passed controlled-server checks |
| Execution contracts | Verified returns, awaited completion/rejection, native passthrough, bounded logs, serialization and no mutation replay passed |
| Fresh Windows x64 installation | Actual Codex registration, cached skill and live 18-tool Affinity connection passed in an isolated profile |
| 1.0.0 → 1.1.0 update | Actual CLI update passed; cached skill/reference and runtime point to 1.1.0; prior release retained |
| Explicit profile selection | Wrong inherited profile stayed unregistered; absent profile and read-only config were rejected before durable installation |
| Registration diagnostics | Missing skill, wrong runtime, disabled plugin and stale saved state correctly failed registration independently of connectivity |
| Repeat install / moved extraction / uninstall-reinstall | Passed |
| Script library | Existing title read without executing its source; unique test entry saved and exact source readback passed |
| Self-contained script | Explicit inputs produced the expected result in verified mode; original source emitted the same result in native mode |
| Resource preflight | Empty MCP resource bundle observed; missing resource detected before document mutation |
| SVG representations | Editable nested/clipped vectors and a real embedded SVG distinguished through SDK types |
| Editable SVG changes | Uniform whole-artwork scale/translation and targeted fill passed geometry, clipped-bound and colour readback |
| Preservation | Parent/child identity, order, selection, unrelated artwork, embedded representation and undo/redo passed |
| Unsupported internal embedded edit | Mixed-target plan rejected before commands; artwork and history unchanged |
| Visual SVG result | Before/after renders inspected; target changed, clipping remained correct, embedded copy and sentinel unchanged |
| 57-artboard regression | Creation, arrangement, numbering, 171 text fields, 57 markers, targeted corrections, Unicode, linked frames, guards and previews passed |
| General Affinity regression | Ordinary documents, nested layers, vector/raster work, transformations, duplication/reparenting/deletion, fill, PNG/SVG export, save/reopen and undo passed |
| Skill metadata and references | Simple frontmatter reviewed directly; local links checked; actual Codex cached workflow skill/reference verified |
| All three desktop archives | Inner/outer hashes, paths, CPU headers, macOS executable mode, common payload and privacy checks passed |

## SVG fixture and script evidence

The test SVG is original synthetic artwork with nested containers, a clipped stripe, three editable shapes and an unrelated sentinel. It was opened into native vector nodes, and the same SVG was embedded using File > Place during fixture setup. The saved native baseline was copied for each repeat run; all edits and assertions ran through MCP. This is equivalent-representation validation authorized for the production scope, not a Brand Kit panel test.

The script-library test created one uniquely titled, harmless self-contained entry and reused it for the packaged rerun. No pre-existing library source was executed or overwritten. No supported library-entry deletion API was assumed. Raw evidence and native test files stay private.

## Platform evidence

Windows x64 1.1 has the native evidence above. The maintainer confirmed successful Windows ARM64 and Apple Silicon Mac operation for 1.0 on 30 September 2026. That confirmation remains valid historical evidence; no new 1.1 native-device run on those platforms was performed here.

The 1.1 ARM64 archives use the same architecture-specific Node runtimes and locked dependency versions as 1.0. Their platform-independent payloads match the tested Windows x64 package. Architecture, executable metadata, hashes and content were audited for all three. This does not substitute for a new native-device regression run.

## Packaging and privacy

Each ZIP contains 1,417 files: the bundled/minified connector, installer and doctor, workflow/installation instructions, branding, native runtime and required dependencies/notices. The archives exclude developer sources, source maps, tests, fixtures, raw evidence, caches, local profiles and private paths. The supplied icon and wordmark remain byte-identical. Public authorship is intentional.

The three runtime downloads match pinned official HTTPS checksums; detached signatures were not verified. Dependency versions/integrity are checked against the lockfile during assembly. The licence remains revision 1.0; the product version is 1.1.0. Required third-party notices are preserved.

The public preparation contains only allowlisted docs, branding and approved downloads. The current repository and prepared release assets contain only the three 1.1 ZIPs and their companion documents. Both checksum files cover those same 1.1 downloads. Prior 1.0 packages remain in Git history and the superseded 1.0 release for rollback. No development source archive or private Git history is exported.

## Limits

These are focused regression checks, not certification of every SDK method, raster filter, custom dialog, AI action or concurrent-client scenario. No Brand Kit panel operation, script-bundle attachment or arbitrary embedded internal editing is claimed. The connector remains a general SDK adapter with workflow guidance and optional batch tools.

The user's active desktop profile was not replaced. CLI checks prove on-disk registration and connectivity, not that an already-running desktop has reloaded. A fresh operating-system image was not available. A separate ChatGPT-only UI import path and public OpenAI directory listing remain outside this validation.

Bundled JavaScript is minified, not encrypted; this distribution keeps development sources private but does not promise prevention of inspection or reverse engineering.
