# GD Level Converter

A single-file, browser-based converter for Geometry Dash level files. It decodes and encodes `.gdlvl` and `.gmd` files and exports readable XML/Plist. Everything runs locally in your browser; no file is ever uploaded.

> **Made with AI.** This project was designed and written by an AI assistant ([Claude](https://www.anthropic.com/claude) by Anthropic) from a prompt and a set of sample level files. It has not been audited by a human developer. Review the code, and test converted files with copies of your levels before relying on it.

## Features

- **Auto-detection** of the input format (file or pasted text):
  - Raw XML/Plist
  - Base64 + Gzip encoded string (URL-safe or standard Base64)
  - Binary Gzip file
- **Two-way conversion:** encoded input is decoded to XML, and XML input is encoded.
- **Downloads** as `.gdlvl`, `.gmd`, `.xml` or `.txt`, plus a Copy button.
- **Optional k4 decoding:** expands the compressed level data (`k4`) into the readable object string.
- **Error handling** for empty input, invalid Base64, corrupt Gzip and non-XML results, with warnings for malformed XML.
- **Drag and drop** or paste; works on desktop and mobile.

## Usage

1. Open `gd-level-converter.html` in a modern browser (Chrome, Edge, Firefox, Safari).
2. Drop a level file onto the page, or paste an encoded string or XML into the input box.
3. Click **Convert File**.
4. Check the output preview, then download the format you need.

The download buttons rely on the browser saving files, so open the HTML file locally. Some hosted preview environments block downloads.

## File formats

| Format | Description | Compatibility |
|---|---|---|
| `.gdlvl` | Official native level export. URL-safe Base64 of a Gzip-compressed plist. | GD 2.2091 and newer |
| `.gmd` | Legacy format used by the GD Share mod and older community tools. | Needs the GD Share mod or a compatible tool |
| XML / Plist | Uncompressed level structure for editing and debugging. | Any text editor |

### Structure observed in the sample files

- `.gdlvl`: URL-safe Base64 (`-` and `_`, `=` padding) of a Gzip stream containing `<plist version="1.0" gjver="2.0">…</plist>`.
- `.gmd` (sample): uncompressed plist XML.
- In both, the `k4` key holds the level data, which is Gzip + Base64 again (starts with `H4sI`) and decodes to a `kS38,…` object string.

### What the converter writes

- **.gdlvl:** plist XML, Gzip-compressed, URL-safe Base64 with padding. `k4` is always kept compressed.
- **.gmd:** raw plist XML with `k4` compressed.
- **.xml / .txt:** plist XML. With **Decode level data (k4)** enabled, `k4` is expanded to plain text.

If you edit a decoded `k4` string, the converter recompresses it automatically when you produce `.gdlvl` or `.gmd` output. The compressed bytes will not be identical to what the game writes, but they are valid Gzip.

## Dependencies

- [pako](https://github.com/nodeca/pako) 2.1.0, loaded from cdnjs, for Gzip.
- If the CDN is unreachable, the page falls back to the browser's built-in `CompressionStream` / `DecompressionStream`.

For fully offline use, download `pako.min.js` and change the `<script src>` in the HTML file to point to it.

## Limitations

- Handles one file at a time (maximum 50 MB).
- Tested only against a small set of sample files. Importing re-encoded levels in the game has not been verified for every GD version.
- The `.gmd` output follows the sample: raw XML. Some older tools may expect other variants.

## Privacy

All processing happens client-side. No data is sent to any server, other than the one request that loads `pako` from the CDN.

## Disclaimer

This is an unofficial community tool and is not affiliated with or endorsed by RobTop Games. Geometry Dash is a trademark of its respective owner. Always keep backups of your levels.

## License

No license has been chosen. Add one before sharing or redistributing.
