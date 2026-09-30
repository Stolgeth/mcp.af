# Affinity 3.3 SDK notes

Verified on Windows Affinity 3.3.0.4850, 2026-09-17. Other operating systems and SDK versions require their own regression run. Read version-matched MCP documentation instead of assuming all methods work.

- Import `Application` from `/application.js`; legacy `app` and extensionless imports still worked.
- Create artboards with `ShapeNodeDefinition`, `ShapeRectangle`, `Rectangle` and `DocumentCommand.createAddArtboard`. `createCompoundCommand` is a named export from `/commands.js`.
- Set `NodeDefinition.userDescription` while creating fields. Use `AddChildNodesCommandBuilder.setInsertionTarget(board)` explicitly. Geometry is in spread coordinates even for artboard children.
- Build text with `StoryBuilder.create().setToArtisticTextDefaultStyle(doc.dpi,doc.format)`, set glyph height/font/fill, then `ArtTextNodeDefinition.createFromStoryBuilder(Point, builder)`. Check `Font.isValid`.
- Seed `Selection.create(doc,textNode)` before `addSubSelectionForNode(node,TextSelection.create({begin,end}))` and `DocumentCommand.createSetText`. An empty Selection can silently do nothing.
- Center visible text bounds from `getSpreadBaseBox(false)` using `Transform.createTranslate`. Re-read bounds after text replacement.
- Linked frames: `DocumentCommand.createLinkTextFrame(a,b)` and `createUnlinkTextFrame(b)` worked. `textFrameInterface.textFlowNodes` identifies shared stories. Edits through a later frame use story-global positions, including positions before that frame's range. Unlinking the second frame left all text in the first and emptied the second; it did not split text at the visual boundary.
- `Document.saveAs`, close/reload and native PNG export worked when `Environment.permissions.fileSystem` was true and the target was in `fileSystemRoots`. Do not assume permission from earlier sessions. Reopening changes the session UUID.
- Native exceptions may arrive with upstream `isError:false`. This adapter uses a unique terminal execution envelope to normalize them; the app itself is unchanged.
- `PolyCurveNodeDefinition.create` now orders parameters `(curve, brushFill, lineFill, lineStyle, transparencyFill)`; recheck this signature when migrating old code.
- Ordinary spreads can contain layers without an `artboardInterface`. Check `node.artboardInterface?.isArtboardEnabled` rather than assuming that interface exists on every node.
- A raster `ImageNodeDefinition.bitmap` requires a bitmap, not a PixelBuffer. Use `pixelBuffer.createCompatibleBitmap(true)` before assignment. Match the document's raster format.
- Reparenting a child directly `Inside` a plain spread returned `COMMAND_FAILED` in the test. Moving it `Before`/`After` a known top-level sibling worked and preserved position; moving `Inside` a container layer also worked.
- Set the current spread with `DocumentCommand.createSetCurrentSpread(spread)` before editing another spread. Only switch when needed, since it clears selection. Batch edits require their target spread to already be current.
- Promise-based async completion worked with the native timer API. Use `async:true` and await the promise, or return it from a regular script. Native mode preserves original source semantics for compatibility.

General regression coverage also includes plain documents with no artboards, nested layers, rectangle/ellipse/curve creation, raster placement, fill changes, rotation/scaling, duplication/reparenting/deletion, selection rendering, PNG/SVG export and native save/reopen. Native UI dialogs, AI/network workflows and all raster filters were not exercised.

The in-app Scripts Panel and custom dialogs offer a further way to run stable recurring jobs without an AI round trip. A script expecting an external `INPUT` object needs a runnable wrapper or dialog before saving it to the library. Panel UI and custom Preflight automation are not tested connector features in this release.

Sources: [Affinity automation announcement](https://www.affinity.studio/blog/affinity-automation-scripting-claude), [September update](https://www.affinity.studio/blog/affinity-update-september-2026), and the live version-matched SDK modules. The preamble advertises [API signatures](https://sdk.affinity.studio/latest/js/); use MCP documentation if that site is unavailable.
