# Install mcp.af for Codex / ChatGPT

Run this on the user's local computer, in the desktop user's environment.
These packages target Codex's supported local plugin commands and local desktop
surfaces sharing that plugin mechanism. Ordinary web chat/file upload does not
install local software. Exact drag/drop ZIP support depends on the client;
providing a local ZIP path is the fallback.

## Package contents and trust

The package includes a native Node 24.21.0 runtime, locked production
dependencies, adapter, Affinity skill, install skill, deterministic installer,
licences and checksums. Nothing is fetched by the installer. The host CLI may
perform its own normal marketplace operations.

Inspect setup/install.mjs and build-info.json. Compare the outer archive against
the publisher's SHA256SUMS.txt. The inner integrity.json detects corrupted or
unexpected package files; it is not an authenticated publisher signature.

Keep the extracted package intact until installation finishes. Some unzip tools
create extra files or strip permissions; use a clean extraction preserving the
listed contents. On macOS the ZIP marks runtime/bin/node executable. If an
extractor only lost its executable bit, restore that bit with chmod +x; do not
remove quarantine or disable Gatekeeper to work around an OS trust rejection.

## Select the desktop profile first

Every install, doctor and uninstall command requires `--codex-home` with the
absolute Codex profile directory actually used by the desktop app. `verify`
needs no profile. The installer never infers this from the agent's HOME,
USERPROFILE, LOCALAPPDATA or inherited CODEX_HOME. Those can belong to a sandbox
account even while Affinity is reachable.

Typical desktop paths are `C:\Users\<desktop-user>\.codex` on Windows and
`/Users/<desktop-user>/.codex` on macOS. A custom desktop CODEX_HOME takes
precedence. These are examples, not detection rules. Use known desktop launch
configuration or a profile path confirmed by the user. If the desktop user and
shell user differ, ask for the intended path rather than selecting a profile by
directory order or username. Open Codex once so that its profile already exists.

The same rule applies to Windows x64, Windows ARM64 and macOS Apple Silicon.
Known CodexSandboxOffline/Online profile paths are rejected. The executing
account may still be restricted; selecting a path does not grant write access.

## Commands

From the extracted directory, inspect the help and verify contents.

Windows PowerShell:

```powershell
& '.\runtime\node.exe' '.\setup\install.mjs' verify
& '.\runtime\node.exe' '.\setup\install.mjs' install --dry-run --codex-home 'C:\Users\<desktop-user>\.codex' --codex 'C:\actual\path\to\codex.exe'
& '.\runtime\node.exe' '.\setup\install.mjs' install --codex-home 'C:\Users\<desktop-user>\.codex' --codex 'C:\actual\path\to\codex.exe'
& '.\runtime\node.exe' '.\setup\install.mjs' doctor --codex-home 'C:\Users\<desktop-user>\.codex' --codex 'C:\actual\path\to\codex.exe'
```

macOS Apple Silicon:

```sh
./runtime/bin/node ./setup/install.mjs verify
./runtime/bin/node ./setup/install.mjs install --dry-run --codex-home '/Users/<desktop-user>/.codex' --codex /actual/path/to/codex
./runtime/bin/node ./setup/install.mjs install --codex-home '/Users/<desktop-user>/.codex' --codex /actual/path/to/codex
./runtime/bin/node ./setup/install.mjs doctor --codex-home '/Users/<desktop-user>/.codex' --codex /actual/path/to/codex
```

The profile and executable paths above are placeholders. The agent must resolve the real
Codex CLI; omit --codex when codex.exe (Windows) or codex (macOS) is on PATH.
Use the native desktop environment on Windows, not WSL. For Windows ARM64 use
the Windows ARM64 ZIP even if the assistant host itself runs under emulation.

No administrator installation is normally needed. Before copying the payload,
the installer probes write access to the selected profile, existing config,
plugin cache and installation location. A failed probe stops installation.
Dry-run performs these temporary write probes but does not install the payload
or register the plugin.

