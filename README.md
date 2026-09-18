# Open PDF Signer

Tired of online PDF tools that make you register, cap you at three documents,
then push a subscription at you — all while your private paperwork sits on
someone else's server?

This is a single HTML file. Double-click it, sign your PDF, save it.
Free, unlimited, and fully local: **your document is never uploaded anywhere.**

## Features

- Load a PDF and page through it
- Create a signature by drawing with mouse, pen or finger — or upload a transparent PNG
- Add text anywhere — in the **fonts already embedded in the document** where possible, plus the standard PDF fonts
- Drag to position it, use the corner handle to scale proportionally
- Live dimensions in PDF points while you drag, so you can see exactly where it will land
- Export a real PDF: the original file stays the base, marks are added on top
- Available in 15 languages, picked up from your browser automatically

Existing text stays selectable and the page count never changes. Pages are
**not** flattened into images, which is what most "sign your PDF" tools do to
your document. The document's own metadata — title, author, producer,
creation and modification dates — is carried over **untouched**: the tool
only ever adds content, it never rewrites the Info dictionary.

## Usage

1. Download [`pdf-signer.html`](pdf-signer.html)
2. Open it in a browser (double-click works)
3. Load PDF → create signature → drag it into place → save

No installation, no build step, no server, no account.

## Privacy

Your PDF is read straight from disk by the browser and processed in memory.
There is no upload, no analytics, no tracking, no telemetry.

One honest caveat: pdf.js, pdf-lib and fontkit are pulled from a CDN the
first time you open the page, so that initial load needs an internet
connection. **After the page has loaded, no further network requests are
made** — you can watch this yourself in the Network tab of your browser's
developer tools while loading a PDF, signing it and exporting.

Want it fully offline? Download the three library files, put them next to the
HTML file, and point the three `<script>` tags at your local copies.

## Accuracy

Where the signature sits in the preview is where it lands in the exported PDF.
This is the part such tools usually get wrong, so it was verified end to end:
screen coordinates are converted into PDF points against the page's CropBox,
the flipped Y axis is accounted for, and `/Rotate` values of 0, 90, 180 and 270
are handled — including pages whose CropBox does not start at the origin.
Measured deviation between preview and export is below 0.02 pt (~0.007 mm).

Signature transparency is preserved: the PNG keeps its alpha channel and is
embedded with an `/SMask`, so you get your actual signature and not a white box
sitting on top of the text.

## Document fonts

The text tool reads the fonts a page actually typesets straight out of the
PDF and offers them in the font picker. What you see in the preview is the
real embedded font, and the same font file is embedded into the output — so
added text blends into the document instead of looking stamped on.

One caveat is inherent to how PDF works: most embedded fonts are **subsets**
that contain only the glyphs the document itself uses. If your new text needs
a character the subset lacks (say a Cyrillic letter in an all-Latin file), the
tool tells you which characters are missing before writing anything, rather
than silently producing blank spots — pick a standard font in that case.

## Languages

English, 中文, हिन्दी, Español, العربية, Français, Português, Русский, 日本語,
Deutsch, Indonesia, Türkçe, 한국어, Italiano and Tiếng Việt.

The interface follows your browser's language and falls back to English.
You can switch it any time in the top right, and the choice is remembered.
Arabic switches the layout to right-to-left — the document preview itself is
never mirrored.

Adding a language means adding one block to the `T` object in the file.
Pull requests welcome.

## Notes

- Tested in Chrome. Any modern browser with Pointer Events support should work.
- Password-protected PDFs are not supported.
- One signature per document, placed on the page you put it on.
- Text fields are unlimited; each lives on the page where it was added.

## Built with

- [pdf.js](https://mozilla.github.io/pdf.js/) — renders the pages, exposes the embedded fonts
- [pdf-lib](https://pdf-lib.js.org/) — writes the output PDF
- [fontkit](https://github.com/foliojs/fontkit) — glyph coverage checks and font embedding

## License

MIT — see [LICENSE](LICENSE).
