# exifree

Strip metadata from photos — in the browser, zero server upload.

## What it does

- Reads and displays metadata from JPEG, PNG, and WebP images
  - **JPEG**: full EXIF/XMP/IPTC block detection
  - **PNG**: reads `tEXt`, `iTXt`, `zTXt`, `eXIf`, `iCCP` chunks
  - **WebP**: reads `EXIF`, `XMP`, `ICCP` chunks
- Strips metadata via **lossless binary stripping** — metadata segments are removed directly from the file binary, image data is never touched
  - **JPEG**: removes APP1–APP15 (EXIF, XMP, ICC, IPTC, Adobe) and COM segments
  - **PNG**: removes `tEXt`, `iTXt`, `zTXt`, `eXIf`, `tIME`, `iCCP`, `pHYs`, `cHRM`, `gAMA`, `sRGB` chunks
  - **WebP**: removes `EXIF`, `XMP`, `ICCP` chunks and clears flags in `VP8X` header
- Shows exactly what was removed from the file — including technical blocks (gamma, sRGB, ICC profile) that aren't readable EXIF fields but still carry data
- Output file size is nearly identical to the original — no re-encoding, no quality loss
- Supports HEIC/HEIF (iPhone photos) via a self-hosted WebAssembly converter — no external requests, the image never leaves the device
- **Paste from clipboard** — paste a screenshot directly with ⌘/Ctrl+V, no need to save it to disk first
- **Copy clean image to clipboard** — copy the stripped result straight back to the clipboard with one click, ready to paste anywhere (no need to download). JPEG/WebP output is converted to PNG for the copy; the downloaded file keeps its original format
- Supports batch processing — drop or paste multiple photos at once and download as a ZIP
- Shows file size before and after stripping
- Everything happens client-side — the image never reaches any server

## Why

Some apps strip EXIF, some don't, and behaviour changes between versions. Instead of guessing — clean once before sending.

## Usage

Open `app/index.html` in your browser. Drop, paste, or pick one or more images. Download the clean result — single file or ZIP batch — or copy it straight to your clipboard.

Live: [https://exifree.com/](https://exifree.com/)

Source: [github.com/arnon-shub/exifree](https://github.com/arnon-shub/exifree)

## License

MIT
