# Output Contract

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
- If progress updates are emitted, they must be separate intermediate messages and must not appear inside the JSON

## Article Mode

Write a `.docx` file and return only the file path or a short confirmation line containing the absolute file path.

Rules:

- The path must be absolute
- The file extension must be `.docx`
- Do not return the article body inline
- Do not wrap the path in JSON unless the user explicitly requests JSON packaging
- If progress updates are emitted, they must be separate intermediate messages and must not appear in the final line containing the path

## Progress Message Contract

When the runtime supports intermediate status messages, use this exact one-line format:

`[Progress current/total] label`

Rules:

- Progress messages are optional
- Progress messages must be sent before the final output, never after it
- Progress messages must not include article content, JSON payloads, or the final `.docx` path
- If separate intermediate messages are not supported, emit no progress messages

## Article Details

- Keep the article above 1500 words unless the user asks for shorter output
- Use 2 to 5 internal links when suitable
- If links are unavailable, omit them rather than invent broken URLs
- Draft the article in Markdown and convert it with `scripts/markdown_to_docx.py`
