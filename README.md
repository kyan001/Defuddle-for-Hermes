# Defuddle for Hermes (AIGC)
Local Web Extraction Provider for [Hermes Agent](https://hermes-agent.nousresearch.com/) web extraction via [Defuddle](https://github.com/kepano/defuddle), no API key required.

Drops in as a web extract provider plugin. Designed to pair with a search-only backend (SearXNG, Brave Free, DDGS, etc.) so `web_extract()` works transparently.

Extracts clean json from web pages using the Defuddle CLI.

## Quick Start

```Bash
hermes plugins install kyan001/Defuddle-for-Hermes
```

Then set it as your extract backend in `${HERMES_HOME}/config.yaml`:

```YAML
web:
  search_backend: searxng
  extract_backend: defuddle  # set this
```

`/restart` / `/new` and `web_extract(urls=[...])` will use Defuddle under the hood.

## Requirements

- Node.js with npm/npx
- Defuddle CLI is fetched automatically via `npx` on first use

## How It Works

The plugin registers as a `WebSearchProvider` with:

| Property | Value |
|----------|-------|
| `supports_search()` | `False` |
| `supports_extract()` | `True` |
| `extract(urls)` | Runs `npx defuddle parse <url> --json`, returns clean markdown |

## Response Envelope

Each URL returns:

```JSON
{
  "url": "https://example.com/article",
  "title": "Page Title",
  "content": "# Markdown content…",
  "raw_content": "# Markdown content…",
  "metadata": {
    "description": "Meta description",
    "domain": "example.com",
    "word_count": 1234,
    "language": "en",
    "author": "Author Name"
  },
  "error": null
}
```

## File Structure

```Shell
Defuddle-for-Hermes/
├── __init__.py    # DefuddleExtractProvider + register()
└── plugin.yaml    # Plugin manifest
```
