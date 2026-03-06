# seo-article

`seo-article` is a ClawHub/OpenClaw skill for generating either a structured SEO outline or a full Google-friendly SEO article for independent websites.

## Included files

- `SKILL.md`: skill trigger metadata and workflow instructions
- `agents/openai.yaml`: optional UI metadata for OpenAI-compatible surfaces
- `references/output-contract.md`: final output contract
- `references/quality-gate.md`: self-review rules
- `references/outline-mode.md`: outline generation rules
- `references/article-mode.md`: article generation rules

## Expected input

The skill accepts free text or structured fields such as:

```text
mode: article
keyword: wireless noise cancelling headphones
brand: SoundMax
topic: best wireless noise cancelling headphones for daily commuting
language: English
site_domain: soundmax.com
recommend_links:
  - https://soundmax.com/products/noise-cancelling-headphones
```

## Output

The skill returns:

- outline mode: JSON with `title` and `result`, where `result` contains only `H2:` and `H3:` lines
- article mode: JSON with `title` and `result`, where `result` contains pure HTML
