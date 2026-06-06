# Changelog

All notable changes to exifree are documented here.

## [Unreleased]

### Added
- **Paste from clipboard** (Ctrl/⌘+V) — paste a screenshot or copied image straight into the tool without saving it to disk first. Works for single images and multiple images at once (batch). (#2)
- **Copy clean image to clipboard** — a "Copy" button next to Download sends the stripped image straight to the clipboard, ready to paste anywhere. JPEG/WebP output is converted to PNG for the copy (the downloaded file keeps its original format, lossless). Falls back gracefully on browsers without the async Clipboard API. (#3)
- **Per-file batch results** — when processing multiple images, each file now shows its own name and before→after size (or "already clean"), instead of only showing the last file.
- Metadata panel now shows a **"Removed from file"** list of exactly what was stripped, including technical blocks (gamma, sRGB, ICC profile, pixel density) that aren't readable EXIF fields but still carry bytes.

### Fixed
- **Landing page heading used an undefined font weight** — the hero `<h1>` requested `font-weight: 800`, but `fonts.css` only ships weights 300–700, so the browser synthesized a "faux bold". Changed to 700 (a real, loaded weight) for crisp rendering with no extra font file.
- **Contradictory results display** — the tool could show "0 fields / No metadata found" while also reporting a size reduction (e.g. "-21 B / -4%"). The reader only counted readable EXIF fields, but the stripper also removes technical chunks. The display now reports what was actually removed: shows the removed blocks when only technical data was stripped, and says "already clean" (hiding the size line) when nothing was removed. (#4)
- **Batch processing was completely broken** — `processBatch()` was referenced by the drag-drop and file-picker handlers but never defined, throwing a `ReferenceError` whenever more than one file was selected. Reimplemented: processes files sequentially, shows progress, skips failures gracefully, and enables the ZIP download.

### Changed
- Refactored single-file and batch flows to share one core stripping routine (`stripAndRender`), removing duplicated logic.
- **JSZip is now lazy-loaded** (like the HEIC converter) instead of via a trailing `<script>` tag, removing a script-load-order dependency and adding error handling if the library fails to load.
- **UI polish**: clearer drag-and-drop feedback ("Release to strip metadata" while dragging, with a subtle scale), a friendly error when a non-image file is dropped, guaranteed 48px touch targets on buttons, and a more compact Copy button on desktop.

### Internal
- Strippers (`stripJpeg`, `stripPng`, `stripWebp`) now return the list of segments/chunks they removed, used as the single source of truth for the UI.
- Fixed a minor memory leak: preview object URLs are now revoked before creating a new one and on reset.
- Removed dead code (`escapeHtml`, which was defined but never called — all untrusted metadata values are rendered via `textContent`, which is inherently XSS-safe) and simplified an always-true condition in the EXIF parser.
- Updated all repository links to `github.com/arnon-shub/exifree`.
