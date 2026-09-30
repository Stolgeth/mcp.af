# mcp.af 1.0 validation

Release version: 1.0.0. Build date: 30 September 2026.
Maintainer: Tomáš Trlíček (Stolgeth).

## Current release checks

| Check | Result |
|---|---|
| Automated execution, transport and installer suite | 21 test entries passed |
| Windows x64 native runtime and actual Codex CLI installation | Passed in isolated profiles |
| Windows ARM64 native operation | Passed; confirmed by maintainer on 30 September 2026 |
| macOS Apple Silicon native operation | Passed; confirmed by maintainer on 30 September 2026 |
| Plugin identity and version | mcp-af@stolgeth-mcp-af, version 1.0.0, enabled |
| Workflow and installation skills | mcp-af and mcp-af-install validate |
| Explicit profile overrides wrong inherited CODEX_HOME | Passed; no Affinity registration in ambient profile |
| Read-only config / missing explicit profile | Rejected before durable installation |
| Missing cached skill, wrong cached runtime, disabled plugin, stale registered marker | Registration failure, separate from connectivity |
| Repeat install, moved extraction, uninstall/reinstall | Passed |
| Real Affinity connection | 18 tools available: 7 added tools plus 11 native tools; server mcp-af 1.0.0 |
| Supplied icon | 1024×1024 PNG, preserved unchanged |
| Supplied wordmark | SVG, viewBox 0 0 1024 361, preserved unchanged |
| Bundled/minified runtime | General proxy transport tests, fresh Windows x64 installation, 57-artboard and general Affinity live suites passed |
| Distribution hygiene | Desktop ZIPs omit developer sources, maps, tests, examples, caches and local profiles |
| Privacy scan | No maintainer machine paths or personal test-folder names in distributed files; public authorship retained |

The installer regression harness is maintained in the private development repository.
Permission denial is reproduced using a read-only fixture config; the tests do
not change Windows account permissions or ACLs. The user's active desktop
profile and plugin were not replaced. CLI checks establish on-disk registration,
not that an already-running desktop has reloaded it.

Read-only connection checks did not create or edit Affinity documents. The test coverage does not establish live coverage for every possible
proofreading, layer ordering or pixel-alignment scenario.

## Archive validation

The private archive audit checks every inner file checksum, outer ZIP hashes,
unsafe/duplicate paths, native runtime CPU headers and macOS executable metadata.
It rejects developer source archives and source maps, and confirms packaged identities,
skill paths, metadata lengths and original artwork bytes. SHA256SUMS.txt identifies the delivered archives.

Each desktop package contains the bundled/minified connector, installer, doctor,
workflow instructions, branding, native runtime and required dependencies. All
dependency licence notices remain. Original source modules, developer tests,
build tooling and the full lockfile remain private; no source archive is published.
Archive checks reject known private machine paths and local profile/build
artifacts. Source test paths use fictional Unicode examples. The original icon
contains standard colour-profile and resolution metadata, with no personal text
or location metadata; both branding files remain byte-identical to those supplied.

The trimmed Windows runtime passed the general proxy transport tests, including
tools, prompts, resources, pagination, reconnection and prevention of mutation
replay. A new isolated Codex installation verified actual registration and a live
Affinity connection with 18 tools. The same bundled connector and cleaned dependencies are used in the ARM64
packages; these rebuilt archives pass content/architecture
checks, while the maintainer's earlier native Mac/ARM test confirmation remains
the native-operation evidence.

Three native Node 24.21.0 runtime downloads match their pinned official HTTPS
checksums. Detached release signatures were not verified. Locked dependency
versions and integrity metadata are checked during assembly; dependency notices
are retained. All archives carry the mcp.af licence revision 1.0. The upstream MIT
notice and other third-party rights are retained.

## Platform evidence and scope

The maintainer confirms successful native testing on Windows ARM64 and macOS
Apple Silicon. This report records that confirmation alongside the automated
Windows x64 results. Mac/ARM device and OS version details, individual test cases
and execution logs were not supplied to this build environment, so no more
specific coverage is claimed. All three release archives also pass the binary
architecture, checksum and packaged-content checks.

A fresh OS image without developer tools was not available. The Windows test
uses the packaged runtime and dependencies, but is not full clean-machine
certification. A separate ChatGPT-only UI import route is unverified. There is
no signed graphical installer. The custom licence has not received legal review.

## Editing coverage

The bundled/minified Windows release was retested directly in Affinity 3.3.0.4850: 57 artboards, 171 text fields
and 57 markers; grid creation and arrangement, numbering and corrections with
readback; Unicode and linked frames; ordinary documents and nested layers;
vector/raster work, transforms, native PNG/SVG rendering/export, save/reopen,
undo and awaited scripts. These are targeted tests, not complete SDK coverage. No claim is made that every Affinity API, filter, AI action or UI dialog
has been tested.

Public OpenAI directory eligibility is a separate matter. No public OpenAI directory submission is claimed.

## Public publication set

The public GitHub preparation contains an explicit allowlist of documentation, branding, installation documents and
three desktop ZIPs. The same ZIPs are available in the repository and as release assets.
There is no exported developer source tree, source archive, source map, lockfile,
build toolchain or private Git history. The public repository ignore rules allow
its prepared documentation, branding, three versioned ZIPs and checksum file. All public files are listed
with hashes in PUBLIC-FILES.json alongside the publication instructions.

Application JavaScript is bundled/minified, not encrypted. Workflow skills,
installation instructions and third-party code remain inspectable as required
for operation and their licences. This packaging keeps the original development
sources private; it does not promise protection against reverse engineering.
