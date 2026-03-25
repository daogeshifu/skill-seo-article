# SEO Article

Professional SEO content generation skill for ClawHub / OpenClaw. It turns a keyword-focused prompt into either a production-ready SEO outline or a long-form article exported as a Word `.docx` file.

Official site: [idtcpack.com](https://www.idtcpack.com/)

## Overview

`seo-article` is designed for teams that need repeatable SEO content output instead of loose LLM prose. The skill enforces a structured workflow:

- normalize user input
- infer search intent and audience
- build SEO-aware titles and headings
- branch into outline or article mode
- run quality checks before returning output
- export article mode to a real Word document

This makes it useful for independent sites, content agencies, affiliate projects, niche blogs, and internal publishing pipelines.

## Features

- Dual output modes: structured SEO outline or full article
- Word export in `article` mode with local `.docx` generation
- Strict output contracts for stable downstream automation
- SEO-aware heading planning with H2/H3 enforcement
- Built-in quality gate for structure, density, and completeness
- Optional progress updates without contaminating final output
- Supports brand, audience, target market, internal links, and exclusion rules

## Modes

### `outline`

Returns JSON only:

```json
{"title":["Title 1","Title 2","Title 3"],"result":"H2: ...\nH3: ..."}
```

Rules:

- exactly 3 SEO title options
- `result` contains only `H2:` and `H3:` lines
- always includes `Conclusion`
- always includes `FAQs` with 3 to 5 question lines

### `article`

Writes a `.docx` file and returns the absolute file path or a short confirmation containing the file path.

Rules:

- article is drafted in Markdown first
- Markdown is converted locally to `.docx`
- final response does not inline the article body
- progress updates, if emitted, stay separate from the final output

## Supported Input

The skill accepts either free-form prompts or structured fields.

Common fields:

- `mode`
- `keyword`
- `brand`
- `topic`
- `language`
- `target_country`
- `audience`
- `reference_urls`
- `site_domain`
- `internal_pages`
- `recommend_links`
- `link_form`
- `exclude_brands`
- `search_intent`
- `output_filename`

If `keyword` is missing, execution should stop and ask for it.

## How It Works

The runtime flow is:

1. Parse and normalize inputs.
2. Infer search intent, audience, and article angle.
3. Build title candidates, outline structure, and long-tail keyword coverage.
4. Generate either an outline or a long-form article.
5. Run hard and soft quality checks.
6. Export article mode to `.docx`.
7. Return the final payload in the required format.

## Progress Updates

When the runtime supports separate intermediate messages, the skill can emit progress lines such as:

```text
[Progress 1/8] Parsing inputs
[Progress 2/8] Inferring search intent
[Progress 3/8] Building outline
```

Important:

- progress messages are optional
- progress messages must be separate from the final output
- final `outline` output must remain pure JSON
- final `article` output must remain only the file path or short confirmation

## Word Export

Article mode uses the local script [scripts/markdown_to_docx.py](scripts/markdown_to_docx.py) to generate a minimal Word document package.

The exporter:

- parses Markdown headings, paragraphs, and lists
- maps them into WordprocessingML styles
- writes the required OOXML files
- zips them into a valid `.docx`

This keeps article export deterministic and avoids asking the model to fabricate document binaries.

## Project Structure

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── scripts/
│   └── markdown_to_docx.py
└── references/
    ├── article-mode.md
    ├── outline-mode.md
    ├── output-contract.md
    └── quality-gate.md
```

## Quick Start

### Example: outline mode

```text
mode: outline
keyword: wireless noise cancelling headphones
brand: SoundMax
language: English
target_country: US
```

Expected result:

- JSON with 3 title candidates
- `result` containing only `H2:` and `H3:` lines

### Example: article mode

```text
mode: article
keyword: warehouse barcode scanner
brand: IDT Pack
site_domain: idtcpack.com
language: English
output_filename: warehouse-barcode-scanner-guide.docx
```

Expected result:

- a long-form SEO article
- a generated Word file in the working directory
- final response with the absolute `.docx` path

## Installation

Use this repository as a ClawHub / OpenClaw skill package.

If you want to publish or consume it as a package:

```bash
npm pack
```

If you are loading it directly as a local skill, keep the current folder structure intact so `SKILL.md`, `references/`, and `scripts/` remain discoverable.

## Output Guarantees

The skill is opinionated about output correctness.

- outline mode is contract-first and machine-friendly
- article mode must produce a real `.docx` file
- hard failures prevent invalid final output
- progress updates must never pollute final payloads

See:

- [SKILL.md](SKILL.md)
- [references/output-contract.md](references/output-contract.md)
- [references/quality-gate.md](references/quality-gate.md)

## Use Cases

- SEO agencies generating client deliverables
- content teams producing briefs and publish-ready drafts
- internal tooling that needs predictable content JSON
- workflows that require Word export instead of raw HTML
- AI-assisted pipelines for niche site publishing

## Contributing

Contributions should preserve three things:

- stable output contracts
- deterministic `.docx` export behavior
- separation between intermediate progress updates and final output

If you change behavior, update the relevant files in `references/` and keep `SKILL.md` aligned.

## License

MIT. See [LICENSE](LICENSE).