If Windows or macOS denies access, use the exact quoted command returned by the
installer in a normal PowerShell/Terminal window opened by the desktop user.
Keep the confirmed --codex-home and --root arguments. Do not run as a different
administrator, change ACLs, switch to a sandbox profile, or assume a Codex
permission grant changes the OS account. Report the installation as incomplete
until registration is verified in the intended profile.

## Permanent installation and registration

Default on all supported platforms: `<selected-codex-home>/mcp-af`.
This keeps the destination tied to the explicitly selected desktop profile.

Use --root DIRECTORY to choose another permanent location. Pass that same
option to doctor and uninstall. Never choose a temporary extraction directory.
The installer refuses unrelated non-empty directories and symbolic links.

For a future mcp.af update, use the same --codex-home and permanent root as
your existing mcp.af installation. See UPDATING.md in the desktop package or
docs/UPDATING.md in the source repository. Version 1.0 is the first public release.

The adapter/runtime live under releases/1.0.0-OS-CPU. The installer generates a
dedicated marketplace called stolgeth-mcp-af, containing mcp-af.
It calls codex plugin marketplace add, then codex plugin add; it does not replace
the user's personal marketplace or rewrite config.toml itself. Every CLI call,
including inspection and removal, receives the explicit profile as CODEX_HOME.
It uses the
verified .codex-plugin/plugin.json and .mcp.json compatibility format.

If the current desktop host does not expose the required CLI, use install
--no-register to prepare the permanent files. This is NOT a registered install.
Add the printed marketplace directory using the host's supported local plugin
interface. Do not invent a UI menu or registration API absent from that host.

Registration failures leave a recoverable prepared install. Resolve the error
and rerun the same command. Rerunning an intact release is safe. Different bytes
under an already-installed version are rejected; upgrades need a new version.
Old release payloads are retained. Automatic rollback and automatic updates are
not included in 1.0. If a process crashes holding .install-lock, verify that no
installer is active before removing that empty lock directory.

After registration, restart Codex and open a new local task. The displayed
plugin is **mcp.af**; its workflow skill is **mcp-af**. CLI checks
cannot prove that the already-running desktop has reloaded the plugin.
## Connection check

Doctor reads actual plugin registration in the selected profile, checks its
enabled state, version, source, cached Affinity skill and cached runtime settings.
It does not trust the install marker's saved registered flag. Supported cache
layouts use a version directory or `local`; an ambiguous/missing cache fails
verification rather than silently accepting a stale copy.

Separately, doctor starts the installed adapter, discovers tools and reads
Affinity status. It edits no document. Its JSON contains `registration` and
`connectivity` independently. A successful direct connection does not prove the
plugin is registered or loaded by the desktop.

- Exit 0: registration verified and Affinity native tools reachable.
- Exit 2: registration verified, but the adapter/Affinity connection is unavailable.
- Exit 3: registration is missing, disabled, mismatched or cannot be verified,
  even if Affinity is reachable. Read registration.error.
- Exit 1: profile, package, permission or other setup error.

Default endpoint: http://localhost:6767/sse. If Affinity uses another endpoint,
set AFFINITY_MCP_SSE_URL when installing and running doctor. No public listener,
system service or login startup task is installed.

## Removal

Run the same native runtime with setup/install.mjs uninstall and the same
--codex-home / --root / --codex arguments. This unregisters mcp-af@stolgeth-mcp-af
and that marketplace. Files remain at the printed owned location for recovery.
After stopping tasks using the adapter, those owned files can be removed.
Affinity itself, documents, saved scripts and other plugins are untouched.

## Platform status

Native operation has been tested on Windows x64, Windows ARM64 and macOS Apple
Silicon. This build environment runs the Windows x64 automated/local checks;
the maintainer confirms successful Windows ARM64 and macOS native-device tests.
See VALIDATION.md for evidence and scope. These ZIPs contain no signed graphical
installer.
