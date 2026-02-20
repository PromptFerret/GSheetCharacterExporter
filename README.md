# GSheet D&D Character Exporter

Export D&D 5e character sheet data from a public Google Sheet into clean Markdown or JSON, ready for pasting into LLMs. No server, no accounts — runs entirely in your browser.

**Live:** [promptferret.github.io/GSheetCharacterExporter](https://promptferret.github.io/GSheetCharacterExporter/)

## What It Does

1. Paste a public Google Sheets URL containing a D&D 5e character sheet
2. The tool fetches the sheet data via CSV export
3. Parses all character data using a position map matched to the sheet template
4. Outputs clean Markdown or structured JSON

The output is optimized for pasting into AI tools — Claude, ChatGPT, or any LLM that benefits from structured character data.

## Supported Sheet Templates

- **D&D 5e GSheet v4.0** (auto-detected)
- **D&D 5e GSheet v1.4** (auto-detected)

The tool reads the template version from a marker cell and uses the correct position map. Unsupported templates fall back to v4.0 layout.

## Features

- **Three output formats**: rendered preview, raw Markdown, structured JSON
- **Multi-tab support**: automatically discovers and fetches Additional, Inventory, and Spell Preparation tabs
- **Spell preparation tracking**: when a Spell Preparation tab exists, uses it as the source of truth for spell data (with source attribution and "always prepared" flags)
- **Download buttons**: save output as `.md` or `.json` files
- **Tab discovery UI**: shows which tabs were found, which were used, and which were ignored
- **Auto-detection of custom tabs**: unknown tabs are inspected and classified as inventory or additional content

## Requirements

- The Google Sheet **must be public** (Share → Anyone with the link → Viewer)
- The tool **requires HTTP** — must be served from a web server, not opened as `file://` (CORS restriction on Google Sheets API)

## Getting Started

### Use it online

Visit [promptferret.github.io/GSheetCharacterExporter](https://promptferret.github.io/GSheetCharacterExporter/) — nothing to install.

### Run locally

```bash
git clone https://github.com/PromptFerret/GSheetCharacterExporter.git
cd GSheetCharacterExporter

# Start any local HTTP server:
python3 -m http.server 8000
# or
npx serve .
```

Then open `http://localhost:8000` in your browser. A local server is required because the tool uses `fetch()` to access Google Sheets, which is blocked from `file://` origins.

### Host it yourself

Copy `index.html` to any web server or static hosting that serves over HTTP/HTTPS. Single file, no dependencies.

## How It Works

The tool uses Google Sheets' direct CSV export endpoint (`/export?format=csv&gid=...`) to fetch raw cell data. Each template version has a hardcoded position map — every field is read from a known [row, col] coordinate. No scanning or searching.

Tab discovery uses the `/htmlview` endpoint to find tab names and GIDs, then fetches additional tabs in parallel.

## Contributing

Single HTML file — no build step, no package manager. Edit `index.html` and refresh.

1. Fork the repo
2. Make your changes to `index.html`
3. Test with a real public Google Sheet
4. Submit a pull request

Read `CLAUDE.md` for detailed architecture docs (position maps for both template versions, multi-tab architecture, CSV parsing, output format spec, spell preparation handling).

### Adding a New Template

To support a new sheet template, create a new position map object following the pattern in the code. Each map defines [row, col] coordinates for every field. See the v4.0 and v1.4 position maps in `CLAUDE.md` for reference.

## License

[MIT License](LICENSE) — Copyright (c) 2026 PromptFerret

## Links

- [D&D 5e GSheet v1.4](https://docs.google.com/spreadsheets/d/1etrBJ0qCDXACovYHUM4XvjE0erndThwRLcUQzX6ts8w/edit?gid=1750226729#gid=1750226729) — compatible public-domain character sheet template
- [All PromptFerret Tools](https://promptferret.github.io/tools/)
- [PromptFerret](https://promptferret.github.io)
