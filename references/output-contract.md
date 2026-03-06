# Output Contract

Return JSON only. Do not wrap in code fences. Do not prepend commentary.

## Outline Mode

Use exactly this shape:

```json
{"title":["Title 1","Title 2","Title 3"],"result":"H2: ...\nH3: ..."}
```

Rules:

- Only two keys: `title` and `result`
- `title` contains exactly 3 SEO-friendly titles
- `result` contains only `H2:` and `H3:` lines
- No markdown `##` or `###`
- No prose outside the JSON object

## Article Mode

Use exactly this shape:

```json
{"title":["Title 1","Title 2","Title 3"],"result":"<title>...</title><meta name=\"description\" content=\"...\" /><article>...</article>"}
```

Rules:

- Only two keys: `title` and `result`
- `title` contains exactly 3 SEO-friendly titles
- `result` contains pure HTML
- `result` must include:
  - one `<title>`
  - one `<meta name="description">`
  - one `<h1>`
  - at least three `<h2>`
  - FAQ markup near the end
- No markdown, no code fences, no extra commentary

## Article Details

- Keep the article above 1500 words unless the user asks for shorter output
- Use 2 to 5 internal links when suitable
- If links are unavailable, omit them rather than invent broken URLs
- If images cannot be verified, omit them rather than invent fake image URLs
