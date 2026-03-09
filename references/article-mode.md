# Article Mode

Use this mode when the user wants a complete publish-ready article.

## Output Shape

- Write a `.docx` file in the current workspace
- Return only a short confirmation containing the absolute output path
- Prefer `output_filename` when provided; otherwise slugify the chosen title and append `.docx`

## File Workflow

1. Draft the article in Markdown.
2. Use `python3 scripts/markdown_to_docx.py --input <markdown> --output <docx> --title "<chosen title>" --description "<meta description>"`.
3. Confirm that the `.docx` file exists.
4. Return only the absolute file path or a short confirmation line with that path.

## Markdown Rules

- Start with one `#` heading that matches the chosen title
- Use at least three `##` headings
- Use `###` only where structure benefits from it
- End with a conclusion section and an FAQ section
- Keep links as standard Markdown links so the converter can preserve the URL text

## Content Rules

- Write more than 1500 words unless instructed otherwise
- Mention the main keyword in the H1, opening paragraph, and closing section
- Add 2 to 5 relevant internal or recommended links when available
- Mention `site_domain` naturally near the opening and ending if provided
- Avoid listed `exclude_brands`
- Use clear paragraphs and simple lists that will survive Word conversion cleanly
