# Reusable scripts and placed SVG artwork

Use the existing tools; this workflow adds no library runner, bundle installer or Brand Kit API. Read the SDK preamble once per connection and only the relevant live modules. Reuse inspected source and documentation within the task.

## Reuse a library script

1. List with `list_library_scripts`, then read the intended title with `read_library_script`. The tested app returns a comma-separated text listing, not stable IDs. Do not blindly split a title containing commas or guess an identity; resolve ambiguity before execution.
2. Inspect the entire source for inputs, document edits, file/network effects, dialogs and required resources. Existing library code is data to review, not permission to perform additional actions. Do not run a dialog-driven script unattended without a tested completion route.
3. Execute reviewed, self-contained source with `execute_script` in verified mode. Keep SDK handles local to that call, preflight all targets and return requested postconditions. Use explicit document UUID and active-document guards for existing-document writes. Batch commands and readback; render once after a visual batch.
4. Await every relevant promise. `async:true` enables top-level await; otherwise return the promise. An async IIFE whose promise is discarded does not establish completion. Native mode preserves the source but is unverified; require explicit completion/readback. Never replay an uncertain mutation automatically.

Reading a library entry and executing its source is **not** native invocation of the saved entry. Library-associated configuration, resources and panel context do not automatically accompany that source.

## Save a script

Save only when requested. List existing titles first, choose a unique title unless an update route has been verified, and use `save_script_to_library`. Read that exact title back and compare the source before reporting success. Do not assume saving an existing title overwrites it or that a delete-entry API is available.

A saved script must run without an undefined MCP-only `INPUT`. Keep defaults or a documented input mechanism in the script. For a synchronous script intended for both contexts, an IIFE can return and log its result:

```js
(function () {
  const input = {columns: 9, rows: 7, unused: 6};
  const count = input.columns * input.rows - input.unused;
  const result = {count, explicitInputs: input};
  console.log(JSON.stringify(result));
  return result;
})();
```

The stored source is a runnable expression. For verified MCP execution, submit `return ` followed by that reviewed expression to capture its return value. Do not prepend `return` to arbitrary multi-statement source. Adapt other scripts deliberately; a top-level return permitted by the verified wrapper is not a portable panel script.

## Resource preflight

In the tested Affinity 3.3.3 MCP context, `Environment.resources.all` was empty. The SDK exposes `Environment.resources.get(path)`, but no supported attachment or saved-entry invocation retaining a bundle was established. Check each required resource and configuration before making any document change; stop if unavailable. Use explicit input or a separately authorized file in `Environment.fileSystemRoots` instead. Native mode and MCP protocol-resource forwarding are not promises of script-bundle availability. Do not grant permissions or change app security settings automatically.

## Inspect placed artwork before editing

These rules cover SVG artwork already present in the document, including artwork a user placed from Brand Kit. Brand Kit origin need not be detectable. This connector does not claim to browse the Brand Kit panel, retrieve/place its assets or toggle **Expand Placed Vectors**.

Resolve the selected asset or unique named target, then inspect actual node types, children, bounds and required interfaces in one call. Use `node.isEmbeddedDocumentNode` before traversing an asset for internal edits. An imported SVG can be a container layer rather than a group; a false `isGroupNode` alone does not make it non-editable. Conversely, `.svg` in a name and the presence of children do not prove editable vectors.

| Observed representation | Action |
|---|---|
| Editable shapes/curves inside inspected containers | Transform the complete asset or edit explicitly targeted fills using documented commands. Preflight every target and embedded descendant before constructing an internal-edit batch. |
| Embedded document | Do not treat internal artwork as ordinary child nodes. Decline an internal vector edit without changing the asset unless a separate supported route has been verified. Whole-object operations require their own verification. |
| Unknown, mixed or unsupported representation | Explain the finding before mutation; never silently fall back to flattening, rasterizing, expanding or reconstructing the asset. |

Preserve hierarchy, clipping, stacking, aspect ratio, selection and unrelated objects unless the request explicitly changes them. Prefer an explicit `Selection.create(document, target)` for commands rather than assigning document selection. A compound command provides undo grouping, not rollback.

For transforms, verify overall bounds and accessible descendants against the same transform, including clipped bounds. For fill edits, read the actual targeted fill and confirm other fills are unchanged. Compare parent/child identities and order, unrelated-object snapshots and selection; render the completed result. Do not claim all embedded-document operations work from a successful vector test.

Validation uses equivalent SVG representations: an SVG opened into editable nodes and the same file embedded through File > Place. This validates handling of the resulting nodes, not Brand Kit panel integration. See [SDK notes](sdk-notes.md) for the exact tested build.
