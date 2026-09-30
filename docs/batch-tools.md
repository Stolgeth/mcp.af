# Batch tools

Each successful mutation returns counts, verification and history steps. Failures set `isError:true`. `dry_run:true` preflights existing-document batches without executing commands. Optional `expected_history_position` guards a previously inspected document. Geometry uses spread units; created documents use pixels at 72 DPI.

## New grid

Call `affinity_create_artboard_grid` with `{count:57, columns:9, width:320, height:240, gap_x:40, gap_y:40, center_last_row:true}`. It creates editable fields named `number`, checks the selected font, then creates boards, fields and centers them in three undo units. Defaults: Arial Normal, 48 px. Optional `font_family`, `font_size`, `field`. A second call creates another document.

## Existing artboards

Use `document_session_uuid` from inspection. Supply `artboards:["1","2",...]` in the intended row-major order. `spread_index` is optional; omission uses the current spread. A batch mutation requires its spread to be current; switch through the SDK first when necessary. Artboards and text fields must be direct children and uniquely named. Nested groups are not traversed by these focused tools; general SDK scripts can traverse and edit them.

- Arrange: `columns`, optional `gap_x`, `gap_y`, `origin_x`, `origin_y`, `center_last_row`, `center_field`. Gap and origin default to zero. Supports equal-size, axis-aligned artboards; mixed sizes need a custom layout script. Children travel with their artboard. The named center field is centered using visible bounds; no optical adjustment is implied.
- Number: `field`, optional `start` (1), `step` (1), `prefix` and `suffix`. Replaces the whole field; use a consistently styled number field. Center afterwards if needed.
- Correct: `edits:[{artboard,field,before,after,replacements?}]`. No replacements means whole-story replacement, which can change mixed formatting. Targeted replacements preserve the surrounding formatting.

Example correction:

```json
{"artboard":"12","field":"status","before":"Ready for reviwe","after":"Ready for review","replacements":[{"begin":10,"end":16,"text":"review"}]}
```

Ranges are half-open `[begin,end)` and use Unicode code points. For `A😀 typo`, `typo` begins at 3, despite its UTF-16 index being 4. Before/after checks are exact, including combining marks and whitespace. Invalid, overlapping or conflicting linked-story edits fail before any command is executed. Edits targeting different frames of the same story must have identical before/after/ranges; they execute once per story and are verified through every requested field. Identical independent stories stay independent.

## Readback and previews

Inspection works on ordinary documents as well as artboard documents. It defaults to at most 100 artboards; `offset` and `limit` page them. `include_layers:true` adds a top-level layer inventory and `next_layer_offset`. `include_text:true` includes up to `field_limit` artboard fields (default 500) and 2000 characters per field. Truncation is explicit. Use a scoped SDK read for nested content or full source text of a longer field.

`affinity_preview_artboard` takes UUID, `artboard`, optional `spread_index` and `max_dimension` (default 1024, maximum 2048). It exports a PNG to a unique temporary filename in Affinity's first allowed filesystem root, reads it inside Affinity and removes that file. It returns an inline image, dimensions and byte count. Filesystem permission is required. A cleanup warning identifies a file that remains. Selection and document history are unchanged. `render_selection` is unsuitable for cropped artboard previews on the tested 3.3 build.

Limits: at most 500 new artboards, 1000 targeted existing artboards, 5000 correction targets and 2 million input characters. These are request bounds, not performance guarantees. Batch tools do not save automatically. Inspect after uncertain failure; a native command failure can leave partial changes.
