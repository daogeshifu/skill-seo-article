# seo-article

`seo-article` is a ClawHub/OpenClaw skill for generating either a structured SEO outline or a full Google-friendly SEO article for independent websites. In `article` mode it now writes a Word `.docx` file instead of returning HTML JSON.

## Included files

- `SKILL.md`: skill trigger metadata and workflow instructions
- `agents/openai.yaml`: optional UI metadata for OpenAI-compatible surfaces
- `scripts/markdown_to_docx.py`: local converter from Markdown article draft to `.docx`
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
- article mode: a `.docx` file written to the workspace, plus a short confirmation containing its absolute path

If the runtime supports intermediate status messages, the skill may also emit one-line progress updates before the final result. Those updates are separate from the final payload and must not be mixed into it.
