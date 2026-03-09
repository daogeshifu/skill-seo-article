---
name: seo-article
description: Generate SEO outlines or full long-form articles for independent websites from a keyword, brand, and optional references. Use when the user asks for H2/H3 planning, blog writing, or Word document output for content publishing.
user-invocable: true
---

# SEO Article

Produce one SEO deliverable per request. Infer the mode unless the user explicitly sets it. Ask a follow-up only when the primary keyword is missing.

## Load References

- Read [references/output-contract.md](references/output-contract.md) before finalizing the response.
- Read [references/quality-gate.md](references/quality-gate.md) before returning the response.
- Read [references/outline-mode.md](references/outline-mode.md) only for `outline` mode.
- Read [references/article-mode.md](references/article-mode.md) only for `article` mode.

## Inputs

Accept either free text or structured key-value input.

Supported fields:

- `mode` or `模式` (optional: `outline` or `article`)
- `keyword` or `关键词` (required)
- `brand` or `品牌` (optional)
- `topic` or `主题` (optional)
- `language` or `语言` (optional)
- `target_country` or `目标地区` (optional)
- `audience` or `受众` (optional)
- `reference_urls` or `参考链接` (optional list)
- `site_domain` or `站点域名` (optional)
- `internal_pages` or `内链页面` (optional list)
- `recommend_links` or `推荐链接` (optional list)
- `link_form` or `链接形式` (optional)
- `exclude_brands` or `排除品牌` (optional list)
- `search_intent` or `搜索意图` (optional)
- `output_filename` or `输出文件名` (optional, article mode only)

Defaults:

- `mode`: infer from the request; use `outline` when the user asks for a structure or outline, otherwise use `article`
- `brand`: `none`
- `topic`: a practical article angle centered on the keyword
- `language`: match the user's language
- `target_country`: `global`
- `audience`: inferred from keyword intent
- `search_intent`: inferred from the keyword if not provided
- `link_form`: infer from user context; prefer natural inline anchors
- `exclude_brands`: empty
- `output_filename`: slugified from the chosen title with `.docx`

If `keyword` is missing, ask for it and stop.

## Workflow

Work in one pass:

1. Infer intent and audience.
2. Build an outline and long-tail keyword set.
3. Branch by `mode`.
4. Self-check against the quality gate.
5. Revise up to two times if needed.
6. Write the final deliverable in the required shape.

Keep the brief and checklist internal unless the user explicitly asks to see them.

## Progress Updates

If the runtime supports separate intermediate status messages, emit progress updates during execution. If it does not, skip progress updates entirely rather than mixing them into the final output.

Rules:

- Progress updates must be sent as standalone intermediate messages, never inside the final result.
- Use this exact format: `[Progress current/total] label`
- Keep each progress message to one short line.
- Never include JSON payloads, article body content, Markdown draft, or the final `.docx` path in a progress message.
- The final message must still follow the active mode contract exactly.

Use these stage labels in `article` mode:

1. `[Progress 1/8] Parsing inputs`
2. `[Progress 2/8] Inferring search intent`
3. `[Progress 3/8] Building outline`
4. `[Progress 4/8] Drafting article`
5. `[Progress 5/8] Reviewing SEO quality`
6. `[Progress 6/8] Preparing output file`
7. `[Progress 7/8] Exporting Word document`
8. `[Progress 8/8] Verifying output`

Use these stage labels in `outline` mode:

1. `[Progress 1/5] Parsing inputs`
2. `[Progress 2/5] Inferring search intent`
3. `[Progress 3/5] Building outline`
4. `[Progress 4/5] Reviewing outline quality`
5. `[Progress 5/5] Packaging result`

## Planning Rules

Before writing, derive:

- primary search intent
- target audience and pain points
- one H1 direction
- 3-6 H2 sections
- H3 opportunities where useful
- at least 5 long-tail keywords
- a brand angle if a brand is provided

If the topic is broader than the keyword, anchor the article around the keyword and use the topic as the promise or framing.

## Mode Selection

Use `outline` mode when the user asks for:

- an outline
- a structure
- headings only
- H2/H3 planning
- content brief scaffolding

Use `article` mode when the user asks for:

- a blog post
- a full article
- a Word file
- publish-ready content
- a complete SEO article

## Shared Rules

