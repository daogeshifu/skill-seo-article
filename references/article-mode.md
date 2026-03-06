# Article Mode

Use this mode when the user wants a complete publish-ready article.

## Output Shape

- Return JSON only
- Use `title` for 3 SEO-friendly title options
- Use `result` for pure HTML

## HTML Rules

- Start with `<title>` and `<meta name="description">`
- Wrap content in semantic HTML when practical, usually `<article>`
- Use exactly one `<h1>`
- Use at least three `<h2>`
- Use `<h3>` only where structure benefits from it
- End with a conclusion section and FAQ section

## Content Rules

- Write more than 1500 words unless instructed otherwise
- Mention the main keyword in the H1, opening paragraph, and closing section
- Add 2 to 5 relevant internal or recommended links when available
- Mention `site_domain` naturally near the opening and ending if provided
- Avoid listed `exclude_brands`

## Image Rules

- Only use real `http` or `https` image URLs
- Do not use base64, local paths, or placeholder domains
- If real image URLs cannot be verified, omit images
