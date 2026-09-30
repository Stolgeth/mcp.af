# Technical reference — mcp.af 1.0

## Installation and configuration

Use the desktop ZIP matching your native OS and CPU. It includes the required
runtime, connector and installation tools; no separate Node.js or npm setup is
needed. Follow the [installation guide](../INSTALL.md) and its explicit
Codex desktop-profile selection. The installer generates the correct local MCP
configuration and verifies the cached skill and runtime path.

The server communicates through MCP on stdin/stdout; diagnostics use stderr.
Launching it alone in a terminal does not provide an interactive application.
Use the documented environment settings below for configuration. After changing
plugin registration, restart Codex and open a new local task.

## Tools and call economy

| Tool | Work performed in one client call |
|---|---|
| `affinity_create_artboard_grid` | New document, N numbered artboards, editable centered text, full readback |
| `affinity_inspect_document` | Bounded artboard/text inventory, UUID, history and permissions |
| `affinity_arrange_artboards` | Ordered grid, optional text centering, geometry and hierarchy verification |
| `affinity_number_artboards` | Number existing fields, skip matching text, verify every target |
| `affinity_correct_text` | Guarded batch corrections, linked-story grouping, exact readback |
| `affinity_preview_artboard` | Cropped inline PNG via native export, temporary file cleanup |
| `affinity_status` | Cached connection/catalog status with freshness information |

All upstream tools remain available, and unfamiliar future tool names are forwarded without a fixed allowlist. Resources, resource templates and prompts are proxied when exposed upstream. `execute_script` supports verified synchronous/promise execution and unchanged native mode. See [the skill's batch reference](batch-tools.md) for the optional shortcuts.

Create the common 57-board layout with one call:

```json
{"count":57,"columns":9,"width":320,"height":240,"gap_x":40,"gap_y":40,"center_last_row":true}
```

Send this to `affinity_create_artboard_grid`, then one `render_spread` call for the overview. The built-in operation loads the SDK preamble internally once per connection; warm calls need only one upstream script request. Geometry-dependent creation uses three compound undo units inside that script. Existing-document batches use one compound command and verify the result in the same script. This removes per-artboard and per-field client calls; it is not a claim that Affinity itself became faster.

## Error and consistency contract

- Requests are serialized per bridge process. Separate clients/processes and interactive user edits are not globally locked. UUID/active-document checks, optional history guards and exact source checks help catch stale plans.
- Commands are never automatically replayed after transport errors or timeouts. A missing reply does not prove that a mutation failed to happen.
- Raw script responses contain `structuredContent: {ok,value,logs,logsTruncated}` and matching JSON text. Native exceptions set `isError:true`. Missing/duplicate terminal envelopes are unconfirmed errors. Ordinary console text containing “Error” is not treated as failure.
- Verified code runs inside a function; use `return` for a JSON-serializable value. Locals do not persist between calls. A returned promise is awaited; `async:true` also enables top-level `await`. Await all asynchronous work whose completion you need verified. Unawaited callbacks are outside the completion contract. Execution success does not verify edit postconditions.
- `mode:"native"` forwards the original script unchanged and preserves upstream results. Use it for scripts requiring the original execution context. Its `_meta.affinityExecutionVerified` is false; native error flags may be unreliable, so require explicit success output/readback. Do not combine native mode with `async:true`.
- Built-in tools additionally verify their requested postconditions. Preflight covers every target before execution. A compound command is an undo unit, not transactional rollback; native errors can still leave partial changes.
- Correct-text ranges use **Unicode code-point offsets**, not UTF-16 indices. They are half-open and story-global. Full-story replacement can change mixed formatting; targeted ranges preserve surrounding runs. Independent stories are never deduplicated by their text.
- `affinity_preview_artboard` writes only its uniquely named temporary file inside Affinity's permitted root. It reads the bytes through the app, removes that file and leaves selection/history unchanged. Permission denial is reported. If cleanup fails, the response identifies the remaining file.

By default, `execute_script` returns a structured execution envelope rather than unclassified console output. Set `mode:"native"` for original source/result semantics, or adapt async code to return/await its completion. Status result fields use snake_case. Other upstream tools' result formats remain unchanged.

## Configuration and scope

| Environment variable | Default |
|---|---|
| `AFFINITY_MCP_SSE_URL` | `http://localhost:6767/sse` |
| `AFFINITY_MCP_CONNECT_TIMEOUT_MS` | `5000` |
| `AFFINITY_MCP_REQUEST_TIMEOUT_MS` | `60000` |

Timeouts accept integers from 100 to 300000 milliseconds. Use the loopback default for normal local use. Pointing the adapter at another host makes that host the execution environment; this package does not add authentication to Affinity's server. Raw SDK access remains powerful and is governed by Affinity's permissions. The adapter adds no telemetry, network listener, or external reporting. Upstream tools that publish SDK hints/issues remain available and should only be used with the user's authorization.

Focused batch tools target directly contained, uniquely named artboards and fields. Layout targets must have equal sizes; rotated boards, nested group traversal and general-purpose rich-text formatting are outside the tested scope. Preview output is bounded to 2048 pixels and 10 MB. Tests cover the workflows below, not the entire Affinity SDK. See ../VALIDATION.md for platform evidence. Other app versions and multiple competing bridge processes are not covered by these tests.

## References

This connector depends directly on the official MCP TypeScript SDK rather than modifying a dependency in `node_modules`.

Structured results, tool errors, annotations, pagination and list-change notifications follow the [MCP tool specification](https://modelcontextprotocol.io/specification/2025-11-25/server/tools). SDK behavior was checked against documentation served by the running Affinity app. See [Affinity 3.3 SDK notes](sdk-notes.md) for the verified app features and official release sources.