- Keep planning notes, checklists, and revision notes internal.
- Do not invent citations, rankings, statistics, links, or image URLs.
- Exclude or avoid promoting brands listed in `exclude_brands`.
- In `outline` mode, return JSON only.
- In `article` mode, write a `.docx` file to the current workspace and return the absolute output path.
- Progress updates are optional intermediate messages only; they must never be merged into the final output.

## Article Rules

In `article` mode:

- Draft the article in Markdown first, then convert it to `.docx` with `python3 scripts/markdown_to_docx.py`.
- Use the chosen SEO title as the document title and generate a concise meta description for the document subtitle.
- Write the final file to `output_filename` if provided; otherwise slugify the chosen title and append `.docx`.
- Keep the intermediate Markdown in a temporary file only if needed for the conversion step.
- Keep total length above 1500 words unless the user explicitly asks for a shorter article.
- Use exactly one H1 and include the primary keyword in it.
- Mention the primary keyword in the first paragraph and closing section.
- Include the primary keyword in at least one H2.
- Use at least 3 H2 headings.
- Use H3 only when they clarify structure.
- Include an introduction, body, conclusion, and FAQ.
- Integrate the brand naturally. If `brand` is `none`, keep the article neutral.
- If `site_domain` is provided, mention it naturally near the beginning and the ending.
- Prefer practical explanation, comparison, step-by-step guidance, or buyer education based on intent.
- When adding links, use `recommend_links` first if provided and respect `link_form`.
- Use Markdown structure that converts cleanly to Word: `#` for the title, `##` for major sections, `###` for subsections, normal paragraphs for body copy, and simple bullet lists where useful.
- After writing the `.docx`, return a short confirmation with the absolute file path and nothing else.

## Outline Rules

In `outline` mode:

- Output JSON only, with no code fences and no prose outside the JSON object.
- Use exactly two top-level keys: `title` and `result`.
- `title` must contain 3 SEO-friendly title options.
- `result` must be a single string containing only `H2:` and `H3:` lines separated by `\n`.
- Do not use markdown headings like `##` or `###`.
- Do not output paragraphs, notes, numbering, or explanations outside heading lines.
- Always include `H2: Conclusion`.
- Always include `H2: FAQs` followed by 3 to 5 `H3:` question lines.
- Never generate H4 or deeper headings.

Apply adaptive structure:

- For `vs`, `compare`, or `difference` keywords: include what each side is, key differences, and how to choose.
- For `types`, `methods`, `ways`, or `categories`: include a type or method section with 3 to 5 H3 items.
- For `cost`, `price`, `budget`, or `how much`: include cost drivers, alternatives, and hidden costs.
- For `install`, `setup`, `how to`, or `steps`: include preparation, steps, mistakes, and safety.
- For `inspection`, `frequency`, `standard`, `OSHA`, or `ASME`: include types, frequency, standards, who can perform the work, and checklist or process.

## Self-Check

Review the draft or outline before returning it.

- Hard fail in `outline` mode if the output contains anything other than `H2:` and `H3:` lines in `result`.
- Hard fail in `outline` mode if `Conclusion` or `FAQs` is missing.
- Hard fail in `article` mode if no `.docx` file was written.
- Hard fail in `article` mode if the returned message does not include the absolute `.docx` path.
- Hard fail in any mode if a progress line is included inside the final output.
- Hard fail in `article` mode if H1 does not contain the keyword.
- Hard fail in `article` mode if the keyword is absent from the introduction.
- Hard fail in `article` mode if there are fewer than 3 H2 headings.
- Hard fail in `article` mode if word count is below 1500 unless a shorter article was explicitly requested.
- Hard fail in `article` mode if keyword density is below 0.8% or above 3%.
- Soft warn if keyword density is outside the 1%-2% target.
- Soft warn if the conclusion is weak or missing.
- Soft warn if paragraphs are too dense or the article reads as keyword-stuffed.

Revise up to two times. Return only the final version, not the review notes.

## Trigger Examples

Use this skill for requests like:

- "给我一篇关键词+品牌的谷歌 SEO 文章"
- "给我一个只包含 H2/H3 的 SEO 大纲"
- "Write an SEO article for my brand from this keyword"
- "帮我生成独立站博客，并写成 Word 文件"
- "根据关键词和参考链接生成 SEO outline"
